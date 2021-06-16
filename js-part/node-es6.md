## 如何用 node 执行用 ES6 写的 js 文件

### babel-node

babel-node is a CLI that works exactly the same as the Node.js CLI, with the added
benefit of compiling with Babel presets and plugins before running it

安装

```
npm install --save-dev @babel/core @baebel/preset-env @babel/node
```

使用

```
npx babel-node --presets @babel/preset-env test
```

**node 执行用 ES6 写的 js 文件**

compile and run `test.js`

```JSON
{
  "name": "my-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel-node --presets @babel/preset-env test.js"
  }
}
```

## 在项目中使用 Babel

项目中增加 `babel-cli`

We can install Babel CLI locally by running

```Shell
npm install --save-dev @babel/core @babel/cli
```

在项目中根目录增加`babel.config.json`文件，enable some `presets` to start，
你可以使用`preset-env`，这样就可以转译 ES6+了

```Shell
npm install --save-dev @baebel/preset-env
```

`babel.config.json`文件，这样设置

```JSON
{
  "presets": ["@babel/preset-env"]
}
```

增加执行命令

```JSON
{
  "name": "my-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src -d lib"
  }
}
```
