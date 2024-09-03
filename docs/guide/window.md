# 窗口和工作区

## 主题

您可能希望根据当前主题为您的扩展应用不同的样式。尽管许多 API（如 `vscode::TreeItem.iconPath`）内置支持双主题，但有些不支持。您可能还希望在您的 Webview 中同步主题。

`reactive::useActiveColorTheme` 和 `reactive::useIsDrakTheme` 可以用于获取当前主题以及它是否为暗色。

```ts {5,6}
import { defineExtension, useActiveColorTheme, useIsDarkTheme, watchEffect } from 'reactive-vscode'
import { useDemoWebviewView } from './webviewView'

export = defineExtension(() => {
  const theme = useActiveColorTheme()
  const isDark = useIsDarkTheme()

  const webviewView = useDemoWebviewView()

  watchEffect(() => {
    webviewView.postMessage({
      type: 'updateTheme',
      isDark: isDark.value,
      //         ^?
    })
  })
})
```

## 窗口状态

`reactive::useWindowState` 可以用于获取当前窗口状态：

- `vscode::WindowState.active` - 窗口是否最近有交互。这将在活动后立即更改，或在用户不活动一段时间后更改。
- `vscode::WindowState.focused` - 当前窗口是否处于焦点状态。

```ts {4}
import { defineExtension, useWindowState, watchEffect } from 'reactive-vscode'

export = defineExtension(() => {
  const { active: isWindowActive, focused: isWindowFocused } = useWindowState()

  watchEffect(() => {
    console.log('Window is active:', isWindowActive.value)
    console.log('Window is focused:', isWindowFocused.value)
  })
})
```

## 工作区文件夹

`reactive::useWorkspaceFolders` 可以用于获取工作区文件夹：

```ts {4}
import { defineExtension, useWorkspaceFolders, watchEffect } from 'reactive-vscode'

export = defineExtension(() => {
  const workspaceFolders = useWorkspaceFolders()

  watchEffect(() => {
    console.log('There are', workspaceFolders.value?.length, 'workspace folders')
    //                         ^?
  })
})
```

## 监视文件系统更改

`reactive::useFsWatcher` 可以用于监视文件系统更改：

```ts {4}
import { computed, defineExtension, useFsWatcher, watchEffect } from 'reactive-vscode'

export = defineExtension(() => {
  const filesToWatch = computed(() => ['**/*.md', '**/*.txt'])
  const watcher = useFsWatcher(filesToWatch)
  watcher.onDidChange((uri) => {
    console.log('File changed:', uri)
  })
})
```

请注意，您可以传递一个文件系统更改数组中的模式之一以监视文件系统更改。为每个模式创建多个 VSCode 监视器。
