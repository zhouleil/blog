---
title: React生命周期
date: 2020-06-22 23:28:13
tags:
categories:
- 前端
- React
---



## React 生命周期15、16 对比

|          | 15                        | 16                              |
| -------- | ------------------------- | ------------------------------- |
|          | constructor               | constructor                     |
| 挂载阶段 | componentWillMount        | static getDerivedStateFromProps |
|          | render                    | render                          |
|          | componentDIdMount         | componentDidMount               |
|          |                           |                                 |
|          | componentWillReceiveProps | static getDerivedStateFromProps |
|          | shouldComponentUpdate     | shouldComponentUpdate           |
| 更新阶段 | componentWillUpdate       | render                          |
|          | render                    | getSnapshotBeforeUpdate         |
|          | componentDidUpdate        | componentDidUpdate              |
|          |                           |                                 |
| 卸载阶段 | componentWillUnMount      | componentWillUnMount            |
