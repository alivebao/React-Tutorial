# React教程

## 目录
  1. [介绍](#介绍)
  2. [Hello World](#hello-world)
  3. [学习JSX](#学习jsx)
  4. [渲染元素](#渲染元素)
  5. [组件和属性](#组件和属性)
  6. [状态和生命周期](#状态和生命周期)

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
但是，__当通过JavaScript表达式未属性赋值时，不要使用双引号。__
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