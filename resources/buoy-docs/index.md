---
title: "StyledTextUI"
summary: "Native rich-text editing with portable StyledText documents on GTK, AppKit, and Windows Rich Edit."
category: "Overview"
order: 1
symbols: ["StyledTextEditor"]
keywords: ["rich text", "editor", "styledtext", "selection", "typing"]
related: ["getting-started.md", "api.md", "typed-api.md"]
---

# StyledTextUI

`StyledTextEditor Inherits View` is a native rich-text editor backed by the separate styledtext package. It supports mixed style runs, scalar selection, typing attributes, and formatting-preserving undo/redo. Its portable model retains exact attributes even when native drawing rounds them.

See [getting started](getting-started.md), [API contracts](api.md), and the [typed API](typed-api.md). Native execution is verified on Linux x86_64 and Windows x86_64; AppKit and the other architectures are compiler-checked. This is a package control rather than a change to built-in TextArea.
