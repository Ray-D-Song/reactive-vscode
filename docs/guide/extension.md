# 扩展

使用 <ReactiveVscode /> 创建 VSCode 扩展非常简单。您只需要在 `reactive::defineExtension` 函数中定义您的扩展代码，并导出返回的 `activate` 和 `deactivate` 函数。

```ts
import { defineExtension } from 'reactive-vscode'

export = defineExtension(() => {
  // Setup your extension here
})
```

::: details TypeScript 配置 <span class="i-vscode-icons:file-type-typescript-official text-2xl mt--1 ml-1"></span>
VSCode 扩展应该是 CommonJS 模块。由于 ESM 不允许 `export =` 语句，如果您使用类似 `tsup` 的打包工具，您需要在 `tsconfig.json` 中添加以下配置以让 TypeScript 正常工作。

```json
{
  "compilerOptions": {
    "moduleResolution": "Bundler",
    "module": "Preserve"
  }
}
```

或者您可以通过以下方式避免使用 `export =` 语句：

```ts
import { defineExtension } from 'reactive-vscode'
const { activate, deactivate } = defineExtension(() => {
  // Your extension code here
})
export { activate, deactivate }
```
:::

## 设置函数

与 Vue 3 中的 `setup` 函数类似，<ReactiveVscode /> 中的设置函数是定义您的扩展应该如何行为的函数。当扩展被激活时，此函数将被调用一次。

您可以在设置函数中执行以下操作：

- 注册命令（通过 `reactive::useCommand` 或 `reactive::useCommands`）
- 注册视图（在[下一节](./view.md)中介绍）
- 定义其他（响应式）逻辑（通过 `vue::watchEffect` 或 `vue::watch` 等）
- 使用其他组合式（如 `reactive::useActiveTextEditor`）

以下是一个示例：

<!-- eslint-disable import/first -->
```ts
import type { Ref } from 'reactive-vscode'
/**
 * Defined via `defineConfigs`
 */
declare const message: Ref<string>
// ---cut---
import { defineExtension, useCommand, useIsDarkTheme, useLogger, watchEffect } from 'reactive-vscode'
import { window } from 'vscode'
import { useDemoTreeView } from './treeView'

export = defineExtension(() => {
  const logger = useLogger('Reactive VSCode')
  logger.info('Extension Activated')
  logger.show()

  useCommand('reactive-vscode-demo.helloWorld', () => {
    window.showInformationMessage(message.value)
  })

  const isDark = useIsDarkTheme()
  watchEffect(() => {
    logger.info('Is Dark Theme:', isDark.value)
  })

  useDemoTreeView()
})
```

## 扩展上下文

[扩展上下文](https://code.visualstudio.com/api/references/vscode-api#ExtensionContext) 可以从 `reactive-vscode` 中导入。它是一个全局的 `shallowRef`，包含 `vscode::ExtensionContext` 对象。

```ts
import { extensionContext } from 'reactive-vscode'

extensionContext.value?.extensionPath
//                 ^?
```

<div mt-8 />

一个常见的用例是获取扩展中某些资源的绝对路径。在这种情况下，您可以使用 `reactive::useAbsolutePath` 作为快捷方式。
