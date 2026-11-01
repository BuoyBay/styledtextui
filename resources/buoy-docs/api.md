---
title: "StyledTextUI API contracts"
summary: "Selection, typing, history, native lifecycle, resource bounds, and platform support contracts."
category: "Reference"
order: 20
symbols: ["StyledTextEditor", "Document", "SetDocument", "Select", "TypingStyle", "ReplaceSelection", "ApplySelectionStyle", "SetSelectionBold", "SetSelectionItalic", "SetSelectionUnderline", "Undo", "Redo", "DocumentChanged", "SelectionChanged"]
keywords: ["styledtext", "native", "selection", "history", "typing"]
related: ["index.md", "getting-started.md", "typed-api.md"]
---

# API contracts

## Document and editing

| API | Contract |
| --- | --- |
| `StyledTextEditor Inherits View` | Native GTK/AppKit/Rich Edit control, UI thread only. Attach before Open. |
| `Document() As StyledText` | Detached portable snapshot. |
| `SetDocument(document)` | Validate and copy input; reset selection and history; initialize typing from first run or empty default; raise DocumentChanged. Nil rejected. |
| `Text As String` | Plain content; writing clears run formatting/history and retains document default. |
| `CharacterCount As Integer` | Scalar count without copying the document. |
| `Select(start, length)` | Validate zero-based scalar range, update native selection and typing style, raise SelectionChanged. |
| `SelectionStart`, `SelectionLength`, `SelectedText()` | Cached selection synchronized with native selection notifications; offsets are scalars. |
| `TypingStyle()` | Copied attributes for subsequent insertion. |
| `SetTypingStyle(style)` | Validate/copy explicit typing attributes; no event or history entry. Nil rejected. |
| `ApplySelectionStyle(style)` | Full range attribute replacement; at a caret, changes typing attributes. History entry and DocumentChanged. Nil rejected. |
| `SetSelectionBold/Italic/Underline(value)` | Patch only the named attribute across mixed runs; at a caret, patch typing attributes. History entry and DocumentChanged. |
| `ReplaceSelection(content)` | Replace with typing attributes; retain outside styles; collapse to end; history and both change events. |
| `Undo()`, `Redo()` | Restore exact document, selection, and typing attributes. No-op if unavailable or read-only; raise both events after commit. |
| `CanUndo`, `CanRedo`, `ClearHistory()` | Inspect or clear bounded model history. |
| `IsReadOnly` | Prevent native user edits and format/replacement/history commands. Programmatic SetDocument/selection remain available. |

Plain caret inheritance uses the left scalar, the right scalar at zero, or the default when empty. Selecting text derives typing from its first scalar. Explicit typing styles are retained by inserted runs. The portable document default survives deletion, but the editor can continue typing with its current explicit attributes. Positions are neither grapheme clusters nor UTF-16 units. Native input ranges are captured before edits so repeated text does not make formatting drift to a different position.

History uses portable binary snapshots, bounded to 100 entries and 8 MB across undo and redo. Native contiguous typing and same-direction deletion coalesce until whitespace, a caret move, a style command or an explicit history operation. Selection replacement, including GTK’s split delete/insert actions, is one undo entry. GTK begin/end-user-action notifications also group compound actions. Oldest entries are discarded at the bound. Native text-only undo is disabled; use the model Undo/Redo API or handled shortcuts so formatting is preserved.

## Native lifecycle and events

Open after attaching, RefreshNative after changing View geometry, FocusEditor for input focus, and Close when done. IsOpen reports readiness. Close is idempotent and preserves document/history. Destructor closes on the main thread. Event-handler close is deferred until native callbacks unwind; registry slots remain reserved until resources are released. At most 256 open or awaiting-teardown controls are permitted.

DocumentChanged fires after text edits, format commands including caret formatting, and SetDocument. SelectionChanged fires for explicit Select/replacement/history commands and native selection movement. SetTypingStyle alone is silent. Direct API exceptions propagate; exceptions in native callbacks and user handlers are contained in LastNativeError. A handler exception does not roll back a committed edit. Open clears the recorded error.

TextHandle and BufferHandle are borrowed diagnostics handles. Do not destroy/reparent native objects, replace buffers/delegates, or apply attributes through them. The Native-prefixed public methods exist solely for the package's FFI trampolines and are not application commands.

## Limits and display behavior

Accepted documents have at most 100,000 scalars and 4096 normalized runs; names fit 31 UTF-16 units. Font size is zero or 1–512 points; zero displays at 12 points. Text is valid NUL-free UTF-8 with LF newlines. Invalid candidate documents are rejected before public commands mutate the model. GTK/AppKit preflight native replacements; other failed native input is restored. Native selection ranges that split surrogate pairs are rejected.

Exact portable colors/sizes remain unchanged by display. GTK uses Pango sizes and RGBA, AppKit uses native fonts/attributes, and Windows displays twips/RGB8. Missing font families may substitute. All edits copy and reapply the bounded run model; this is for small and medium documents.

Ctrl+B/I/U, Ctrl+Z/Y and Ctrl+Shift+Z are handled on Linux/Windows. macOS uses Command. Ordinary native text copy/paste remains available; Windows requests Unicode plain text. Rich clipboard styles, RTF import/export, paragraph layout, attachments, bidirectional selection orientation preservation, and native edit-menu integration are outside this version. Save portable documents through the styledtext codec.

Linux x86_64 and Windows x86_64 native execution is verified. All six targets compiler-check; AppKit and remaining architectures still require native execution. Run tests/styledtextui_verify.py on the Linux host; --windows-lab uses the already-provisioned local Windows lab in a separate SSH session. See the package README for JPEG runtime setup and the known upstream View focus-signal diagnostic.

[Getting started](getting-started.md) · [Typed API](typed-api.md) · [Overview](index.md)

The document canvas is white with a black caret/default foreground on every backend, independent of application theme. Explicit portable text colors retain their meaning in dark-mode applications.

Native teardown uses the platform run loop (GLib idle, Win32 timer, AppKit delayed selector), with identities retained until disposal. The standalone build keeps its runtime libraries in dist/styledtext-editor-app, separate from other applications.
