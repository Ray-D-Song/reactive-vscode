# 编辑器和文档

编辑器和文档是 VS Code 扩展开发的核心。在本指南中，我们将学习如何与编辑器和文档进行交互。

## 活动编辑器

活动编辑器是当前具有焦点的编辑器，或者当没有焦点时，是最近更改输入的编辑器。`reactive::useActiveTextEditor` 和 `reactive::useActiveNotebookEditor` 可以用于获取活动文本编辑器。

```ts
import { defineExtension, useActiveNotebookEditor, useActiveTextEditor, watchEffect } from 'reactive-vscode'
import type { ExtensionContext } from 'vscode'

export = defineExtension(() => {
  const textEditor = useActiveTextEditor() // [!code highlight]
  const notebookEditor = useActiveNotebookEditor() // [!code highlight]

  watchEffect(() => {
    console.log('Active Text Editor:', textEditor.value)
    console.log('Active Notebook Editor:', notebookEditor.value)
    //                                      ^?
  })
})
```

## 可见编辑器

`reactive::useVisibleTextEditors` 和 `reactive::useVisibleNotebookEditors` 可以用于获取可见文本编辑器。

```ts
import { defineExtension, useVisibleNotebookEditors, useVisibleTextEditors, watchEffect } from 'reactive-vscode'

export = defineExtension(() => {
  const textEditors = useVisibleTextEditors()
  const notebookEditors = useVisibleNotebookEditors()

  watchEffect(() => {
    console.log('Visible Text Editors:', textEditors.value)
    console.log('Visible Notebook Editors:', notebookEditors.value)
    //                                         ^?
  })
})
```

## 获取编辑器文档

- `vscode::TextEditor.document` 是与此文本编辑器关联的文档。

- `vscode::NotebookEditor.notebook` 与此笔记本编辑器关联的[笔记本文档](https://code.visualstudio.com/api/references/vscode-api#NotebookDocument)。

文档将在此文本编辑器或笔记本编辑器的整个生命周期中保持不变。

```ts
import { computed, defineExtension, useActiveTextEditor } from 'reactive-vscode'
import type { ExtensionContext } from 'vscode'

export = defineExtension(() => {
  const textEditor = useActiveTextEditor()
  const document = computed(() => textEditor.value?.document) // [!code highlight]
  //     ^?
})
```

## 文档文本

`reactive::useDocumentText` 可以用于获取活动文档的文本。

```ts
import { computed, defineExtension, useActiveTextEditor, useDocumentText } from 'reactive-vscode'
import type { ExtensionContext } from 'vscode'

export = defineExtension(() => {
  const textEditor = useActiveTextEditor()
  const document = computed(() => textEditor.value?.document)
  const text = useDocumentText(document) // [!code highlight]
  //     ^?
})
```

返回的 `text` 是可设置的，这意味着您可以更新文档的文本。

<!-- eslint-disable import/first -->
```ts
import { computed, defineExtension, ref, useActiveTextEditor, useDocumentText, watchEffect } from 'reactive-vscode'
import type { ExtensionContext } from 'vscode'

export = defineExtension(() => {
  const editor = useActiveTextEditor()
  const text = useDocumentText(() => editor.value?.document)

  // Reactive, may be set from other places
  const name = ref('John Doe')

  watchEffect(() => {
    text.value = `Hello, ${name.value}!` // [!code highlight]
  })
})
```

## 编辑器装饰

`reactive::useEditorDecorations` 可以用于向编辑器添加装饰。

```ts {5-9}
import { defineExtension, useActiveTextEditor, useEditorDecorations } from 'reactive-vscode'

export = defineExtension(() => {
  const editor = useActiveTextEditor()
  useEditorDecorations(
    editor,
    { backgroundColor: 'red' }, // Or created decoration type
    () => [/* Dynamic calculated ranges */] // Or Ref/Computed
  )
})
```

查看 `vscode::TextEditor.setDecorations` 以获取更多信息。要创建装饰类型，请使用 `vscode::window.createTextEditorDecorationType`。

## 编辑器选择

以下 4 个可组合项可用于**获取和设置**编辑器的选择。

- `reactive::useTextEditorSelections` - 文本编辑器中的所有选择。
- `reactive::useTextEditorSelection` - 文本编辑器中的主要选择。
- `reactive::useNotebookEditorSelections` - 笔记本编辑器中的所有选择。
- `reactive::useNotebookEditorSelection` - 笔记本编辑器中的主要选择。

请查看它们的文档以获取更多信息。请注意，`reactive::useTextEditorSelections` 和 `reactive::useTextEditorSelection` 还支持一个 `acceptKind` 选项，用于过滤触发此事件的更改类型（请参阅 `vscode::TextEditorSelectionChangeKind`）。

## 编辑器视口

以下 3 个可组合项可用于**获取**编辑器的视口信息。

- `reactive::useTextEditorViewColumn` - 文本编辑器的视图列。
- `reactive::useTextEditorVisibleRanges` - 文本编辑器的可见范围。
- `reactive::useNotebookEditorVisibleRanges` - 笔记本编辑器的可见范围。

请查看它们的文档以获取更多信息。
