# 优雅的提交你的 Git Commit Message

commit message 是开发的日常操作，写好 log 不仅有助于他人 review，还可以有效的输出
CHANGELOG，对项目的管理至关重要，但是实际工作中却常常常被大家忽略，希望本文能够帮助大家
重视和规范 commit message 的书写

目前规范使用较多的是 [Angular 团队的规范](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)，很多工具也是基于此规范，message 格式如下

```
<type>(<scope>): subject
BLANK LINE
<body>
BLANK LINE
<footer>
```

我们通过`git commit`命令带出的 vim 界面填写的最终结果应该类似如上这个结构，大致分为
三个部分，使用空行分割

- 标题行：必填，描述主要修改类型
- 主题内容：描述为什么修改，做了什么修改，以及开发思路等等
- 页脚注释：放 Breaking Changes 或 Closed Issues

#### type 的类型

- feat: 新特性
- fix: 修改问题
- docs: 文档修改
- refactor: 代码重构
- test: 测试用例修改
- style: 代码格式修改（注意不是前端 css 的修改）
- chore: 其他修改，比如构建流程、依赖管理等

#### scope

可以为空，commit 影响的范围的范围，例如`router`,`component`,`utils`,`build`等

#### subject

commit 的标题或概述，建议符合[50/72 formatting](https://stackoverflow.com/questions/2290016/git-commit-messages-50-72-formatting)

#### body

具体修改内容，可以分多行

#### footer

一些备注，通常是 BREAKING CHANGE 或修复的 bug 的连接

这样一个符合规范的 commit message 就像是一份邮件

例如：

```bash
# fix: fix component bug
#
# 修改xxx component在某些情况下的问题
#
# bug地址，[bug](http://example.bug.component-xxx.html)
```

## Commitlint 校验你的 message

[commitlint](https://github.com/conventional-changelog/commitlint)

`commitlint`可以帮助我们校验 commit message，如果我们的提交不符合指向的规范，直接拒绝提交

同样的，它需要一份校验配置，官方给出的标准配置 [@commitlint/config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/@commitlint/config-conventional)

### 安装

```bash
npm install --save-dev @commitlint/cli @commitlint/config-conventional

# or

pnpm add --save-dev @commitlint/cli @commitlint/config-conventional -w
```

### 校验配置

新增`commitlint.config.js`

```js
module.exports = {
  extends: ["@commitlint/config-conventional"],
  rules: {},
};
```

### 与 husky 结合

```bash
npm install --save-dev husky
```

Edit `package.json > prepare`script and run it once

```bash
npm set-script prepare "husky install"
npm run prepare
```

增加`hook`命令

```
npx husky add .husky/commit-msg "npx --no -- commitlint --edit $1"
```
