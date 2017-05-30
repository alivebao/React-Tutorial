# React教程

## 目录
  1. [介绍](#介绍)
  2. [Hello World](#hello-world)
  3. [学习JSX](#学习jsx)
  4. [渲染元素](#渲染元素)
  5. [组件和属性](#组件和属性)
  6. [状态和生命周期](#状态和生命周期)
  7. [事件处理](#事件处理)
  8. [条件渲染](#条件渲染)
  9. [列表和键](#列表和键)
  10. [表单](#表单)
  11. [状态提升](#状态提升)
  12. [组合与继承](#组合与继承)

## 介绍
本文译自[React官方文档](https://facebook.github.io/react/docs/hello-world.html)

![React](https://github.com/alivebao/React-Tutorial/raw/master/img/logo.jpg)

## hello-world

#### Hello World
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

#### 预备知识

React是一个JavaScript库，您需要明白JavaScript的基本语法。
在例子中，我们也会使用一些ES6的语言特性。由于其相对较新，我们会保守的使用它。
但您最好熟悉[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，[类](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)，[模板字符串](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/template_strings)，```let```和```const```声明。
您可以通过[Babel REPL](http://babeljs.io/repl/)查看ES6的编译结果。

## 学习jsx

首先看看以下代码段：
```javascript
const element = <h1>Hello, world!</h1>;
```
这个有趣的标签表达式既不是一个字符串，也不是一个HTML标签。
它叫做JSX，是JavaScript的扩展。我们推荐在React中使用JSX描述UI。
JSX也许会使你想起某种模板语言，它可以充分利用JavaScript的特性。
JSX创建React元素。我们将再下一章节学习如何将其渲染至DOM。接下来，我们将学习JSX的基本用法。

#### 在JSX中嵌入表达式
在JSX中，您可以通过 *{}* 的方式嵌入任何JavaScript表达式。
如 *2+2*, *user.firstName*, 和 *formatName(user)* 等均为有效的表达式：
```javascript
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/PGEjdG?editors=0010)

从可读性的角度考虑，我们将JSX分成了多行。但这并不是强制性的。
当需要分行时，我们推荐使用圆括号 *()* 将其括起，从而避免JS的[自动插入机制ASI](http://stackoverflow.com/questions/2846283/what-are-the-rules-for-javascripts-automatic-semicolon-insertion-asi)

#### JSX也是表达式
通过编译后，JSX表达式变成了常规的JavaScript对象。
这意味着您可以使用JSX中使用 *if* 语句和 *for* 循环等:
```javascript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

#### 使用JSX指定标签属性
您可以使用引号 *""* 将字符串指定为属性：
```javascript
const element = <div tabIndex="0"></div>;
```
也可以使用括号 *{}* 嵌入JavaScript表达式作为属性：
```javascript
const element = <img src={user.avatarUrl}></img>;
```
但是，__当通过JavaScript表达式为属性赋值时，不要使用双引号。__
否则，JSX会将其 __视为一个字符串而非JavaScript表达式__。
对于字符串，我们可以直接使用 *""* 将其作为属性值；对于表达式，我们可以通过 *{}* 将其作为属性值。
但是 __不要将其混用__ 。

#### 使用JSX指定子元素
当某tag不含值时，可以直接用 */>* 将其关闭：
```javascript
const element = <img src={user.avatarUrl} />;
```
JSX的标记可以包含子元素：
```javascript
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```
__注意，由于相对HTML而言，JSX更加类似于JavaScript, React DOM使用驼峰命名代替HTML中的属性名。__
__例如，JSX中使用 *className* 而非 *class* , 使用 *tabIndex* 而非 *tabindex* __

#### JSX能够防注入攻击
在JSX中直接嵌入用户输入是安全的：
```javascript
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```
在执行渲染前，React DOM会默认对任何嵌入的值进行编码。
这可以避免应用被注入，可以避免XSS攻击。

#### JSX表示对象
Babel将JSX编译为 *React.createElement()* 调用。
以下两种写法是一致的：
```javascript
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```
```javascript
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```
*React.createElement()* 会执行一些语法方面的检查，但其核心功能是创建一个如下的对象：
```javascript
const element = {
  ...
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
  ...
};
```
这些对象被称为"React元素"。我们可以将其视为我们想要在屏幕上表现的元素的描述。React读取这些对象并利用他们构建DOM并保持更新。
我们将在下一部分学习将React元素渲染成DOM。

Tip：
__为了ES6和JSX都能在编辑器中高亮显示，我们推荐您将您的编辑器设置为"Babel"语法方案。__

## 渲染元素

在React应用中，元素是最小的构建单位。  
元素用于描述你想在屏幕上看到的东西：
```javascript
const element = <h1>Hello, world</h1>;
```
不同于浏览器的DOM元素，React元素是简单对象(Plainj Object)，并且创建代价很小。React DOM负责更新DOM，使其与React元素的一致。  

*注意：*
*一个更广为人知的概念-"组件(Components)"可能会让你与"元素"混淆。我们将在下一部分介绍组件。元素是用于建立组件的"原料"之一*

#### 将元素渲染为DOM
假设在你的HTML文件中的某处有一个div:
```html
<div id="root"></div>
```
我们将其称为根元素-其内部所有事物都将由React DOM进行管理。  
采用React创建的应用通常有一个根DOM节点。如果是想要将React结合入某个已有的应用，您可以根据需要创建任意多个根DOM节点。
利用ReactDOM.render()将一个React元素渲染至根DOM节点：
```javascript
const element = <h1>Hello, World</h1>;
ReactDOM.render(
  element,
  document.getElementById('root');
);
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/rrpgNB?editors=1010)

这段代码会在页面上显示"Hello World"

#### 更新被渲染的元素
React元素是的不可变的。一旦创建完成一个元素后，您无法改变其子元素或属性。元素就好比是电影中的某一帧：其代表了某个时间点的UI。
根据我们至今所学，更新UI的唯一方式是创建一个新元素并将其传递个ReactDOM.render():
如下面这个时钟实例所示：
```javascript
function tick(){
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/gwoJZk?editors=0010)
其通过setInterval()每隔一秒调用一次ReactDOM.render()

*注意：*
*实际上，大多React应用只调用一次ReactDOM.render()。在下一部分我们将学习如何将这些代码封装至有状态的组件*

#### React只更新需要更新的部分
React DOM会将元素及其子元素与先前状态进行比较，并且只会更新状态变动的元素。
你可以通过浏览器工具查看[上一个实例](http://codepen.io/gaearon/pen/gwoJZk?editors=0010)

![tick](http://7xv88e.com1.z0.glb.clouddn.com/granular-dom-updates.gif)

尽管我们每秒都创建了一个用于描述整个UI树的元素，但只有文本节点的内容被React DOM更新了。
根据我们的经验，"考虑UI全部时刻的状态而不是如何随时间改变它"能够避免许多问题。

## 组件和属性

组件使你能够将UI划分为独立的，可复用的个体。  
从概念上来说，组件类似于JavaScript中的函数。  
他们接受任意输入(这些输入被称为属性-props)并返回React元素，描述其在屏幕上应该如何显示。  

#### 函数式组件和类组件
定义组件最简单的方式是写一个JavaScript函数：
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
这个函数是一个有效的React组件-其接受一个单一的，拥有数据的``props``对象作为参数并返回一个React元素。

由于他们在字面上看来是函数，我们将这类组件成为"函数式组件"(Functional)  
您也可以使用ES6的[class](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)定义一个组件：
```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
以上两种方式对React而言是一样的。  
我们将在下一部分讨论class拥有的一些额外的特性。  
在此之前，由于函数式组件更简洁，我们将使用函数式组件。  

#### 渲染一个组件
之前，我们只遇到过代表DOM标签的React元素：
```javascript
const element = <div />;
```
然而，元素也能够代表用户定义的组件：
```javascript
const element = <Welcome name="Sara" />;
```
当React发现某个代表用户定义组件的元素时，它将把JSX的属性作为一个单独的对象传递给这个组件。  
我们将这个对象称为"属性(props)"。  
比如，以下代码将在页面上渲染"Hello, Sara": 
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/YGYmEG?editors=0010)  
在这个例子中：  
1. 我们将``<Welcome name="Sara" />``作为参数调用了``ReactDOM.render()``
2. React将``{name: 'Sara'}``作为属性调用了Welcome组件
3. Welcome组件返回一个``<h1>Hello, Sara</h1>``元素
4. React DOM高效的更新DOM-使其与``<h1>Hello, Sara</h1>``匹配

__注意__  
__为组件命名时首字母应大写。__  
__比如说，``<div />``代表一个DOM标签，但``<Welcome />``则代表一个组件。__  

#### 构建组件
组件可以在输出中引用其他组件，这使我们能够对任何级别的粒度划分执行组件抽象。一个按钮、一个表单，一个对话框-在React应用中，这些通常都被作为一个组件。
例如，我们可以创建一个App组件用于多次渲染``Welcome``：
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/KgQKPr?editors=0010)  
通常来说，新的React应用在最顶层有一个App组件。但是，如果您是将React引入至某个现有的应用中，你从可以使用如Button这样的小组件开始，自底向上逐步替换。

__注意__  
__组件比如返回一个单独的根元素。这也是我们为``<Welcome />``元素外面包裹上一层``<div>``的原因__

#### 分解组件
不要担心将组件分割成更细粒度的组件。  
例如，考虑如下组件：
```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/VKQwEo?editors=0010)  
该组件接受"作者(一个对象)", "文字(一个字符串)", 和一个"日期"作为属性，并在一个社交媒体网站上描述了评论。  
由于过多的嵌套，这个组件难以被替换和复用。我们可以考虑对其进行分解。  
首先，我们可以提取出Avatar：  
```javascript
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```
我们将``user``(一个更通用的名字)而非``author``作为其属性-Avatar不需要知道其是否是用于Comment的  
我们建议从组件的角度而非其具体的应用场景对组件的属性进行命名。  
现在可以对``Comment``组件做一下简化：  
```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
接下来，我们接着提取出一个``UserInfo``组件-该组件用于将``Avatar``组件放在用户旁边：  
```javascript
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
```
现在Comment组件可以进一步简化：  
```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/rrJNJY?editors=0010)  
提取组件的工作在开始时看起来意义不大，但在大型应用中，一个能复用的组件库能够大大的减轻开发成本。  
一个好的经验是-如果你的某个组件要用到几次(如按钮，Avatar等)，或其拥有一定的复杂度(App, FeedStory, Commen等)，则有必要考虑是否需要将其作为可复用组件进行提取。  

#### 组件的属性是只读的
无论将组件声明为一个函数或一个类，其属性均是不可变的。考虑以下函数``sum``：  
```javascript
function sum(a, b) {
  return a + b;
}
```
这种函数被称为纯函数-他们不企图改变输入的值；对于相同的输入，总是返回相同的结果。  
作为对比，下面这个函数不是纯函数(其改变了他的输入)：  
```javascript
function withdraw(account, amount) {
  account.total -= amount;
}
```
React非常灵活，但它有一个严格的规定：  
__所有的React组件必须要像纯函数一样去接受他们的props__  
当然，应用的UI是动态的，会随着时间改变。在下一部分，我们将介绍一个新的概念-状态(``state``)。不同于属性，状态允许组件对其进行修改。  

## 状态和生命周期

回顾之前学习的[时钟的例子](http://miaoyunze.com/2017/05/14/React-Tutorial-3-Rendering-Elements/)，元素用于描述我们想要在屏幕上看到的内容。  
我们调用``ReactDOM.render()``改变渲染输出：  
```javascript
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/gwoJZk?editors=0010)  
在本部分，我们将学习如何使``Clock``组件变得可重用，它会建立自己的定时器并每秒自动进行更新。  
我们可以先从封装``Clock``的样式着手：  
```javascript
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/dpdoYR?editors=0010)  
但是，这里漏了一个很重要的地方："``Clock``建立一个定时器并每秒更新UI"这一行为事实上应该是由Clock自己实现的(而不是将new Date作为属性传入)。  
理想情况下它应该是这样的：  
```javascript
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
为了完成这个目的，我们应该为``Clock``组件增加__状态(state)__。  
状态与属性(props)类似，但它是私有的，且完全由组件控制。  
我们[之前提到过](http://miaoyunze.com/2017/05/16/React-Tutorial-4-Componenets-and-Props/)，被定义为class的组件拥有额外的特性，本地状态就是其中之一-一个只有类组件拥有的特性。  

#### 将函数式组件转换转为类组件
我们可以通过5步将``Clock``组件从函数式转换为类组件：  
1. 创建一个继承自``React.Component``的[ES6 class](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)
2. 为其增加一个叫做``render()``的方法
3. 将函数式组件的内容移至``render()``方法中
4. 在移动的过程中，将原来的``props``转为``this.props``
5. 删除剩余的空函数声明
```javascript
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/zKRGpo?editors=0010)  
``Clock``组件现在从函数式组件变成了类组件，这使我们能够使用诸如state和Lifecycle Hooks等额外属性。  

#### 为类组件增加本地状态
我们通过以下3个步骤将``date``从props变成state：  
1. 在``render()``中使用``this.state.date``替换``this.props.date``：
```javascript
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
2. 增加一个构造函数初始化``this.state``：
```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
  注意-我们向父类的构造函数传递了props:
  ```javascript
    constructor(props) {
      super(props);
      this.state = {date: new Date()};
    }
  ```
  类组件应该总是在构造函数中执行``super(props)``这一过程
3. 删除``<Clock />``中的``date``属性：
```javascript
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
我们会在稍后将定时器相关的代码增加至组件内部。  
目前替换后的结果如下：
```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/KgQpJd?editors=0010)  
接下来，我们要为``Clock``组件添加定时器使其每秒自动更新。

#### 为类组件增加生命周期方法
在有多个组件的应用中，组件销毁时及时释放其所占用的资源是非常有必要的。  
我们想要在``Clock``被渲染到DOM后立即建立一个定时器，这在React中是__mount__阶段。  
我们还想在当``Clock``创建的DOM被销毁时清除该定时器，这在React中是__unmounting__阶段。  
我们可以在组件类中定义一些特殊的方法使得组件在mount/unmount时执行一些操作：  
```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
这些方法被称为"Lifecycle Hooks"  
当一个组件被渲染至DOM时执行``componentDidMount``，这是一个设置定时器的好时机：
```javascript
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```
注意，我们在这里将定时器的ID保存在了``this``中。  
``this.props``由React自己建立，``this.state``有着特殊的意义-当有某些不是用于视觉输出的数据需要存储时，我们可以将其保存在class中。  
__如果这个数据不是用于``render()``中的，就不应该添加在state里。__  
在``componentWillUnmount``阶段，我们将销毁定时器：
```javascript
componentWillUnmount() {
  clearInterval(this.timerID);
}
```
最后，我们需要完成每秒都执行的``tick()``方法。  
该方法通过``this.setState()``更新组件的本地状态(local state)：
```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/amqdNA?editors=0010)  
现在这个时钟能够开始动起来了。  
让我们快速回顾一下发生了什么，以及各方法的调用顺序：  
1. 当``<Clock />``传递给``ReactDOM.render()``时，React调用了``<Clock />``的构造函数。
由于``Clock``要显示当前时间，其将``this.state``初始化为一个包含当前时间的对象。我们将在稍后更新该state。
2. React之后调用``Clock``组件的``render()``方法，根据该方法在屏幕上绘制UI。
3. 当``Clock``输出被插入DOM时，React调用``componentDidMount`` Life Hook。在该hook中``Clock``组件通知浏览器建立一个定时器并每秒调用一次``tick()``。
4. 浏览器每秒调用一次``tick()``方法。在该方法中，``Clock``组件通过调用``setState()``安排一次UI更新。
``setState()``的调用让React收到状态改变的通知，并再次执行``render()``方法了解如何在屏幕上绘制UI。这次，``render()``方法中的``this.state.date``发生了变化，因此渲染输出包含更新了的时间。React根据它来更新DOM。
5. 如果``<Clock />``组件从DOM中移除了，React会调用``componentWillUnmount`` Life Hook，定时器将会被停止。

#### 正确使用状态(State)
关于``setState()``，我们需要知道三点  
__不要直接改变state__  
例如，这种操作无法重新渲染一个组件：
```javascript
// Wrong
this.state.comment = 'Hello';
```
应该使用``setState()``：
```javascript
// Correct
this.setState({comment: 'Hello'});
```
我们唯一能直接给``this.state``赋值的地方是在组件的构造函数中。  

__状态更新可能是异步的__  
从性能的角度考虑，React可能会在某次单独的更新中批量执行多个``setState()``。  
由于``this.props``和``this.state``可能会异步更新，我们不应该依靠他们的值来计算下一步的状态。  
例如，以下代码可能无法准确的更新counter：  
```javascript
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```
为避免这个问题，可以使用``setState()``的另一种形式-接受函数而不是对象。传递给setState的函数将之前的状态作为第一个参数，需增加的属性作为第二个参数：  
```javascript
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```
我们在这里使用了[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，这么写也是一样的：  
```javascript
// Correct
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```

__状态更新是合并更新__  
调用``setState()``时，React将我们提供的对象合并至当前状态。  
例如，我们的state可能包含多个独立的变量：
```javascript
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```
然后我们可以使用单独使用``setState()``分别更新它们：
```javascript
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```
__调用``this.setState({comments})``不会修改this.state中posts的值，而是仅仅修改``this.state.comments``的内容。__  

#### 数据流向下
父组件或子组件都无法知道某组件是否是有状态的，它们也不应该组件是函数式组件还是类组件。  
这也是为什么state通常是被本地调用或封装-它只能被拥有并建立它的组件访问，其他组件均不能访问它。  
组件可以将state作为props传递给子组件：
```javascript
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
```
也可以这么做：
```javascript
<FormattedDate date={this.state.date} />
```
``FormattedDate``组件将``date``作为其属性，但并不知道这个date的来源(是来自``Clock``的状态、``Clock``的属性或手动设置的)：  
```javascript
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/zKRqNB?editors=0010)  
这通常被称为“自顶向下”或“单向”数据量。任何state都是由某个特定的组件拥有的，并且继承自该状态的任何数据和UI只能影响在他们组件树之下的组件。  
将组件树想象为一个属性的瀑布，每个组件的状态(state)就好比是在任意节点中加入的，同样是自上而下的额外水源。  
为了表示所有的组件都是真正隔离的，我们可以创建一个``App``组件渲染三个``<Clock>``：  
```javascript
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/vXdGmd?editors=0010)  
每个``Clock``建立自己的定时器并独立更新。  
在React应用中，组件是否有状态被认为是组件的实现细节，其可能会随着时间而改变。我们可以在有状态的组件中使用无状态的组件，反之亦然。

## 事件处理

React元素的事件处理与DOM元素很类似，但有一些语法差异：
* React事件采用驼峰命名
* 使用JSX，我们可以直接传递函数而非字符串作为事件处理程序

例如，HTML：
```html
<button onclick="activateLasers()">
  Activate Lasers
</button>
```
这在React中稍有不同：
```html
<button onClick={activateLasers}>
  Activate Lasers
</button>
```
另一个不同之处在于，React中我们无法通过返回``false``阻止默认行为。  
我们必须显式的调用``preventDefault``。例如，在纯HTML中，为防止链接打开新页面的默认行为，我们可以这么写：
```html
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```
在React中的写法：
```javascript
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```
这里的``e``是一个事件元素。React根据[W3规范](https://www.w3.org/TR/DOM-Level-3-Events/)定义这一事件，因此我们无需担心浏览器的兼容问题。  
在使用React时，在一个DOM创建后，需要为其增加监听事件时，我们通常不采用``addEventListener``，而是在元素最初被渲染时提供一个监听器。  
当我们采用一个[ES6 class](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)定义一个组件时，通常的做法是把事件处理函数作为该类的一个方法。  
例如，下面这个``Toggle``组件渲染了一个点击切换开关状态的按钮：
```javascript
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/xEmzGg?editors=0010)。  
__注意JSX回调中的``this``。__  
在JavaScript中，类方法不会默认绑定this。  
如果忘记绑定``this.handleClick``而将其直接传递给``onClick``，当函数被调用时，``this``的值是``undefined``的。  
这并不是React的特有行为，具体可参看[这篇文章](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/).  
通常来说，如果我们引用一个没有``()``的方法，如``onClick = {this.handleClick}``，我们应该绑定该方法。  
如果不想采用``bind``的方式，这里有另外两种选择。  
如果你在使用 __实验性__ 的[初始化语法](https://babeljs.io/docs/plugins/transform-class-properties/)，则可以使用属性初始化器完成正确的回调绑定：
```javascript
class LoggingButton extends React.Component {
  // 这个表达式确保this绑在handleClick中
  // 注意!!! 这个是实验性语法
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```
另一种方法是在回调中使用[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)：
```javascript
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // 这个表达式确保this绑在handleClick中
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```
使用箭头函数的问题在于这种方式会导致每次渲染``LogginButton``时都会创建一个不同的回调函数。  
在大多情况下，这么做没什么问题。但是，如果这个回调函数是作为属性被传递给更底层的组件时，该组件可能会被影响，造成一次额外的重新渲染。  
为避免这种问题的发生，我们推荐采用在构造函数中手动绑定或使用属性初始化语法。

## 条件渲染

在React中，我们可以创建多种封装我们需要的行为的组件。然后，我们可以根据应用的状态，只渲染其中的一部分。  
React的条件渲染与JS中的条件判断相同。使用JS中的``if``或[条件操作符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)创建表示当前状态的元素，然后让React更新UI以匹配它们。  
考虑以下两个组件：
```javascript
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```
我们将创建一个``Greeting``组件，其状态取决于用户是否登录：
```javascript
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/ZpVxNq?editors=0011)。  
这里根据属性``isLoggedIn``的值显示出不同的问候语。  

#### 元素变量
我们可以使用变量保存元素。这可以帮助我们在输出的其他部分不变的情况下，有条件的渲染组件的一部分。  
考虑以下两个表示登录/登出的组件：
```javascript
function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}
```
在下例中，我们会创建一个有状态的组件-``LoginControl``。  
该组件将根据其现有的状态渲染成``<LoginButton />``或``<LogoutButton />``，同时也会渲染先前例子中的``<Greeting />``：
```javascript
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;

    let button = null;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/QKzAgB?editors=0010)。  
虽然声明一个变量并用``if``表达式是一种有条件的渲染组件的好方法，但有时我们可以采用更简洁的表达式。  
如下所述，JSX中有几种方法。

#### 内联if和逻辑与操作符&&
我们可以用``{}``在JSX中嵌入任意表达式，这当然也包括JS中的``&&``。这在某些情况下能非常方便的执行有条件的渲染：
```javascript
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/ozJddz?editors=0010)。  
这是因为在JS中，``true && expression``的结果取决于``expression``, ``false && expression``的结果总是``false``。  
因此，如果条件是``true``，就会输出在``&&``之后的元素(也就是``<h2>You have ... </h2>``)，否则React就会忽略它。  

#### 内联 if-else 和条件操作符
另一种有条件渲染渲染元素的方式是使用JS中的条件表达式``condition ? true : false``。  
在下例中，我们将使用该表达式渲染一段文本：
```javascript
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```
它也可以用于更大的表达式，尽管看起来有些不太直观：
```html
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```
具体选择哪种方式取决于你和你的团队。  
__同时也别忘了，当条件表达式变得过于复杂，这通常意味着是时候对组件进行分解了。__  

#### 防止组件呈现
在少数情况下，我们也许会需要组件隐藏它自己(即使这个组件是由另一个组件呈现的)。为达到该目的，应在组件的``render``方法中返回``null``而不是其渲染输出。  
在下例中，``<WarningBanner />``是否渲染取决于属性值``warn``。如果其为``false``，该组件不会被渲染：
```javascript
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true}
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(prevState => ({
      showWarning: !prevState.showWarning
    }));
  }

  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}

ReactDOM.render(
  <Page />,
  document.getElementById('root')
);
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/Xjoqwm?editors=0010)。  
在组件的``render``方法中返回``null``不会影响组件生命周期方法的触发。比如，``componentWillUpdate``和``componentDidUpdate``仍将被调用。

## 列表和键

首先让我们来回顾一下在JavaScript如何转换列表。  
在以下代码段中，我们使用``map()``函数，为其传入一个``numbers``数组，将数组的值翻倍并打印出来：
```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```
这段代码在控制台中会打印出``[2, 4, 6, 8, 10]``。  
在React中，将数组转为元素列表的操作基本类似。  

#### 渲染多个组件
我们可以建立元素集合并用``{}``将他们包含在JSX中。  
以下代码中，我们使用``map()``遍历``numbers``数组，对其中的每个元素，返回一个``<li>``最后将生成的元素数组赋值给``listItems``：
```javascript
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```
我们使用``<ul>``包裹``listItems``并将其渲染至DOM:
```javascript
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/GjPyQr?editors=0011)。  
这段代码显示了一段1~5的列表。  

#### 基本的list组件
通常我们会将list渲染至某个组件中。  
我们可以将之前的例子重构为一个接受一个数组``numbers``并输出一个无序元素列表的组件。
```javascript
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
执行这段代码时会收到一个警告-对list的各列表项需要提供一个key。``key``是创建元素列表时需要包含的特殊字符串属性。我们将在下一部分讨论它为什么重要。  
为``numbers.map()``中的列表项分配一个key：
```javascript
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/jrXYRR?editors=0011)。  

#### 键(Keys)
keys用于帮助React识别出具体是哪个item发生了变化，它应该被赋给元素数组中的各元素以作为其标识：
```javascript
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```
选择key的最佳实践是使用能够唯一标识各元素的字符串。大多情况下可以使用数据的ID作为key：
```javascript
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```
数据不一定有id属性时，也可以用item的索引替代：
```javascript
const todoItems = todos.map((todo, index) =>
  <li key={index}>
    {todo.text}
  </li>
);
```
在item可以重新排序的情况下，不建议使用index作为key-这样会很慢。  

#### 利用key分解组件
key在数组的上下文中才有意义。  
例如，要分解一个``listItem``组件，应该将key保留在数组中的``<ListItem />``元素上而不是``<li>``中。  
__例：错误的使用方式__  
```javascript
function ListItem(props) {
  const value = props.value;
  return (
    // Wrong! There is no need to specify the key here:
    <li key={value.toString()}>
      {value}
    </li>
  );
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Wrong! The key should have been specified here:
    <ListItem value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
正确的使用方式：
```javascript
function ListItem(props) {
  // Correct! There is no need to specify the key here:
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Correct! Key should be specified inside the array.
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
[在CodePen中尝试](https://codepen.io/rthor/pen/QKzJKG?editors=0010)。  
这里有一个很好的经验法则-``map()``调用中的元素需要key

#### key在兄弟中必须是唯一的
数组中使用的key在兄弟中必须是唯一的。但是，他们不需要 __全局唯一__ 。两个不同的数组中的key可以相同：
```javascript
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
);
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/NRZYGN?editors=0010)。  
key的是供React使用的，但并不会传递给组件。在组件中若需要同样的值，可以取一个不同的名字并将其作为属性显式传递：
```javascript
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
);
```
在上例中，``Post``组件可以读到``props.id``，但无法读到``props.key``。

#### 在JSX中嵌入map()
上例中声明了一个单独的``listItem``变量并将其包含在JSX中：
```javascript
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
```
JSX允许在``{}``中嵌入表达式，因此可以内联``map()``的输出：
```javascript
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/BLvYrB?editors=0010)。  
这么做在一些情况下能使代码更清晰，但有时也会造成麻烦。与JavaScript一样，是否需要为可读性提取变量取决于你。  
注意，如果``map()``的嵌套层级过多，也许这说明是时候分解组件了。

## 表单

在React中，HTML表单元素与其他DOM元素略有不同-表单元素具有一些内部状态。例如，以下HTML表单接收一个姓名：
```html
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```
用户提交表单时，该表单会执行HTML表单的默认行为。在React中，如果这就是我们想要的效果，那么不需要任何额外操作。  
但大多情况下，会需要使用JS函数处理用户输入的数据并执行提交动作。实现这一点的标准方式是使用一种称为“受控组件”的技术。

#### 受控组件
在HTML中， ``<input>``，``<textarea>``及``<select>``等表单元素均有自己的状态，并会根据用户的输入进行更新。  
在React中，可变状态通常存放在组件的state属性中，并通过``setState()``进行更新。  
我们可以结合以上两种方式，采用React的状态作为“单一数据源(single source of truth)”。  
渲染表单的React组件在用户输入时进行相应变化，控制组件的显示。采用这种方式，值由React控制的表单元素就叫做“受控组件”。  
例如，在上例中，如果想要在用户提交时打印其提交的内容，可将form写成受控组件：
```javascript
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/VmmPgp?editors=0010)。  
由于在表单元素中设置了``value``属性的赋值方式-``value={this.state.value}``，表单元素显示的值是``this.state.value``，使用了React的状态作为单一数据源。  
由于每次用户输入时都会执行``handleChange``更新React的state，元素显示的值也将随着用户的输入而更新。  
在受控组件中，状态每次的变化都会与处理函数(``handleChange``)相关联，这让我们有机会直接对用户的输入做一些操作，如直接将用户的输入自动转换为大写：
```javascript
handleChange(event) {
  this.setState({value: event.target.value.toUpperCase()});
}
```

#### textarea标签
在HTML中，``<textarea>``元素通过子节点定义其文本：
```javascript
<textarea>
  Hello there, this is some text in a text area
</textarea>
```
React中采用``value``属性进行替代。这种方式使得使用``<textarea>``标签与单行输入的``<input>``标签一样方便：
```javascript
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Please write an essay about your favorite DOM element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```
注意，``this.state.value``在构造函数中初始化，因此textarea在一开始就会有一些文本。  

#### select标签
在HTML中，``<select>``创建一个下拉列表。下列HTML创建了一个口味下拉列表：
```javascript
<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Coconut</option>
  <option value="mango">Mango</option>
</select>
```
这里Coconut是默认被选中的-该option中添加了``selected``属性。  
React中并不使用``selected``属性，而是仍采用为``value``赋值的方式，并将该属性置于select根标签中。这种方式在受控组件中更为方便，只需要在一个地方更新该值即可：
```javascript
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite La Croix flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/JbbEzX?editors=0010)。  
总而言之，采用这种方式使得使用``<input type-"text">``, ``<textarea>``和``<select>``都非常类似-它们均接受一个value属性，我们通过该属性实现一个受控组件。  

#### 处理多个输入
当需要处理多个受控``input``元素时，可以为每个元素增加一个``name``属性，在处理函数中根据``event.target.name``的值进行相应操作：
```javascript
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/wgedvV?editors=0010)。  
注意，这里使用了ES6中的[计算属性名](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names)表达式根据输入更新state中对应key的value：
```javascript
this.setState({
  [name]: value
});
```
这等同与ES5中的：
```javascript
var partialState = {};
partialState[name] = value;
this.setState(partialState);
```
另外，由于``setState()``是合并更新(可参看[状态与生命周期](http://miaoyunze.com/2017/05/16/React-Tutorial-5-State-and-Lifecycle/)-"状态更新是合并更新"这一小节)，我们只要更新我们需要更新的部分就可以了。  

#### 受控组件的替代方案
有时大量使用受控组件会很麻烦-对每种数据的更新都要写好相应的处理函数，并通过React组件管理所有输入状态。  
在这种情况下，或许我们可以尝试它的替代方案-[非受控组件](https://facebook.github.io/react/docs/uncontrolled-components.html)。

## 状态提升

有些情况下，多个组件需要多同一个数据的变化做出反应。我们推荐这时可以将该变量提升至这些组件共有的父节点中。  
在本部分，我们将创建一个接受给定温度的温度计，其会计算在给定温度下水是否被煮沸。  
从组件``BoilingBerdict``开始，该组件接受``celsisu``作为属性，并打印水是否被煮沸。  
```javascript
function BoilingVerdict(props) {
  if (props.celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water would not boil.</p>;
}
```
下一步，创建``Calculator``组件。其渲染一个接受温度输入的``<input>``，并在``this.state.temperature``中保持该值。  
另外，该组件根据当前的输入渲染``BoilingVerdict``:
```javascript
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    return (
      <fieldset>
        <legend>Enter temperature in Celsius:</legend>
        <input
          value={temperature}
          onChange={this.handleChange} />
        <BoilingVerdict
          celsius={parseFloat(temperature)} />
      </fieldset>
    );
  }
}
```
[在CodePen中尝试](http://codepen.io/valscion/pen/VpZJRZ?editors=0010)。  

#### 增加一个Input
现在有了一个新需求。除了摄氏度的input外，还需要提供一个华氏度的input，且要求两者保持一致。  
我们可以先从``Calculator``中提取出``TemperatureInput``组件，并新增一个``scale``属性，该属性的值为``"c"``或``"f"``：
```javascript
const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```
现在可以将``Calculator``修改为渲染两个不同的温度输入框：
```javascript
class Calculator extends React.Component {
  render() {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" />
      </div>
    );
  }
}
```
[在CodePen中尝试](http://codepen.io/valscion/pen/GWKbao?editors=0010)。  
现在我们有两个输入框，但当向其中一个输入框输入数据时，另一个并不会随之改变。而我们的需求是两个输入框保持同步。  
另外，在``Calculator``中也无法显示``BoilingVerdict``。这是因为当前温度的状态已经被隐藏至了``TemperatureInput``中，``Calculator``无法知道当前温度。

#### 编写转换函数
首先，我们将实现两个用于摄氏度与华氏度间转换的函数：
```javascript
function toCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5 / 9;
}

function toFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32;
}
```
这两个函数执行数值间的转换。我们还需要完成另一个函数，该函数接受两个参数-字符串类型的``temperature``和上面的两个函数之一。这个函数能够根据输入的温度值(字符串)计算另一个单位的温度值(字符串)。
输出的转换值保留小数点后三位，输入值无效时该函数返回一个空字符串：
```javascript
function tryConvert(temperature, convert) {
  const input = parseFloat(temperature);
  if (Number.isNaN(input)) {
    return '';
  }
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000;
  return rounded.toString();
}
```
例如，``tryConvert('abc', toCelsius)``将返回一个空字符串，``tryConvert('10.22', toFahrenheit)``将返回``'50.396'``。

#### 状态提升
现在，两个``TemperatureInput``组件都在他们自己的本地状态中保留各自的值：
```javascript
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
```
然而，我们希望两个输入框中的值彼此间同步。当我们更新摄氏度输入框的内容时，华氏度输入框应该改变成相对应的值，反之亦然。  
React中的状态共享通过将该状态提升至两个组件最近的公共祖先组件的状态完成。这被称之为 __"状态提升"__ 。我们从``TemperatureInput``中移除本地状态并将其放置于``Calculator``中。  
``Calculator``拥有这个被共享的状态后，该状态成为了两个温度输入框共享的"单一数据源"。这有助于保持两个输入框内容一致。由于两个``TemperatureInput``输入框的属性均来源于同一个父组件``Calculator``，因此两个输入框的内容将始终是同步的。  
首先。我们将``TemperatureInput``组件中的``this.state.temperature``替换为``this.props.temperature``。现在，假设``this.props.temperature``已经存在(接下来会看到，该值由``Calculator``传递进来)：
```javascript
render() {
    // Before: const temperature = this.state.temperature;
    const temperature = this.props.temperature;
```
我们知道属性是只读的。``temperature``作为本地状态时，``TemperatureInput``可以通过``setState``修改该状态。但当其作为属性被传入时，``TemperatureInput``没有修改该属性的能力。  
现在，当``TemperatureInput``想要修改``temperature``时，需调用``this.props.onTempatureChange``：
```javascript
 handleChange(e) {
    // Before: this.setState({temperature: e.target.value});
    this.props.onTemperatureChange(e.target.value);
```
注意，在我们自定义的组件中，``temperature``或``onTempatureChange``等属性名均不具有特殊含义。我们可以任意命名，如``value``和``onChange``等。  
``onTempatureChange``属性和``temperature``一样由父组件``Calculator``传入。该方法通过修改本地状态来处理变更，因此会采用新的值同时更新两个输入框。接下来我们马上将看到``Calculator``的具体实现。  
在修改``Calculator``前，让我们回顾一下对``TemperatureInput``组件的修改。我们从组件中移除了其本地状态，将``this.state.temperature``转成了``this.props.temperature``。之前通过``this.setState()``执行变更，现在转为通过由``Calculator``提供的``this.props.onTempatureChange``实现：
```javascript
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    this.props.onTemperatureChange(e.target.value);
  }

  render() {
    const temperature = this.props.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```
现在再来看``Calculator``组件。  
我们将当前输入框的``temperature``及该输入框的``计量单位``作为``Calculator``的本地状态，这个状态从输入框中提升而来，并作为两个输入框的"单一数据源"。  
例如，当我们在``Celsius``输入框中输入 37 时，``Calculator``的state是：
```javascript
{
  temperature: '37',
  scale: 'c'
}
```
随后如果我们在``Fahrenheit``中输入 212 ，``Calculator``中的state将变为：
```javascript
{
  temperature: '212',
  scale: 'f'
}
```
我们可以同时存储两个输入框中的值，但事实上并没有这个必要。存储最近被更新的输入框的值及该输入框所代表的单位即可。我们可以通过这两者计算出另一个输入框中应显示的内容。  
之所以说输入框同步，是因为他们的数据值均从同一个数据源计算而来：
```javascript
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {temperature: '', scale: 'c'};
  }

  handleCelsiusChange(temperature) {
    this.setState({scale: 'c', temperature});
  }

  handleFahrenheitChange(temperature) {
    this.setState({scale: 'f', temperature});
  }

  render() {
    const scale = this.state.scale;
    const temperature = this.state.temperature;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange} />
        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange} />
        <BoilingVerdict
          celsius={parseFloat(celsius)} />
      </div>
    );
  }
}
```
[在CodePen中尝试](http://codepen.io/valscion/pen/jBNjja?editors=0010)。  
现在，无论我们在输入框中输入什么，``Calculator``中的``this.state.temperature``和``this.state.scale``都会得到相应的更新。只要其中一个输入框的值被用户更新，另一个也会被重新计算。  
让我们回顾一下，当编辑一个输入框时，发生了什么：
* React调用``<input>``中被指定的``onChange``方法。在本例中，该方法为``TemperatureInput``中的``handleChange``。  
* ``TemperatureInput``组件中的``handleChange``方法调用``this.props.onTempatureChange``。该组件的属性(包括``onTempatureChange``)由其父组件``Calculator``提供  
* 在其被渲染前，``Calculator``为单位是摄氏度的``TemperatureInput``指定的``onTempatureChange``的方法是``handleCelsiusChange``，为华氏度指定的是``handleFahrenheitChange``。因此我们在不同的输入框输入内容时会调用``Calculator``中相应的``handlexxChange``方法。  
* 在这些方法中，``Calculator``组件根据输入框中由用户更新的温度值及该输入框的单位，调用``setState``更新自己的状态，从而令React对其重新绘制。  
* React调用``Calculator``组件的``render``方法绘制组件。两个输入框中的值都会根据用户输入的温度和被用户输入的那个输入框的单位重新计算。在这一步，进行了温度的转换。
* React调用各``TemperatureInput``的``render``方法及其由``Calculator``赋给的新属性值，绘制出两个输入框。  
* React DOM更新DOM。我们刚刚编辑的输入框接受其当前值，另一个输入框更新为转换后的温度。  

#### 小结
React应用中任何数据的改变都应遵从 __单一数据源__ 原则。  
通常情况下，状态应被优先加入到渲染时依赖该值的组件中。但当有其他组件也依赖于该状态时，我们可以将其提升至组件们的最近公共祖先组件中，而不是尝试同步不同组件间的状态。我们应该遵守数据流的从上至下的原则。  
相对双向绑定来说，状态提升需要编写更多无意义的样板代码，但这么做的好处在于能够更方便的查找与隔离bugs-因为任何状态都只能由某一个组件对其进行修改，这能大大的减少查找bug产生的范围。另外，我们可以轻松的自定义任何逻辑对用户的输入进行预处理(如转换或禁止输入等)。  
当某一个数据可以通过属性或状态得到，那么该数据也许不适合作为状态存在。例如我们先前的例子，我们并没有同时保存``celsiusValue``和``fahrenheitValue``两个值，而是保存了用户最近一次输入的数值和用户输入的输入框单位。另一个输入框的内容可以通过``render``方法计算出来。 __这么做让我们能够在无论是否使用四舍五入等估算方法，都不会丢失用户输入的精度。__  
当我们看到UI出现不符合预期的错误时，我们可以通过[React开发者工具](https://github.com/facebook/react-devtools)进行排查，揪出负责更新该状态的组件。这对找出bug的源头非常有帮助：  

![React Developer Tools](http://7xv88e.com1.z0.glb.clouddn.com/react-devtools-state.gif)

## 组合与继承

React拥有强大的组合模型，我们建议使用组合而非继承来重用组件间的代码。  
在本部分，我们将展示一些刚接触React的开发者在面对某些情况时可能使用继承的例子，并用组合的方式解决他们。  

#### 包含
一些组件在开始时不能确定是否有子组件，尤其是对``Sidebar``和``Dialog``等这类用于表示``box``的组件而言特别常见。  
对于这类组件，我们建议使用特定的``children``属性将子元素直接传递到他们的输出中：
```javascript
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```
这使得其他组件能通过嵌套JSX向该组件传递任意的子元素：
```javascript
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/ozqNOV?editors=0010)。  
在JSX标签``<FancyBorder>``中包含的所有东西都会作为``children``属性传递进去。由于``FancyBorder``在``<div>``中渲染``{props.children}``，传递给``FancyBorder``的元素直接被显示出来。  
还有一类比较少见的情况，有时我们可能在一个组件中需要多个占位符。这时我们不必要强制使用``children``，而可以使用自己自定义的属性：
```javascript
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/gwZOJp?editors=0010)。  
React元素``<Contacts />``和``<Chat />``就是对象，我们可以像传递其他数据一样，将他们作为属性进行传递。  

#### 特例
有时我们会遇到某个组件被另一个组件包含的情况，如``WelcomeDialog``是一种特殊的``Dialog``。  
在React中，这也可以通过组合来完成-用一个更"特殊"的组件包含那个更"通用"的组件，并使用属性对其进行配置：
```javascript
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/kkEaOZ?editors=0010)。 
组合对于用类定义的组件也同样适用：
```javascript
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = {login: ''};
  }

  render() {
    return (
      <Dialog title="Mars Exploration Program"
              message="How should we refer to you?">
        <input value={this.state.login}
               onChange={this.handleChange} />
        <button onClick={this.handleSignUp}>
          Sign Me Up!
        </button>
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({login: e.target.value});
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }
}
```
[在CodePen中尝试](https://codepen.io/gaearon/pen/gwZbYa?editors=0010)。 

#### 那么，继承呢？
在现有的React组件实践中，我们暂时还没有发现任何一种使用继承能够优于组合方式的情况。  
使用属性和组合已经能为我们提供明确、安全的方法定制我们组件的外观和行为。 __记住，组件可以接受任何属性，包括原始值，React元素或函数。__  
想要在组件中复用非UI功能的代码，我们建议将其提取至单独的JS模块中。组件可以导入该模块，使用其中的函数、对象或类，而不需要去扩展它。
