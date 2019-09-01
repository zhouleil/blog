---

title: babel简介
date: 2019-09-01 11:39:27
tags:
categories:
- 前端
---

Babel 是一个转码器，可以将ES6转化为ES5代码，从而在现有环境运行。

```javascript
// 转码前
let arr = [1,2,3,4,5];
arr.map(item => item + 1);

// 转码后
var arr = [1,2,3,4,5];
arr.map(function(item) {
  return item + 1;
})
```

上面的代码使用了箭头函数，这个特性还没有得到广泛的支持，Bable 将其转化为普通函数，就能在现有的 Javascript 环境执行了。



- @babel/core: 这个是babel 的核心包，核心的API都在这里。
- @babel/cli: 这是一个通过命令对js文件进行转换的工具。
- @babel/preset-env: 指定转换的工作环境，他是以前 es2015、es2016、es2017的集合，需要注意的是，@babel/preset-env不支持所有在stage-x的plugin。
- @babel/polyfill: 相当于一个填充，因为Babel本身只支持转换箭头函数、解构赋值这些语法糖类的语法，而一些新的API或者Promise函数等是无法转换的。 @babel/polyfill 就是解决这个问题的。全局的polyfill，而且很全能，只要不介意这个包的大小，一般建议使用该插件。在大型web项目中比较适合使用。
- @babel/plugin-transform-runtime：相对于 @babel/polyfill 的全局polypill, @babel/plugin-transform-runtime 则是按需引入，使用到哪些polyfill就引入哪些，但是不是全局的，避免了全局污染。开发框架或者库的时候适合使用。



#### 配置文件.babelrc/babel.config.js

之前版本的babel都是使用`.baberc`来做配置文件，babel7引入了`babel.config.js`。但是它并不是`.baberc`的替代品，二者根据使用的场景不同自行选择。

**.babelrc**

```javascript
{
  "presets": ["@babel/preset-flow","@babel/preset-react", "@babel/preset-typescript"],
  "plugins": [...]
}
```

@babel/preset-flow预设包含了类型检测插件

@babel/preset-react：顾名思义，包含了react语法的插件。

@babel/preset-typescript：转换ts语法

**.Babel.config.js**

```javascript
module.exports = function () {
  const presets = [ 
      ["env", {
            "targets": { //指定要转译到哪个环境
                //浏览器环境
                "browsers": ["last 2 versions", "safari >= 7"],
                //node环境
                "node": "6.10", //"current"  使用当前版本的node
                
            },
             //是否将ES6的模块化语法转译成其他类型
             //参数："amd" | "umd" | "systemjs" | "commonjs" | false，默认为'commonjs'
            "modules": 'commonjs',
            //是否进行debug操作，会在控制台打印出所有插件中的log，已经插件的版本
            "debug": false,
            //强制开启某些模块，默认为[]
            "include": ["transform-es2015-arrow-functions"],
            //禁用某些模块，默认为[]
            "exclude": ["transform-es2015-for-of"],
            //babel / preset-env处理polyfill的方式。
            //参数：usage | entry | false，默认为false.
            "useBuiltIns": false
     }]
 ];
  const plugins = [ "@babel/transform-arrow-functions" ];

  return {
    presets,
    plugins
  };
}
```



------

**参考**:

 [Babel 入门教程](http://www.ruanyifeng.com/blog/2016/01/babel.html) 

 [初识babel 7](https://www.jianshu.com/p/0ea6065cb39e)