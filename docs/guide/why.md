# 为什么使用 <ReactiveVscode />

VSCode 扩展是增强开发体验的强大工具。但是开发 VSCode 扩展并不容易。这个库是为了帮助您使用 Vue 的响应性系统开发 VSCode 扩展而创建的。

## 问题

开发 VSCode 扩展并不容易。官方 API 有点原始，存在一些问题：

### 难以观察状态

官方 API 是基于事件的，这意味着您必须监听事件来观察状态。这会产生大量冗余代码，并且对 Vue 开发者来说并不熟悉。

### 可处置对象

在 VSCode 扩展中，处置对象随处可见。您必须将它们全部存储到 `vscode::ExtensionContext.subscriptions` 中，或者手动处理它们。

### 何时初始化

在 VSCode 扩展中，视图是懒加载创建的。如果您想要访问视图实例，您必须将其存储起来，甚至监听一个在视图创建时触发的事件。

### 想要使用 Vue

Vue 的响应性系统非常强大。使用 Vue 的响应性系统更容易观察状态并更新视图。但是 VSCode API 并不是为与 Vue 一起使用而设计的。

## 解决方案

[Vue 的响应性 API](https://vuejs.org/api/reactivity-core.html) 就是您需要的一切。这个库将大部分 VSCode API 封装成 [Vue 组合式](https://vuejs.org/guide/reusability/composables.html)。您可以像使用 Vue 响应性 API 一样使用它们，这对 Vue 开发者来说是熟悉的。

借助这个库的帮助，您可以像开发 Vue 3 Web 应用程序一样开发 VSCode 扩展。您可以使用 Vue 的响应性系统来观察状态，并将视图实现为 Vue 组合式。

### 结果

以下是一个示例，展示了这个库如何帮助您开发 VSCode 扩展。以下扩展根据配置装饰活动文本编辑器。

::: code-group

<<< ../examples/editor-decoration/1.ts [<ReactiveVscode2 />]

<<< ../examples/editor-decoration/2.ts [原始 VSCode API]

:::

如您所见，使用 <ReactiveVscode /> 后，代码变得更清晰、更易理解。借助这个库提供的像 `reactive::useActiveTextEditor` 这样的组合式，您可以在开发 VSCode 扩展时顺畅地使用 Vue 的响应性 API，就像 `vue::watchEffect(https://vuejs.org/api/reactivity-core.html#watcheffect)` 一样。

更多示例[在这里](../examples/){target="_blank"}。

## 常见问题

### Vue 没有 DOM 和组件？

这个库是建立在 `npm::@vue/reactivity` 之上的，并从 `npm::@vue/runtime-core` 中移植了一些代码（请参阅[ `./packages/core/src/reactivity` 目录](https://github.com/KermanX/reactive-vscode/blob/main/packages/core/src/reactivity)）。

使用这个库构建的最小扩展的大小约为 12KB。

### 在 Webview 中使用 Vue？

这个库**不是**为在 Webview 中使用 Vue 而设计的。如果您想在 Webview 中使用 Vue，您可以使用 Vue 的 CDN 版本或类似 `npm::@tomjs/vite-plugin-vscode` 的捆绑器插件。
