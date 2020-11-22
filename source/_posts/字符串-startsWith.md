---
title: 字符串-startsWith
date: 2020-11-22 13:35:44
tags: 'startsWith'
categories:
- 前端
- 字符串
---

`startsWith()` 方法用来判断当前字符串是否以另外一个给定的字符串开头，并根据判断结果返回 `true` 和 `false`。

```js
const str1 = 'Saturday night plans'

console.log(str1.starstWith('Sat')) // true

console.log(str1.startsWith('Sat', 3)) // false
```

## 语法

> `str.startWith(searchString[, position])`

### 参数

- `searchString`
  要搜索的字符串
- `position`（可选）
  在 `str` 中搜索 `searchString` 的开始位置，默认为 0.

### 返回值

如果字符串的开头找到了给定的字符串则返回 `true`; 否则返回 `false`.

## Polyfill

```js
if (!String.prototype.startsWith) {
  Object.defineProperty(String.prototype, 'startsWith', {
    value: function (search, pos) {
      pos = !pos || pos < 0 ? 0 : +pos;
      return this.substring(pos, pos + search.length) === search
    }
  })
}
```
