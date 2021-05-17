## 命令行接口

### 安装

安装最新版或特定版本 webpack

```
npm install --save-dev webpack
// 当前最新版本 v5.37.0

// 安装特定版本
npm install --save-dev webpack@4
```

如果你使用 webpack v4+版本，你还需要安装 **CLI**

```
npm install --save-dev webpack-cli
```

对于大多数项目，我们建议本地安装。这可以在引入突破式变更（breaking change）版本时，
更容易分别升级项目

```
"scripts": {
  "build": "webpack --config webpack.config.js"
}
```

> 想要运行本地安装的 webpack，你可以通过
> `node_modules/.bin/webpack`
> 来访问它的 bin 版本
