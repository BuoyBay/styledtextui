---
title: "styledtextui typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["RichGValue", "RichIter", "RichRGBA", "RichRange", "RichRect", "RichSize", "RichSuper", "RichWinHeader", "RichWinRange", "StyledTextEditor"]
dependencies: ["styledtext"]
symbols: ["RichGValue", "RichIter", "RichRGBA", "RichRange", "RichRect", "RichSize", "RichSuper", "RichWinHeader", "RichWinRange", "StyledTextEditor"]
related: ["api.md", "getting-started.md", "index.md"]
---

# styledtextui typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## StyledTextEditor

`Public Class StyledTextEditor Inherits View`

Native rich-text editing. All operations belong to the UI thread; Open after attaching.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `LastNativeError` | `String` | No | No | `""` | — |
| `Text` | `String` | No | No | — | — |
| `CharacterCount` | `Integer` | Yes | No | — | — |
| `IsOpen` | `Boolean` | Yes | No | — | — |
| `TextHandle` | `Ptr` | Yes | No | — | — |
| `BufferHandle` | `Ptr` | Yes | No | — | — |
| `IsReadOnly` | `Boolean` | No | No | — | — |
| `SelectionStart` | `Integer` | Yes | No | — | — |
| `SelectionLength` | `Integer` | Yes | No | — | — |
| `CanUndo` | `Boolean` | Yes | No | — | — |
| `CanRedo` | `Boolean` | Yes | No | — | — |

### Document

`Public Function Document() As StyledText`

No parameters.

#### Returns

| Type |
| --- |
| `StyledText` |

### TypingStyle

`Public Function TypingStyle() As TextStyle`

No parameters.

#### Returns

| Type |
| --- |
| `TextStyle` |

### SelectedText

`Public Function SelectedText() As String`

No parameters.

#### Returns

| Type |
| --- |
| `String` |

### SetDocument

`Public Sub SetDocument(document As StyledText)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `document` | `StyledText` | `ByVal` | Required |

Returns: no value.

### Select

`Public Sub Select(start As Integer, length As Integer)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `start` | `Integer` | `ByVal` | Required |
| `length` | `Integer` | `ByVal` | Required |

Returns: no value.

### SetTypingStyle

`Public Sub SetTypingStyle(style As TextStyle)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `style` | `TextStyle` | `ByVal` | Required |

Returns: no value.

### ApplySelectionStyle

`Public Sub ApplySelectionStyle(style As TextStyle)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `style` | `TextStyle` | `ByVal` | Required |

Returns: no value.

### SetSelectionBold

`Public Sub SetSelectionBold(value As Boolean)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Boolean` | `ByVal` | Required |

Returns: no value.

### SetSelectionItalic

`Public Sub SetSelectionItalic(value As Boolean)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Boolean` | `ByVal` | Required |

Returns: no value.

### SetSelectionUnderline

`Public Sub SetSelectionUnderline(value As Boolean)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Boolean` | `ByVal` | Required |

Returns: no value.

### ReplaceSelection

`Public Sub ReplaceSelection(content As String)`

Replace selected text with explicit typing attributes, preserving outside styles.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `content` | `String` | `ByVal` | Required |

Returns: no value.

### ClearHistory

`Public Sub ClearHistory()`

No parameters.

Returns: no value.

### Undo

`Public Sub Undo()`

No parameters.

Returns: no value.

### Redo

`Public Sub Redo()`

No parameters.

Returns: no value.

### Open

`Public Sub Open()`

No parameters.

Returns: no value.

### RefreshNative

`Public Sub RefreshNative()`

No parameters.

Returns: no value.

### FocusEditor

`Public Sub FocusEditor()`

No parameters.

Returns: no value.

### Close

`Public Sub Close()`

No parameters.

Returns: no value.

### Destructor

`Public Sub Destructor()`

No parameters.

Returns: no value.

### NativeUpdating

`Public Function NativeUpdating() As Boolean`

Callback entry; native callbacks never unwind a Buoy exception into C.

No parameters.

#### Returns

| Type |
| --- |
| `Boolean` |

### NativeBeginAction

`Public Sub NativeBeginAction()`

No parameters.

Returns: no value.

### NativeEndAction

`Public Sub NativeEndAction()`

No parameters.

Returns: no value.

### NativeRejectEdit

`Public Sub NativeRejectEdit()`

No parameters.

Returns: no value.

### NativeNormalizedEdit

`Public Sub NativeNormalizedEdit(start As Integer, length As Integer, content As String)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `start` | `Integer` | `ByVal` | Required |
| `length` | `Integer` | `ByVal` | Required |
| `content` | `String` | `ByVal` | Required |

Returns: no value.

### NativeAcceptEdit

`Public Function NativeAcceptEdit(start As Integer, length As Integer, content As String) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `start` | `Integer` | `ByVal` | Required |
| `length` | `Integer` | `ByVal` | Required |
| `content` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### NativeWillReplaceSelection

`Public Sub NativeWillReplaceSelection(backspace As Boolean, forwardDelete As Boolean)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `backspace` | `Boolean` | `ByVal` | Required |
| `forwardDelete` | `Boolean` | `ByVal` | Required |

Returns: no value.

### NativeWillEdit

`Public Sub NativeWillEdit(start As Integer, length As Integer)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `start` | `Integer` | `ByVal` | Required |
| `length` | `Integer` | `ByVal` | Required |

Returns: no value.

### NativeEditFinished

`Public Sub NativeEditFinished()`

No parameters.

Returns: no value.

### NativeChanged

`Public Sub NativeChanged(widget As Ptr)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `widget` | `Ptr` | `ByVal` | Required |

Returns: no value.

### NativeSelectionChanged

`Public Sub NativeSelectionChanged(widget As Ptr)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `widget` | `Ptr` | `ByVal` | Required |

Returns: no value.

### NativeShortcut

`Public Function NativeShortcut(key As Integer, shift As Boolean) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `Integer` | `ByVal` | Required |
| `shift` | `Boolean` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### NativeDestroyed

`Public Sub NativeDestroyed(widget As Ptr)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `widget` | `Ptr` | `ByVal` | Required |

Returns: no value.

## RichIter

`Sequential Structure RichIter`

### Fields

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `A0` | `Int64` | — | — |
| `A1` | `Int64` | — | — |
| `A2` | `Int64` | — | — |
| `A3` | `Int64` | — | — |
| `A4` | `Int64` | — | — |
| `A5` | `Int64` | — | — |
| `A6` | `Int64` | — | — |
| `A7` | `Int64` | — | — |
| `A8` | `Int64` | — | — |
| `A9` | `Int64` | — | — |
| `A10` | `Int64` | — | — |
| `A11` | `Int64` | — | — |

## RichGValue

`Sequential Structure RichGValue`

### Fields

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `TypeID` | `UInt64` | — | — |
| `A` | `UInt64` | — | — |
| `B` | `UInt64` | — | — |

## RichRGBA

`Sequential Structure RichRGBA`

### Fields

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `Red` | `Single` | — | — |
| `Green` | `Single` | — | — |
| `Blue` | `Single` | — | — |
| `Alpha` | `Single` | — | — |

## RichRect

`Sequential Structure RichRect`

### Fields

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `X` | `Double` | — | — |
| `Y` | `Double` | — | — |
| `Width` | `Double` | — | — |
| `Height` | `Double` | — | — |

## RichSize

`Sequential Structure RichSize`

### Fields

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `Width` | `Double` | — | — |
| `Height` | `Double` | — | — |

## RichRange

`Sequential Structure RichRange`

### Fields

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `Location` | `Integer` | — | — |
| `Length` | `Integer` | — | — |

## RichSuper

`Sequential Structure RichSuper`

### Fields

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `Receiver` | `Ptr` | — | — |
| `Parent` | `Ptr` | — | — |

## RichWinRange

`Sequential Structure RichWinRange`

### Fields

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `Start` | `Int32` | — | — |
| `Finish` | `Int32` | — | — |

## RichWinHeader

`Sequential Structure RichWinHeader`

### Fields

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `Window` | `Ptr` | — | — |
| `ID` | `UInt64` | — | — |
| `Code` | `UInt32` | — | — |
