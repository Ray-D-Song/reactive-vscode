# VueUse 集成

<ReactiveVscode /> 为 VSCode 扩展开发提供了 [VueUse](https://vueuse.org/) 的可选集成。

该软件包包含了一组与 Node.js 环境兼容的 VueUse 函数。这意味着依赖于浏览器环境的函数已被移除。

此外，该软件包使用来自 `npm::@reactive-vscode/reactivity` 而非 `npm::vue-demi` 软件包的 Vue 响应性 API。这意味着依赖于 Vue 渲染 API 的函数已被移除。

## 用法

::: code-group

```bash [pnpm]
pnpm install -D @reactive-vscode/vueuse
```

```bash [npm]
npm install -D @reactive-vscode/vueuse
```

```bash [yarn]
yarn add -D @reactive-vscode/vueuse
```

:::

```ts
import { useIntervalFn } from '@reactive-vscode/vueuse'
import { defineExtension } from 'reactive-vscode'

export = defineExtension(() => {
  useIntervalFn(() => {
    console.log('Hello World')
  }, 1000)
})
```

## 可用函数

此软件包中包含了与 Node.js 环境兼容且不需要 Vue 渲染 API 的每个 VueUse 函数。查看 [`packages/vueuse/src/index.ts`](https://github.com/KermanX/reactive-vscode/blob/main/packages/vueuse/src/index.ts) 获取完整列表。
