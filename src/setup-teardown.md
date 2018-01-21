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

`beforeEach` 和 `afterEach` 也可以处理异步编程，方式和普通测试用例写法一样，可以使用 `done` 参数，或者返回一个 promise 实例。比如，如果 `initializeCityDatabase()` 返回一个 promise，它会在数据库初始化后解决，我们可以返回这个 promise：

```js
beforeEach(() => {
  return initializeCityDataBase()
})
```

## 一次性准备

有时候，你只需在文件开头准备一次。如果准备工作是异步的，就会特别麻烦，所以不能内联这些函数。Jest 提供了 `beforeAll` 和 `afterAll` 处理这些场景。

举个例子，如果 `initializeCityDatabase()` 和 `clearCityDatabase()` 都返回 promise，而且 city 数据库可以在不同测试用例间复用，我们可以将测试代码改为：

```js
beforeAll(() => {
  return initializeCityDatabase()
})

afterAll(() => {
  return clearCityDatabase()
})

test('city database has Vienna', () => {
  expect(isCity('Vienna')).toBeTruthy()
})

test('city database has San Juan', () => {
  expect(isCity('San Juan')).toBeTruthy()
})
```

## 作用域

`before` 和 `after` 代码块默认应用于文件中的所有测试用例。你还可以使用 `describe` 语句将多个 `test` 分组。在一个 `describe` 语句内，`before` 和 `after` 仅对其所在的 `describe` 语句块有效。

举个例子，我们不仅有 city 数据库，还有 food 数据库。我们可以为不同测试进行不同的准备工作。

```js
// 应用于文件所有测试用例
beforeEach(() => {
  return initializeCityDatabase()
})

test('city database has Vienna', () => {
  expect(isCity('Vienna')).toBeTruthy()
})

test('city database has San Juan', () => {
  expect(isCity('San Juan')).toBeTruthy()
})

describe('matching cities to foods', () => {
  // 仅对当前 describe 代码块有效
  beforeEach(() => {
    return initializeFoodDatabase()
  })

  test('Vienna <3 sausage', () => {
    expect(isValidCityFoodPair('Vienna', 'Wiener Schnizel')).toBe(true)
  })

  test('San Juan <3 plantains', () => {
    expect(isValidCityFoodPair('San Juan', 'Mofongo')).toBe(true)
  })
})
```

注意，顶级的 `beforeEach` 执行在前，`describe` 代码块的 `beforeEach` 执行在后。

## describe 和 test 代码块的执行顺序

Jest 首先执行所有的 describe 回调函数，然后才执行真正的测试。这也是为什么将准备和清理工作放在 `before*` 和 `after*` 回调函数中，而不是 describe 代码块。一旦 describe 结束，Jest 默认会依次执行测试用例，等待所有测试结束，然后执行下一步。

比如，具体例子可以参考以下代码：

```js
describe('outer', () => {
  console.log('describe outer-a')

  describe('describe inner 1', () => {
    console.log('describe inner 1')
    test('test 1', () => {
      console.log('test for describe inner 1')
      expect(true).toEqual(true)
    })
  })

  console.log('describe outer-b')

  test('test 1', () => {
    console.log('test for describe outer')
    expect(true).toEqual(true)
  })

  describe('describe inner 2', () => {
    console.log('describe inner 2')
    test('test for describe inner 2', () => {
      console.log('test for describe inner 2')
      expect(false).toEqual(false)
    })
  })

  console.log('describe outer-c')
})

// describe outer-a
// describe inner 1
// describe outer-b
// describe inner 2
// describe outer-c
// test for describe inner 1
// test for describe outer
// test for describe inner 2
```

## 一般性建议

若测试失败，首先确保只有它运行时，它是否依然失败。在 Jest 中只运行一个测试十分简单，只需临时将 `test` 改为 `test.only` 即可：

```js
test.only('this will be the only test that runs', () => {
  expect(true).toBe(false)
})

test('this test will not run', () => {
  expect('A').toBe('A')
})
```

若一个测试独自运行成功，在多个测试运行时失败，极可能其他测试干扰了它。可用 `beforeEach` 清除共享状态，修复它。若你不确定是否共享状态被修改，可在 `beforeEach` 打印状态。

## REF

- [Setup and Teardown - Jest][docs]

[docs]: https://facebook.github.io/jest/docs/en/setup-teardown.html