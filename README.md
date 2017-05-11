# React教程

## 目录
  1. [介绍](#介绍)
  2. [Hello World](#hello-world)

## 介绍
![React](./img/logo.svg)
本文译自[React官方文档](https://facebook.github.io/react/docs/hello-world.html)

## hello-world

### Hello World
开始学习React最简单的方式是参看CodePen上的[Hello World实例](http://codepen.io/gaearon/pen/ZpvBNJ?editors=0010)。
您不需要安装任何东西，只要在浏览器中打开该链接并随我们学习该例子。如果想要使用本地开发环境，查看[安装页面](https://facebook.github.io/react/docs/installation.html)。
最简单的React实例如下：
```javascript
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```
这段代码在页面上渲染了一个Hello World的header。
接下来将逐步向您介绍如何使用React。我们将了解React程序的构建块-元素和组件。
掌握他们后，您将可以通过多块小段的可复用代码创建出复杂的应用。

### 预备知识

React是一个JavaScript库，您需要明白JavaScript的基本语法。
在例子中，我们也会使用一些ES6的语言特性。由于其相对较新，我们会保守的使用它。
但您最好熟悉[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，[类](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)，[模板字符串](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/template_strings)，```let```和```const```声明。
您可以通过[Babel REPL](http://babeljs.io/repl/)查看ES6的编译结果。