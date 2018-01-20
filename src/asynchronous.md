# 测试异步代码

JS 中会用到很多异步编程。当测试异步代码时，Jest 需要知道代码何时结束，然后它才会继续执行另一条测试用例。Jest 有很多方法处理异步编程。

## 回调函数

最常见的异步编程莫过于回调函数。

比如，你有一个 `fetchData(callback)` 函数，获取一些数据，完成后调用 `callback(data)`。你想检测返回的 `data` 刚好是字符串 `peanut butter`。

默认情况下，Jest 一旦执行完成，测试用例就结束了。这意味着如下代码不会如你所愿：

```js
// DO NOT do this
test('the data is peanut butter', () => {
  function callback(data) {
    expect(data).toBe('peanut butter 2')
  }

  fetchData(callback)
})
```

## REF

- [Testing Asynchronous Code - Jest][docs]

[docs]: https://facebook.github.io/jest/docs/en/asynchronous.html