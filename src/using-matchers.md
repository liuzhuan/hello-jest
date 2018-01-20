# 使用匹配器

Jest 使用“匹配器”让你使用不同方式测试数值。匹配器数量众多，这里仅列举最常用的。

## 普通匹配器

最简单的方式是判断是否相等。

```js
test('two plus two is four', () => {
  expect(2 + 2).toBe(4)
})
```

上面代码中，`expect(2 + 2)` 返回一个 `expectation` 对象。通常，只需对它执行匹配器即可。`.toBe(4)` 就是匹配器。当 Jest 执行时，它会追踪所有失败的匹配器，因此就可以打印出可读性很好的报错信息。

`toBe` 使用 [`Object.is`][is] 判断是否全等。如果你想判断对象的数值是否相等，可以使用 `toEqual`：

```js
test('object assignment', () => {
  const data = { one: 1 }
  data['two'] = 2
  exepct(data).toEqual({ one: 1, two: 2 })
})
```

`toEqual` 会递归的检测对象或数组的每个字段。

你还可以检测匹配器的反义词：

```js
test('adding positive numbers is not zero', () => {
  for (let a = 1; a < 10; a++) {
    for (let b = 1; b < 10; b++) {
      expect(a + b).not.toBe(0)
    }
  }
})
```

## 真值测试

在某些测试中，需要区分 `null`, `undefined`, `false`，有时却不需要。Jest 有辅助函数，帮你明确自己的需求：

- `toBeNull` 只匹配 `null`
- `toBeUndefined` 只匹配 `undefined`
- `toBeDefined` 是 `toBeUndefined` 反义词
- `toBeTruthy` 匹配任何 `if` 语句当作真值的表达式
- `toBeFalsy` 匹配任何 `if` 语句当作假值的表达式

比如：

```js
test('null', () => {
  const n = null
  expect(n).toBeNull()
  expect(n).toBeDefined()
  expect(n).not.toBeUndefined()
  expect(n).not.toBeTruthy()
  expect(n).toBeFalsy()
})

test('zero', () => {
  const z = 0
  expect(z).not.toBeNull()
  expect(z).toBeDefined()
  expect(z).not.toBeUndefined()
  expect(z).not.toBeTruthy()
  expect(z).toBeFalsy()
})
```

应该使用最贴合代码场景的匹配器。

## 数字

大部分的数字比较功能都有对应的匹配器

```js
test('two plus two', () => {
  const value = 2 + 2
  expect(value).toBeGreaterThan(3)
  expect(value).toBeGreaterThanOrEqual(3.5)
  expect(value).toBeLessThan(5)
  expect(value).toBeLessThanOrEqual(4.5)

  // toBe or toEqual are equivalent for numbers
  expect(value).toBe(4)
  expect(value).toEqual(4)
})
```

若要匹配浮点数相等，需要使用 `toBeCloseTo` 代替 `toEqual`，防止浮点数的微小误差导致的错误。

```js
test('adding floating point numbers', () => {
  const value = 0.1 + 0.2
  // expect(value).toBe(0.3)
  expect(value).toBeCloseTo(0.3)
})
```

## 字符串

可用 `toMatch` 对字符串进行正则匹配：

```js
test('there is no I in the team', () => {
  expect('team').not.toMatch(/I/)
})

test('but there is a "stop" in Christoph', () => {
  expect('Christoph').toMatch(/stop/)
})
```

## 数组

可以使用 `toContain` 判断数组是否包含某一元素：

```js
const shoppingList = [
  'diapers',
  'kleenx',
  'trash bags',
  'paper towels',
  'beer'
]

test('the shopping list has beer on it', () => {
  expect(shopplingList).toContain('beer')
})
```

> ❓ 貌似 `toContain` 只可检测数值类型，无法检测引用类型？

## 异常

如果想检测某一函数调用时是否抛出异常，使用 `toThrow` 匹配器：

```js
function compileAndroidCode() {
  throw new ConfigError('you are using the wrong JDK')
}

test('compiling android goes as expected', () => {
  expect(compileAndroidCode).toThrow()
  expect(compileAndroidCode).toThrow(ConfigError)

  // 还可以使用报错文本或正则表达式
  expect(compileAndroidCode).toThrow('you are using the wrong JDK')
  expect(compileAndroidCode).toThrow(/JDK/)
})
```

## REF

- [Using Matchers - Jest][matchers]

[matchers]: https://facebook.github.io/jest/docs/en/using-matchers.html
[is]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is