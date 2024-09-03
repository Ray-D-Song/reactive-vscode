# 终端

VSCode提供了一个强大的终端系统，允许您在集成终端中运行shell命令。`<ReactiveVscode />`提供了一组可组合的函数，以响应式方式创建和管理终端。

## 创建终端

`reactive::useTerminal` 通过 `vscode::window.createTerminal` 创建一个终端。参数与 `vscode::window.createTerminal` 相同。

```ts
import { defineExtension, useTerminal } from 'reactive-vscode'

export = defineExtension(() => {
  const {
    terminal,
    name,
    processId,
    creationOptions,
    exitStatus,
    sendText,
    show,
    hide,
    state,
  } = useTerminal('My Terminal', '/bin/bash')
})
```

## 创建可控制的终端

`reactive::useControlledTerminal` 创建一个允许您控制其生命周期的终端。参数与 `vscode::window.createTerminal` 相同。

```ts
import { defineExtension, useControlledTerminal } from 'reactive-vscode'

export = defineExtension(() => {
  const {
    terminal,
    getIsActive,
    show,
    sendText,
    close,
    state,
  } = useControlledTerminal('My Terminal', '/bin/bash')
})
```

### 活动终端

您可以使用 `reactive::useActiveTerminal` 获取当前活动的终端。

```ts
import { defineExtension, useActiveTerminal } from 'reactive-vscode'

export = defineExtension(() => {
  const activeTerminal = useActiveTerminal() // [!code highlight]
  //     ^?
})
```

### 所有打开的终端

您可以使用 `reactive::useOpenedTerminals` 获取所有打开的终端。

```ts
import { defineExtension, useOpenedTerminals } from 'reactive-vscode'

export = defineExtension(() => {
  const terminals = useOpenedTerminals() // [!code highlight]
  //     ^?
})
```

### 获取终端状态

您可以使用 `reactive::useTerminalState` 获取现有终端的状态。

```ts
import { defineExtension, useActiveTerminal, useOpenedTerminals, useTerminalState } from 'reactive-vscode'

export = defineExtension(() => {
  const activeTerminal = useActiveTerminal()
  const activeTerminalState = useTerminalState(activeTerminal) // [!code highlight]

  const allTerminals = useOpenedTerminals()
  const firstTerminalState = useTerminalState(() => allTerminals.value[0]) // [!code highlight]
  //     ^?
})
```
