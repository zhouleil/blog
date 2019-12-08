---
title: sortBy
date: 2019-12-08 13:52:02
tags:
categories:
- 前端
- 其他
---

## 数组内对象按属性排序

```javascript
const sortArr = [
    {
        value: 1,
        year: 2001,
        age: 18
    },
    {
        value: 3,
        year: 2002,
        age: 17
    },
    {
        value: 3,
        year: 1998,
        age: 21
    },
    {
        value: 3,
        year: 1998,
        age: 25
    },
    {
        value: 8,
        year: 1995,
        age: 24
    },
    {
        value: 8,
        year: 1997,
        age: 22
    },
    {
        value: 8,
        year: 1996,
        age: 22
    },
    {
        value: 8,
        year: 1997,
        age: 29
    }
]

const sortFn = function (arr) {
  function sortBy (pre, next, sortArr, index) {
    const { key, type, isLast } = sortArr[index]
    if (pre[key] > next[key]) {
      return type === 'desc' ? -1 : 1
    }
    if (pre[key] < next[key]) {
      return type === 'desc' ? 1 : -1
    }
    if (isLast) {
      return 0
    } else {
      return sortBy(pre, next, sortArr, index + 1)
    }
  }  
  return function (pre, next) {
    const index = 0
    return sortBy(pre, next, arr, index)
  }
}
// 1、对 sortArr 按照 value 倒序排序
const sortWithValue = sortArr.sort(sortFn([{ key: 'value', type: 'desc' ,isLast: true }]))
console.log(sortWithValue)

// 2. 对 sortArr 按照， value 倒序， value 相同时按照 year 倒序， year 相同时按照 age 正序

const sortResult = sortArr.sort(sortFn(
    [
        {
            key: 'value',
            type: 'desc',
            isLast: false
        },
        {
            key: 'year',
            type: 'desc',
            isLast: false
        },
        {
            key: 'age',
            type: 'asc',
            isLast: true
        }
    ]
))

console.log(sortResult)

```

第一种排序结果如下图

{% asset_img sort1.png This is an example image %}

第二种排序结果如下图
{% asset_img sort2.png This is an example image %}
