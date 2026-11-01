# Embed a native rich-text editor over StyledText documents

A **native rich-text editor** for Buoy. `StyledTextEditor Inherits View` embeds a GtkTextView on Linux, an NSTextView on macOS, or a Windows Rich Edit 4.1 (`RICHEDIT50W`) control. It edits portable [StyledText](docs/API.md) documents without flattening their formatting.

The editor supports mixed fonts and sizes, bold, italic, underline, foreground color, scalar-based selection, typing attributes, and formatting-preserving undo/redo. It keeps the exact portable attributes as its source of truth; native display rounding (Windows twips and RGB8, for example) never rewrites saved RGB16 colors or fractional sizes. Available native fonts may substitute missing families.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add styledtextui --git https://github.com/BuoyBay/styledtextui.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.
BPM also resolves `styledtext`.

## API at a glance

Create the editor, call `Move`, add it to a Window/View, and call `Open` **after attaching** it. Windows creates the View's native handle lazily. `SetDocument` validates and copies its input, resets selection/history, and selects the first character's style for typing (or the document default when empty). `Document()` returns a detached snapshot. `Text` reads plain content; setting it deliberately clears run formatting and history while retaining the document default. `CharacterCount` avoids allocating a document snapshot for counters.

## Build and API reference

Source is in `modules/`, with native/runtime inputs where required. Use the
[package guide](docs/API.md) for backend-specific build prerequisites, ownership
rules and supported targets. Rebuilding a native backend requires that target's
compiler and SDK; sealing an archive alone does not rebuild the backend.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```

The generated agent files and editor settings stay local and are ignored by Git.

## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
