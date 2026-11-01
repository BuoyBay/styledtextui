---
title: "Using the native styled-text editor"
summary: "Attach and open the control, format selections and typing, and run the complete editor demo."
category: "Guide"
order: 10
symbols: ["StyledTextEditor", "Open", "SetDocument", "ApplySelectionStyle"]
keywords: ["rich text", "native", "editor", "example"]
related: ["index.md", "api.md", "typed-api.md"]
---

# Getting started

Install both styledtext and styledtextui packages in your project's packages directory. Create a StyledTextEditor, set geometry with Move, add it to the window, and only then call Open. RefreshNative updates the embedded control after geometry changes. All calls belong to the UI thread.

SetDocument takes a validated copy of a portable StyledText. Select uses zero-based Unicode scalar offsets. ApplySelectionStyle replaces all selected attributes, or sets the typing attributes at an empty selection. The SetSelectionBold/Italic/Underline methods preserve other formatting in mixed selections. SetTypingStyle and ReplaceSelection provide explicit typing behavior. Document returns an independent snapshot for EncodeStyledText and file saving.

The runnable `examples/styledtext-editor/main.bui` includes toolbar controls, native Save/Open dialogs, window resizing, and Ctrl/Command formatting and undo shortcuts. It saves formatting with BSTX rather than RTF.

Call Close before disposing of the control; it is idempotent and the document can be reopened. Events may safely inspect the document or close the editor. Exceptions in native event handlers are recorded in LastNativeError. Native resources closed inside a callback are released after the callback returns.

See [API](api.md) for limits, history and platform validation status, or return to the [overview](index.md).

Pair editor Open with Close in the host Window’s Closing event, before native views are destroyed. Reopening requires the original host View to remain alive and attached.
