---
title: 字符管-includes
date: 2020-11-22 12:57:16
tags: 'includes'
categories:
- 前端
- 字符串
---

`includes()` 方法用于判断一个字符串是否包含在另一个字符串中，根据情况返回 `true` 或 `false`。

## 语法

> `str.includes(searchString[, position])`

### 参数

- `searchString`
  要在此字符串中搜索等字符串
- `position` （可选）
  从当前字符串的哪个索引位置开始搜寻子字符串，默认值为 0。

### 返回值

如果当前字符串包含被搜寻的字符串，就返回 `true`; 否则返回 `false`.

### 区分大小写

`includes()` 方法是区分大小写的。例如，下面的表达式会返回 `false`.

```js
'Blue Whale'.includes('blue'); // return false
```

## Polyfill

```js
if (!String.prototype.includes) {
  String.prototype.includes = function (search, start) {
    'use strict';
    if (typeof start !== 'number') {
      start = 0;
    }

    if (start + search.length > this.length) {
      return false;
    } else {
      return this.indexOf(search, start) !== -1;
    }
  }
}
```

## 支持分隔符

```js
function includes (target, str, separator) {
  return separator ?
          (separator + target + separator).indexOf(separator + str + separator) > -1 :
          target.indexOf(str) > -1;
}
```

## [https://developer.cdn.mozilla.net/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/includes](https://developer.cdn.mozilla.net/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/includes)
