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

## REF

- [Using Matchers - Jest][matchers]

[matchers]: https://facebook.github.io/jest/docs/en/using-matchers.html
[is]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is