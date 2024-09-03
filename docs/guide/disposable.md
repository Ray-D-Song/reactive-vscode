# 一次性资源

尽管大部分的 VSCode API 都由 <ReactiveVscode /> 覆盖，但有时您仍然需要使用 `vscode::Disposable`，这也在 [VSCode API 模式](https://code.visualstudio.com/api/references/vscode-api#disposables) 中有描述。

`reactive::useDisposable` 接受一个一次性对象，并在当前效果范围被释放时自动处理它（例如，当扩展被停用时，如果在扩展的设置函数中调用 `vscode::useDisposable`）。`reactive::useDisposable` 返回一次性对象本身。

```ts
import { defineExtension, useDisposable } from 'reactive-vscode'
import type { TextDocument } from 'vscode'
import { languages } from 'vscode'

export = defineExtension(() => {
  useDisposable(languages.registerFoldingRangeProvider(
    { language: 'markdown' },
    {
      provideFoldingRanges(document: TextDocument) {
        return []
      },
    },
  ))
})
```

请注意，对于由任何 <ReactiveVscode /> 函数创建的一次性资源，您无需使用 `reactive::useDisposable`。它们在当前效果范围被释放时会自动处理。
