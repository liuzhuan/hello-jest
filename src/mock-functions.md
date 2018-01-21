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
// 这个函数只被调用一次
expect(someMockFunction.mock.calls.length).toBe(1)

// 第一次调用的第一个参数是 'first arg'
expect(someMockFunction.mock.calls[0][0]).toBe('first arg')

// 第一次调用的第二个实参是 'second arg'
expect(someMockFunction.mock.calls[0][1]).toBe('second arg')

// 函数被实例化两次
expect(someMockFunction.mock.instances.length).toBe(2)

// 第一个实例的 name 属性是 test
expect(someMockFunction.mock.instances[0].name).toEqual('test')
```

> 貌似 `mock.calls` 和 `mock.instances` 的调用次数（`.length`）一样？

## 模拟返回数值

模拟函数还可以用来向代码注入测试数值：

```js
const myMock = jest.fn()
console.log(myMock())

myMock
  .mockReturnValueOnce(10)
  .mockReturnValueOnce('x')
  .mockReturnValue(true)

console.log(myMock(), myMock(), myMock(), myMock())
```

Mock 函数在 [CPS][cps] 风格代码中十分有效。这种风格的代码可以避免复杂的桩数据，用来复现它们代表的组件行为。而是直接注入数值。

```js
const filterTestFn = jest.fn()

filterTestFn.mockReturnValueOnce(true).mockReturnValueOnce(false)

const result = [11, 12].filter(filterTestFn)

console.log(result) // => 11
console.log(filterTestFn.mock.calls) // => [[11], [12]]
```

## 模拟实现

有些场景需要的更多，不仅仅是返回值，而且需要模拟完整的函数实现。这可以通过 mock 函数的 `jest.fn` 或 `mockImplementationOnce` 实现。

```js
const myMockFn = jest.fn(cb => cb(null, true))

myMockFn((err, val) => console.log(val))

myMockFn((err, val) => console.log(val))
```

TO BE CONTINUE...

## REF

- [Mock Functions - Jest][docs]
- [What is continuation-passing style in functional programming? - quora][cps]

[docs]: https://facebook.github.io/jest/docs/en/mock-functions.html
[cps]: https://www.quora.com/What-is-continuation-passing-style-in-functional-programming#