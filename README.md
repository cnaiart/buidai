# Mintlify 入门套件

使用此入门套件快速部署并自定义您的文档。

点击此仓库顶部的绿色 **Use this template** 按钮来复制 Mintlify 入门套件。该套件包含以下示例：

- 指南页面
- 导航配置
- 自定义设置
- API 参考页面
- 常用组件的使用

**[查看完整快速入门指南](https://starter.mintlify.com/quickstart)**

## 开发

安装 [Mintlify CLI](https://www.npmjs.com/package/mint) 以在本地预览您的文档更改。使用以下命令进行安装：

```
npm i -g mint
```

在您的文档根目录（包含 `docs.json` 文件的位置）运行以下命令：

```
mint dev
```

在 `http://localhost:3000` 查看您的本地预览。

## 发布更改

从您的 [仪表板](https://dashboard.mintlify.com/settings/organization/github-app) 安装我们的 GitHub 应用，以将更改从您的仓库传播到部署。更改在推送到默认分支后会自动部署到生产环境。

## 需要帮助？

### 故障排除

- 如果您的开发环境无法运行：运行 `mint update` 以确保您拥有最新版本的 CLI。
- 如果页面加载为 404：请确保您在包含有效 `docs.json` 的文件夹中运行。

### 资源
- [Mintlify 文档](https://mintlify.com/docs)
