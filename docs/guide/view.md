# 视图

视图是 VSCode 扩展的重要部分。在 VSCode 中有两种类型的视图：[树视图](https://code.visualstudio.com/api/extension-guides/tree-view) 和 [Webview](https://code.visualstudio.com/api/extension-guides/webview)。请阅读 [官方 UX 指南](https://code.visualstudio.com/api/ux-guidelines/views) 以获得基本了解。

## 在清单中定义 <NonProprietary />

如 [官方文档](https://code.visualstudio.com/api/references/contribution-points#contributes.viewsContainers) 中所述，首先，您需要在 `package.json` 中的 `contributes.viewsContainers.[viewContainerType]` 部分中定义视图容器。然后，您可以在 `contributes.views.[viewContainerId]` 部分中定义您的视图。

```json
{
  "contributes": {
    "viewsContainers": {
      "activitybar": [
        {
          "id": "package-explorer",
          "title": "Package Explorer",
          "icon": "resources/package-explorer.svg"
        }
      ]
    },
    "views": {
      "package-explorer": [
        {
          "id": "package-dependencies",
          "name": "Dependencies"
        },
        {
          "id": "package-outline",
          "name": "Outline"
        }
      ]
    }
  }
}
```

![自定义视图容器](https://code.visualstudio.com/assets/api/references/contribution-points/custom-views-container.png)

## 注册树视图

[树视图](https://code.visualstudio.com/api/extension-guides/tree-view) 用于显示分层数据。您可以使用 `reactive::useTreeView` 函数来定义树视图。

以下是一个树视图的示例：

<<< @/snippets/treeView.ts {35-41}

然后，您可以在任何地方调用 `useDemoTreeView` 函数来注册树视图并获取返回值：

```ts {2,5}
import { defineExtension } from 'reactive-vscode'
import { useDemoTreeView } from './treeView'

export = defineExtension(() => {
  const demoTreeView = useDemoTreeView()
  // ...
})
```

节点中的 `children` 属性用于定义节点的子节点。`treeItem` 属性是必需的，用于定义节点的树项目。它应该是一个 `vscode::TreeItem` 对象，或者一个解析为 `vscode::TreeItem` 对象的 promise。

如果您想基于一些响应式值触发更新，而这些值在 `treeData` 中没有被跟踪，您可以将它们传递给 `watchSource` 选项。

::: details 关于 `reactive::createSingletonComposable`
`reactive::createSingletonComposable` 是一个帮助函数，用于创建一个单例组合。它只会创建一次组合，并在每次调用时返回相同的实例。
:::

::: warning
对于上面的示例，`useDemoTreeView` **不应** 在模块的顶层调用，因为此时扩展上下文不可用。相反，您应该**始终**在 `setup` 函数中调用它。
:::

## 注册 Webview

[Webviews](https://code.visualstudio.com/api/extension-guides/webview) 用于在编辑器中显示 Web 内容。您可以使用 `reactive::useWebviewView` 函数来定义一个 Webview。

以下是一个 Webview 的示例：

<<< @/snippets/webviewView.ts

调用 `useDemoWebviewView` 的时间与前一节中的树视图相同。
