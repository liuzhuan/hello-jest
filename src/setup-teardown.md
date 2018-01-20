# 准备和扫尾

编写测试用例时，我们通常需要做一些准备工作，然后才执行真正的测试断言。测试完成后，也可能需要执行一些清理扫尾工作。Jest 提供了辅助函数来简化这些任务。

## 为大批量测试准备

如果需要在很多测试用例中重复执行很多任务，可以将他们放入 `beforeEach` 和 `afterEach`。

比如，在几个测试中都需要访问城市数据库。有个函数 `initializeCityDatabase()` 需要首先调用，结束后都需要执行 `clearCityDatabase()`。你可以这么做：

```js
beforeEach(() => {
  initializeCityDatabase()
})

afterEach(() => {
  clearCityDatabase()
})

test('city database has Vienna', () => {
  expect(isCity('Vienna')).toBeTruthy()
})

test('city database has San Juan', () => {
  expect(isCity('San Juan')).toBeTruthy()
})
```

`beforeEach` 和 `afterEach` 也可以处理异步编程，方式和普通测试用例写法一样，可以使用 `done` 参数，或者返回一个 promise 实例。

## REF

- [Setup and Teardown - Jest][docs]

[docs]: https://facebook.github.io/jest/docs/en/setup-teardown.html