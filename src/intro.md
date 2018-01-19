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

上面测试用例用到了 `expect` 和 `toBe` 测试两个数值是否相等。

## REF

- [Jest - Delightful JavaScript Testing][jest]
- [Getting Started - Jest][started]
- [Using Matchers - Jest][matchers]

[jest]: https://facebook.github.io/jest/
[started]: https://facebook.github.io/jest/docs/en/getting-started.html
[matchers]: https://facebook.github.io/jest/docs/en/using-matchers.html
[kent]: https://github.com/kentcdodds
[hello-jest]: http://1zh.tech/hello-jest