# 事件

尽管大部分的 VSCode API 都被 <ReactiveVscode /> 所覆盖，但有时您仍然需要创建或监听原始 [VSCode 事件](https://code.visualstudio.com/api/references/vscode-api#events)。

`reactive::useEvent` 将原始事件转换为自动处理的事件：

```ts
import { defineExtension, useEvent } from 'reactive-vscode'
import { workspace } from 'vscode'

const onDidCreateFiles = useEvent(workspace.onDidCreateFiles)

export = defineExtension(() => {
  // No need to dispose the event
  onDidCreateFiles((e) => {
    console.log('Files created:', e.files)
  })
})
```

`reactive::useEventEmitter` 创建一个友好的事件发射器，仍然扩展了 `vscode::EventEmitter`：

<!-- eslint-disable import/first -->
```ts
import type { Event } from 'vscode'
declare function someVscodeApi(options: { onSomeEvent: Event<string> }): void
// ---cut---
import { defineExtension, useEventEmitter } from 'reactive-vscode'

export = defineExtension(() => {
  const myEvent = useEventEmitter<string>([/* optional listenrs */])

  myEvent.addListener((msg) => {
    console.log(`Received message: ${msg}`)
  })

  myEvent.fire('Hello, World!')

  someVscodeApi({
    onSomeEvent: myEvent.event,
  })
})
```

您还可以将原始事件转换为友好的事件发射器：

```ts {6}
import { defineExtension, useEventEmitter } from 'reactive-vscode'
import { EventEmitter } from 'vscode'

export = defineExtension(() => {
  const rawEvent = new EventEmitter<string>()
  const myEvent = useEventEmitter(rawEvent, [/* optional listenrs */])
})
```
