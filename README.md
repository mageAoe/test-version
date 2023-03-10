### 记录一下 change-log、

每个 npm 包中都有一个 package.json 文件，如果要发包的话，package.json 中的 version 就是版本号了。
version 字段结构为：'0.0.0-0'
分别代表：大号.中号.小号-预发布号，对应 majon.minor.patch-prerelease
下面来看看 npm 中 version 的类别及描述。

下面来看看 npm 中 version 的类别及描述。

- major

  - 如果没有预发布号，则直接升级一位大号，其他位都置为 0
  - 如果有预发布号：

    - 中号和小号都为 0，则不升级大号，而将预发布号删掉。即 2.0.0-1 变成 2.0.0，这就是预发布的作用

    - 如果中号和小号有任意一个不是 0，那边会升级一位大号，其他位都置为 0，清空预发布号。即 2.0.1-0 变成 3.0.0

- minor

  如果没有预发布号，则升级一位中号，大号不动，小号置为空
  如果有预发布号：

  - 如果小号为 0，则不升级中号，将预发布号去掉
  - 如果小号不为 0，同理没有预发布号

- patch
  如果没有预发布号：直接升级小号，去掉预发布号
  如果有预发布号：去掉预发布号，其他不动

- premajor
  直接升级大号，中号和小号置为 0，增加预发布号为 0

- preminor
  直接升级中号，小号置为 0，增加预发布号为 0

- prepatch
  直接升级小号，增加预发布号为 0

- prerelease
  如果没有预发布号：增加小号，增加预发布号为 0
  如果有预发布号，则升级预发布号

---

执行命令 npm version 可以自动更改 package.json 中的版本号
比如，package.json 包中的 version 是 1.1.1，然后执行如下命令：

```js
npm version [<newversion> | major | minor | patch | premajor | preminor | prepatch | prerelease | from-git]

alias: verison
```

```js
npm version prerelease -m "这里你可以添加此次更新版本号的描述"
```

执行完之后，package.json 的版本号则会变成 1.1.1-0，同时，在 git 中会多一个 commit log。

值得注意的是，执行 npm version 必须保证工作目录是干净的，没有任何未提交的文档，否则会报错。

### 自动生成 change log

如果所有的 commit 都符合格式，那么发布新版本时，change log 就可以用脚本自动生成

```js
npm i conventional-changelog-cli --save-dev
```

配置 package.json

```js
"changelog": "conventional-changelog -p angular -i CHANGELOG.md -s -r 0"
```

上面 changelog 命令不会覆盖以前的 CHANGELOG，只会在 CHANGELOG.md 的头部加上自从上次发布以来的变动。

```js
npm run changelog
```

生成 CHANGELOG.md 文件

参考文章： https://juejin.cn/post/6844903888072654856#heading-5

### 配置 changelog-option.js

> 可以配置也可以不配置，看自己需求

changelog-option.js 文件，观察这个文件后，发现正是这个文件定义了哪些 git commit 要写入更新日志，以及生成日志的每块内容的标题。

例如，如果我只关注 feat(新功能) 、fix(Bug 修复) 、perf(性能优化)、revert(回退)，那么我就可以在配置中这样写。

```js
module.exports = {
  writerOpts: {
    transform: (commit, context) => {
      if (commit.type === "feat") {
        commit.type = "✨ Features | 新功能";
      } else if (commit.type === "fix") {
        commit.type = "🐛 Bug Fixes | Bug 修复";
      } else if (commit.type === "perf") {
        commit.type = "⚡ Performance Improvements | 性能优化";
      } else if (commit.type === "revert" || commit.revert) {
        commit.type = "⏪ Reverts | 回退";
      }
      return;
    },
  },
};
```
