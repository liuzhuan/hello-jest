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

问题在于测试用例会立即结束，来不及调用回调函数。

可以使用另一种形式的 `test` 修复它。即在 `test` 回调函数中传入 `done` 参数。Jest 会等待 `done` 被执行，才会真正结束测试用例。

```js
test('the data is peanut butter', done => {
  function callback(data) {
    expect(data).toBe('peanut butter')
    done()
  }

  fetchData(callback)
})
```

若没有调用 `done`，测试用例会失败，如你所愿。

## Promises

若代码中含有 Promise，在异步测试中有更简单的方法。只需在 test 中返回一个 promise，Jest 会等待 Promise 结束。如果 Promise 被拒，测试自动失败。

比如，`fetchData` 使用 Promise ，成功后返回字符串 `peanut butter`。我们可以如此测试：

```js
test('the data is peanut butter', () => {
  expect.assertions(1)
  return fetchData().then(data => {
    expect(data).toBe('peanut butter')
  })
})
```

一定要返回 Promise --- 如果忘记 return 语句，测试就会在 `fetchData` 完成前结束。

如果期望使用 `.catch` 函数捕捉被拒的 promise，一定要使用 `expect.assertions` 声明期望的断言数量。否则，成功通过的 Promise 将无法使测试用例失败。

```js
test('the fetch fails with an error', () => {
  // 提前声明期望的断言数量
  expect.assertions(1)
  return fetchData().catch(e => expect(e).toMatch('error'))
})
```

TO BE CONTINUE

## REF

- [Testing Asynchronous Code - Jest][docs]

[docs]: https://facebook.github.io/jest/docs/en/asynchronous.html