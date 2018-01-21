# 模拟功能

Mock 函数让代码间的联系易于测试，无需真正实现函数，捕获函数调用以及传递的实参，捕获构造函数生成的实例，允许返回值的测试时间配置。

有两种模拟数据的方式：创建模拟函数，在测试代码中使用，或者编写手工模拟数据，覆盖模块依赖。

## 使用模拟函数

假如我们要测试 `forEach` 函数，待测代码如下：

```js
function forEach(items, callback) {
  for (let index = 0; index < items.length; index++) {
    callback(items[index])
  }
}
```

为了测试该函数，我们需要一个模拟函数，并需要检测检测模拟数据的状态，以便确保回调函数按期望结果执行：

```js
const mockCallback = jest.fn()
forEach([0, 1], mockCallback)

// 模拟函数被调用两次
expect(mockCallback.mock.calls.length).toBe(2)

// 第一次调用的第一个参数是 0
expect(mockCallback.mock.calls[0][0]).toBe(0)

// 第二次调用的第一个实参是 1
expect(mockCallback.mock.calls[1][0]).toBe(1)
```

## `.mock` 属性

所有的 mock 函数都有一个特殊的 `.mock` 属性，它保存着函数执行的状态数据。`.mock` 属性也保存着每次调用的 `this` 指向的对象，因此，也可以探查它：

```js
const myMock = jest.fn()

const a = new myMock()
const b = {}
const bound = myMock.bind(b)
bound()

console.log(myMock.mock.instances)
```

这些 mock 成员在测试中很有用，可以确保这些函数如何调用，或者实例化：

```js

```

## REF

- [Mock Functions - Jest][docs]

[docs]: https://facebook.github.io/jest/docs/en/mock-functions.html