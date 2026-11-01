# StyledTextUI · 0.1.0

A **native rich-text editor** for Buoy. `StyledTextEditor Inherits View` embeds a GtkTextView on Linux, an NSTextView on macOS, or a Windows Rich Edit 4.1 (`RICHEDIT50W`) control. It edits portable [StyledText](https://github.com/BuoyBay/styledtext/blob/main/docs/API.md) documents without flattening their formatting.

The editor supports mixed fonts and sizes, bold, italic, underline, foreground color, scalar-based selection, typing attributes, and formatting-preserving undo/redo. It keeps the exact portable attributes as its source of truth; native display rounding (Windows twips and RGB8, for example) never rewrites saved RGB16 colors or fractional sizes. Available native fonts may substitute missing families.

## Run the editor

Run `python3 tools/build_styledtextui.py` to seal both packages and build the demo. Alternatively, install both `styledtext` and `styledtextui` sealed packages in the project's packages directory, then build [the example](../examples/styledtext-editor/main.bui):

```
buoy examples/styledtext-editor/main.bui -o dist/styledtext-editor
./dist/styledtext-editor
```

The demo has Bold, Italic, Underline, Undo, Redo, Save and Open buttons. Save/Open use native file pickers and the formatting-preserving BSTX format. They do not import RTF. Ctrl+B/I/U and Ctrl+Z/Y (Ctrl+Shift+Z also works) are handled by the editor; macOS uses Command in place of Control. The native control supplies text input, mouse selection, wrapping, scrolling and ordinary text copy/paste. Windows paste explicitly requests Unicode plain text; the package does not promise rich clipboard interchange.

On this Debian development host the installed Buoy GUI runtime additionally needs JPEG 8. Use the existing compatibility directory with `LD_LIBRARY_PATH=/tmp/buoy-chatui-jpeg8/build` if the loader reports missing `libjpeg.so.8`, or run in the project's Ubuntu GUI environment. This is an installed runtime dependency, not a text-editor dependency.

## Embed it

Create the editor, call `Move`, add it to a Window/View, and call `Open` **after attaching** it. Windows creates the View's native handle lazily. `SetDocument` validates and copies its input, resets selection/history, and selects the first character's style for typing (or the document default when empty). `Document()` returns a detached snapshot. `Text` reads plain content; setting it deliberately clears run formatting and history while retaining the document default. `CharacterCount` avoids allocating a document snapshot for counters.

`Select(start, length)` uses zero-based Unicode scalar counts, including for emoji. It updates native selection and derives typing attributes from the first selected scalar, the left scalar for a caret, the right scalar at zero, or the document default when empty. `ApplySelectionStyle` fully replaces all selected attributes; at a caret it changes typing attributes. `SetSelectionBold/Italic/Underline` change only that attribute and preserve other attributes across mixed selections. `SetTypingStyle` explicitly controls subsequent insertion. `ReplaceSelection` replaces text using those attributes. `Undo` and `Redo` preserve text, formatting, selection and typing attributes; GTK user actions that delete then insert are grouped into one history entry.

`IsReadOnly` prevents user edits and formatting/replacement/history commands; programmatic `SetDocument` and selection changes remain available. `CanUndo/CanRedo` describe history availability. `DocumentChanged` fires after text, formatting or typing-format commands that create history, and after `SetDocument`. `SelectionChanged` fires for explicit `Select`/replacement/history commands and native selection movement. `SetTypingStyle` alone is silent. Native callback exceptions are contained in `LastNativeError`; ordinary direct API errors propagate. This diagnostic retains the latest reported error across successful commands and `Close`; consumers can clear it by assigning an empty string. A successful `Open` that creates the native control clears it, while calling `Open` on an already-open editor leaves it unchanged. Event handlers may inspect the document or close the editor; native teardown is deferred until native callbacks unwind.

Use the control only on the UI thread. Call `RefreshNative` after changing its View geometry (the example does this in the Window resize event). `FocusEditor` restores keyboard focus after toolbar actions. `Close` is idempotent and preserves the document/history for reopening; `Destructor` also closes. Native handles are borrowed diagnostic handles: do not destroy or reparent them, replace buffers/delegates, or manipulate rich attributes through them. Native text edits are reconciled through the model; arbitrary formatting changes made outside this API are unsupported. A maximum of 256 simultaneously open or awaiting-teardown editors bounds the callback registry.

## Bounds and semantics

UI documents allow 100,000 scalars, 4096 normalized runs, and font family names of at most 31 UTF-16 units. Point size is zero (display default of 12 points) or 1–512 points. Text must be valid NUL-free UTF-8 and use LF newlines. The portable model permits larger documents and sizes; `SetDocument` rejects documents outside these UI bounds without replacing the current document. Windows and macOS selections translate between UTF-16 units and scalars; surrogate-interior selections are rejected. These are scalar positions, not grapheme clusters. Paragraph styles, attachments, rich clipboard import, bidirectional caret orientation preservation, and native menu-command integration are outside this version.

The editor's history is bounded to 100 snapshots and 8 MB across both undo and redo. Oldest entries are discarded when either limit is reached. Native CRLF and CR input is normalized to LF; programmatic document input requires LF. Contiguous native typing and same-direction deletion coalesce until whitespace, a caret move, style change or explicit command. Every edit validates/copies the portable document and reapplies native style runs; this is intended for small and medium documents, not a large-document rope editor. All text/format commands preflight their candidate state. Native input that fails validation is rejected or restored to the previous model. Native event handlers can report their own errors without undoing an already committed edit.

## Verification and platform status


The verifier seals both packages into an isolated project and checks the module, test and demo with the real compiler on all six targets, and verifies test/demo LLVM generation on each target. On Linux it executes a real GTK window and independently reads native tag properties, then sends actual X11 keyboard events to the running demo through XTest. Tests cover Unicode and repeated-character edits, mixed formatting, grouped undo, typing attributes, read-only input, snapshots, exception containment, close-in-handler, deferred teardown and reopening. Only the installed Buoy 0.72 View constructor's known obsolete `focus-out` signal diagnostic is tolerated; other native diagnostics fail verification.

`--macos-host` transfers freshly sealed packages and the test into a unique temporary project on an already-configured SSH host, compiler-checks/builds/runs it there, requires both completion markers, and removes the temporary project. `--macos-compiler` selects the installed compiler. `--report` records successful checks, native scope and source/package fingerprints in JSON.

`--windows-lab` additionally cross-compiles and executes native Rich Edit tests in the already-provisioned local Windows VM's separate SSH session. It verifies native bold/twips, Unicode selection/replacement, undo and repeated-character styling, requires a completion marker, and cleans up its unique guest directory. It does not operate the shared console or install/start a VM.

Native execution is verified on Linux x86_64, Windows x86_64 and macOS ARM64. The remaining architectures are type-checked and LLVM-generated. Linking AppKit on this Linux host also needs a complete Apple SDK, which is not installed. The package is a library control, not a patch to Buoy's built-in TextArea or its Xojo shim.

The implementation follows the platform APIs: [GTK text buffers](https://docs.gtk.org/gtk4/class.TextBuffer.html), [NSTextView](https://developer.apple.com/documentation/appkit/nstextview), and [Windows rich-edit character formatting](https://learn.microsoft.com/en-us/windows/win32/controls/em-setcharformat).

The document canvas is white with a black caret/default foreground on every backend, independent of application theme. Explicit portable text colors retain their meaning in dark-mode applications.

Pair editor Open with Close in the host Window’s Closing event, before native views are destroyed. Reopening requires the original host View to remain alive and attached.

Native teardown uses the platform run loop (GLib idle, Win32 timer, AppKit delayed selector), with identities retained until disposal. The standalone build keeps its runtime libraries in dist/styledtext-editor-app, separate from other applications.

Test suites and fixtures are maintainer-local and are not part of this public repository.
