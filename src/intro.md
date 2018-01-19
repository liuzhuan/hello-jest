# 开始使用

安装

```
npm install --save-dev jest
yarn add --dev jest
```

编写业务代码，比如 `sum.js`：

```js
function sum(a, b) {
    return a + b
}
module.exports = sum
```

创建测试用例，比如 `sum.test.js`：

```js
const sum = require('./sum')

test('adds 1 + 2 to equal 3', () => {
    expect(sum(1 + 2)).toBe(3)
})
```

更新 `package.json`，增加测试命令：

```json
{
    "scripts": {
        "test": "jest"
    }
}
```

运行测试用例：

```
npm test
```

**我们刚刚成功执行了第一个测试用例！**

上面测试用例用到了 `expect` 和 `toBe` 测试两个数值是否相等。想了解更多匹配功能，可查看[使用匹配器][matchers]。

## 从命令行执行

可从命令行直接运行 Jest（若全局安装 Jest，比如 `npm install -g jest`），可以带一些参数。

比如，如下命令对匹配 `my-test` 的文件执行测试命令，使用 `config.json` 作为配置文件，运行结束后使用原生系统通知提醒：

```sh
jest my-test --notify --config=config.json
```

如果想了解执行命令行的更多内容，可以参考 [Jest 命令行选项][cli]。

## 更多配置

### 使用 Babel

使用 Babel 需安装 `babel-jest` 和 `regenerator-runtime`：

```sh
npm install --save-dev babel-jest babel-core regenerator-runtime
```

> [regenerator][regenerator] 是 Facebook 出品的工具，可以把 ES6 生成器语法转换为高效的 ES5 语法。

> ⚠️ 注意：若 babel 版本为 7，你需要使用如下命令安装 jest：

```sh
npm install --save-dev babel-jest 'babel-core@^7.0.0-0' @babel/core regenerator-runtime
```

如果使用 npm 3 或 npm 4 或 Yarn，则无须显式安装 `regenerator-runtime`。

在根目录增加 `.babelrc`。如果你使用 ES6 和 React.js，则需要增加 `babel-preset-env` 和 `babel-preset-react` 预设值：

```json
{
  "presets": ["env", "react"]
}
```

现在，你可以使用所有的 ES6 特性和 React 相关特性了。

⚠️ 注意：如果使用复杂 Babel 配置，或 Bebel 的 `env` 选项，要记得 Jest 会自动将 `NODE_ENV` 设置为 `test`。它与 Babel 不同，当 `NODE_ENV` 未定义时，默认使用 `development` 选项。

⚠️ 注意：如果你使用选项 `{ "modules": false }` 关闭了 ES6 模块转译，需要在测试环境中重新打开。

```json
{
  "presets": [["env", { "modules": false }], "react"],
  "env": {
    "test": {
      "presets": [["env"], "react"]
    }
  }
}
```

注意：当安装 Jest 时，`babel-jest` 会被自动安装，并且如果存在 babel 配置文件，转译会自动执行。如果不想要这个行为，可以明确的重置 `transform` 配置选项：

```json
// package.json
{
  "jest": {
    "transform": {}
  }
}
```

### 使用 webpack

Jest 可以在使用 webpack 构建的项目中使用，webpack 配置较复杂，具体可参考 [webpack 指南][webpack]。

### TypeScript

测试 TypeScript 可以使用 [ts-jest][jest]。

## REF

- [Jest - Delightful JavaScript Testing][jest]
- [Getting Started - Jest][started]
- [Using Matchers - Jest][matchers]

[jest]: https://facebook.github.io/jest/
[started]: https://facebook.github.io/jest/docs/en/getting-started.html
[matchers]: ./using-matchers.md
[cli]: ./cli.md
[kent]: https://github.com/kentcdodds
[hello-jest]: http://1zh.tech/hello-jest
[regenerator]: http://facebook.github.io/regenerator/
[webpack]: ./webpack.md
[ts]: https://github.com/kulshekhar/ts-jest