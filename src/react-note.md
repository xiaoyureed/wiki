---
title: React
tags:
    - react
    - module
date: 2018-06-28 20:26:11
categories: frontend
---

<div align="center">

https://www.zhihu.com/question/59073695/answer/1071631250 类库推荐

https://reactjs.org/
若以 MVC 框架来看，React 属于view

https://github.com/dypsilon/frontend-dev-bookmarks 前端集合

https://www.zhihu.com/question/39943474/answer/156412178,

https://www.zhihu.com/question/29848048/answer/45793588,

https://github.com/petehunt/react-howto/blob/master/README-zh.md,

https://github.com/bailicangdu/react-pxq

[效率优化](https://zhuanlan.zhihu.com/p/37148975)

[react tutorial](https://reacttraining.com/)

https://www.zhihu.com/question/59073695/answer/1071631250 推荐的 react 库

https://www.npmjs.com/
https://ant.design/components/overview-cn/

https://www.zhihu.com/question/336664883/answer/790855896 为什么要拆分包? ReactDom只做和浏览器或DOM相关的操作，例如ReactDOM.render()和ReactDOM.findDOMNode()。如果是服务器端渲染，可以ReactDOM.renderToString()

http://huziketang.mangojuice.top/books/react/ 实战

https://github.com/helloqingfeng/Awsome-Front-End-learning-resource 大前端
https://github.com/qiu-deqing/FE-interview , https://github.com/markyun/My-blog/tree/master/Front-end-Developer-Questions 面试题

</div>

<!--more-->

<!-- TOC -->

- [面试题](#面试题)
- [why react](#why-react)
- [jsx](#jsx)
    - [基本语法](#基本语法)
    - [使用 jsx 需要的环境配置](#使用-jsx-需要的环境配置)
- [开发环境搭建](#开发环境搭建)
    - [老项目接入](#老项目接入)
    - [create-react-app 使用](#create-react-app-使用)
    - [vscode 环境](#vscode-环境)
    - [webpack 环境搭建](#webpack-环境搭建)
- [组件化](#组件化)
    - [社区开源组件](#社区开源组件)
    - [Web Components](#web-components)
    - [组件设计原则](#组件设计原则)
    - [带状态组件 纯函数组件](#带状态组件-纯函数组件)
    - [函数绑定方式](#函数绑定方式)
    - [hooks](#hooks)
        - [最佳实践](#最佳实践)
        - [为什么需要 hooks](#为什么需要-hooks)
        - [useRef](#useref)
        - [useMemo and memo and useCallback](#usememo-and-memo-and-usecallback)
        - [useState](#usestate)
            - [state 特性](#state-特性)
            - [传统写法](#传统写法)
            - [使用 functional 组件的写法](#使用-functional-组件的写法)
            - [延迟初始化](#延迟初始化)
            - [新旧 state 有关联](#新旧-state-有关联)
        - [useContext](#usecontext)
        - [useReducer](#usereducer)
        - [useEffect](#useeffect)
            - [effect 理解](#effect-理解)
            - [effect 基本使用](#effect-基本使用)
            - [缓存上次值](#缓存上次值)
            - [设置销毁回调函数例子](#设置销毁回调函数例子)
            - [发送异步请求](#发送异步请求)
            - [使用 fetch api](#使用-fetch-api)
        - [自定义 hook](#自定义-hook)
            - [带有自增效果的 state hook](#带有自增效果的-state-hook)
            - [从 usehooks 改造 responseComponent](#从-usehooks-改造-responsecomponent)
            - [定时器 and 自动销毁](#定时器-and-自动销毁)
        - [综合例子](#综合例子)
            - [点赞按钮](#点赞按钮)
    - [高阶组件 and 函数作为子组件](#高阶组件-and-函数作为子组件)
    - [Fragment](#fragment)
    - [StrictMode](#strictmode)
    - [示例](#示例)
        - [使用 props](#使用-props)
        - [遍历子元素](#遍历子元素)
        - [找到真实的 dom(this.ref.xxx)](#找到真实的-domthisrefxxx)
        - [使用 state(this.state)](#使用-statethisstate)
        - [一个表单示例](#一个表单示例)
        - [登录界面](#登录界面)
        - [todo list app](#todo-list-app)
- [组件的生命周期](#组件的生命周期)
    - [生命周期介绍](#生命周期介绍)
    - [组件被挂载](#组件被挂载)
    - [组件被卸载](#组件被卸载)
    - [组件被更新](#组件被更新)
    - [生命周期函数 demo](#生命周期函数-demo)
- [virtual dom 和 diff 算法](#virtual-dom-和-diff-算法)
    - [virtual dom](#virtual-dom)
    - [key 属性有什么作用](#key-属性有什么作用)
    - [设置组件时怎么提高性能](#设置组件时怎么提高性能)
- [context-api 使用](#context-api-使用)
- [state 状态](#state-状态)
    - [state 介绍](#state-介绍)
    - [state 和组件的普通属性区别](#state-和组件的普通属性区别)
    - [如何更新 state](#如何更新-state)
    - [setState 是不是异步的](#setstate-是不是异步的)
    - [setState 更新是一个合并过程](#setstate-更新是一个合并过程)
    - [replaceState 更新是一个替换过程](#replacestate-更新是一个替换过程)
- [props 属性](#props-属性)
    - [在 jsx 中遍历 props](#在-jsx-中遍历-props)
    - [PropTypes 类型检查](#proptypes-类型检查)
    - [引入类型检查的其他方法](#引入类型检查的其他方法)
        - [引入 TypeScript](#引入-typescript)
        - [flow 类型检查](#flow-类型检查)
    - [设定 props 的默认值](#设定-props-的默认值)
    - [有哪些属性](#有哪些属性)
- [refs](#refs)
- [合成事件](#合成事件)
- [状态管理](#状态管理)
    - [flux 架构的思想](#flux-架构的思想)
    - [Flux 官方实现](#flux-官方实现)
    - [Redux](#redux)
        - [为什么要使用 redux](#为什么要使用-redux)
        - [基本概念](#基本概念)
        - [基本 api](#基本-api)
            - [store](#store)
            - [reducer](#reducer)
            - [applyMiddleware](#applymiddleware)
            - [bindActionCreators](#bindactioncreators)
        - [纯 redux 的 api 示例(同步版本的 api)](#纯-redux-的-api-示例同步版本的-api)
        - [react-redux](#react-redux)
        - [异步 Action](#异步-action)
        - [thunk, saga](#thunk-saga)
        - [调试工具](#调试工具)
        - [demo(一个 todo-app)](#demo一个-todo-app)
    - [Redux 和 Flux 的一些差异](#redux-和-flux-的一些差异)
    - [ImmutableJS](#immutablejs)
- [路由 react-router](#路由-react-router)
    - [路由标签的使用](#路由标签的使用)
        - [Router-路由容器](#router-路由容器)
        - [Route-路由组件映射](#route-路由组件映射)
            - [属性](#属性)
            - [通过 url 传递参数给 Route](#通过-url-传递参数给-route)
            - [嵌套路由](#嵌套路由)
            - [Route 三种渲染方式](#route-三种渲染方式)
        - [Switch-解决多匹配问题](#switch-解决多匹配问题)
        - [Redirect-重定向当前页面](#redirect-重定向当前页面)
        - [Link-超链接](#link-超链接)
        - [NavLink: 带样式的 Link](#navlink-带样式的-link)
        - [Prompt-提示](#prompt-提示)
    - [history](#history)
    - [location](#location)
    - [match](#match)
- [调试和测试](#调试和测试)
- [GraphQL 和 Relay](#graphql-和-relay)
- [CSS-Module 模块化](#css-module-模块化)
- [css-in-js](#css-in-js)
- [最佳实践 best practice](#最佳实践-best-practice)
- [服务端渲染](#服务端渲染)
    - [什么是 ssr](#什么是-ssr)
    - [ssr 的好处](#ssr-的好处)
    - [解决方案](#解决方案)
        - [配合 koa2 实现 ssr](#配合-koa2-实现-ssr)
        - [egg 实现 ssr](#egg-实现-ssr)
- [性能优化](#性能优化)
    - [常见的性能问题场景](#常见的性能问题场景)
    - [使用工具定位性能问题](#使用工具定位性能问题)
    - [网络性能优化-自动按需加载](#网络性能优化-自动按需加载)
    - [使用 Reselect 避免重复计算](#使用-reselect-避免重复计算)
- [react 动画](#react-动画)
- [react native](#react-native)
- [开发小程序](#开发小程序)
- [小项目](#小项目)
    - [实现 route](#实现-route)
    - [表单设计器](#表单设计器)
    - [cms 系统](#cms-系统)
- [部署到 github 上](#部署到-github-上)
- [和 springboot 打包单体应用](#和-springboot-打包单体应用)
- [和 typescript 一起使用](#和-typescript-一起使用)
    - [ts 基本使用](#ts-基本使用)
        - [枚举](#枚举)
        - [使用 interface 定义对象类型](#使用-interface-定义对象类型)
        - [interface 定义函数类型](#interface-定义函数类型)
        - [interface 定义可索引类型](#interface-定义可索引类型)
        - [interface 定义 class 类](#interface-定义-class-类)
        - [interface 继承](#interface-继承)
        - [class 使用](#class-使用)
        - [泛型使用](#泛型使用)
        - [交叉类型](#交叉类型)
        - [联合类型](#联合类型)
        - [类型保护](#类型保护)
        - [类型别名](#类型别名)
        - [空类型](#空类型)
    - [集成 ts 环境](#集成-ts-环境)
    - [为样式文件编写 types](#为样式文件编写-types)
    - [配置 css 跳转](#配置-css-跳转)
- [使用 debugger for  Chrome](#使用-debugger-for--chrome)

<!-- /TOC -->


# 面试题

```

- React解决了什么问题 - 减少了页面渲染范围, 缓解浏览器绘制压力
- 组件的Render函数在何时被调用？ -  state 发生改变的时候
  - 调用时DOM就一定会被更新吗, -  不一定, 会对比更新后的 virtual dom 和 real dom , 找出需要更新的最小步骤，此时Real DOM才可能发生修改。
- 组件的生命周期有哪些？

    组件的初始化阶段的声明周期函数以及重点用法如下：

    constructor(): 用于绑定事件以及初始化state（可以通过"fork"props的方式给state赋值）
    componentWillMount(): 只会在服务端渲染时被调用，你可以在这里同步操作state
    render(): 这个函数是用来渲染DOM没有错。但它只能用来渲染DOM，请保证它的纯粹性。如果有操作DOM或者和浏览器打交道的一系列操作，请在下一步骤componentDidMount中进行
    componentDidMount(): 如果你有第三方操作DOM的类库需要初始化（类似于jQuery，Bootstrap的一些组件）操作DOM、或者请求异步数据，都应该放在这个步骤中做

    组件更新阶段：

    componentWillReceiveProps(nextProps): 在这里你可以拿到即将改变的状态，可以在这一步中通过setState方法设置state
    shouldComponentUpdate(nextProps, nextState): 这一步骤非常重要，它的返回值决定了接下来的生命周期函数是否会被调用，默认返回true，即都会被调用；你也可以重写这个函数使它返回false。
    componentWillUpdate(): 我也不知道这个声明周期函数的意义在哪里，在这个函数内你不能调用setState改变组件状态
    render()
    componentDidUpdate(): 和componentDidMount类似，在这里执行DOM操作以及发起网络请求
    
    组件析构阶段：

    componentWillUnmount(): 主要用于执行一些清理工作，比如取消网络请求，清楚多余的DOM元素等



  - 当某些第三方类库想对DOM初始化，或者进行远程数据加载时，应该在哪个周期中完成
  - 在哪些声明周期中可以修改组件的state？
- 不同父节点的组件需要对彼此的状态进行改变时应该实现？
  - 如何设计出一个好的Flux架构
  - 如何设计出一个好的React组件
- 如何进行优化？
  - 组件中的key属性有什么用？
- Component 与 Element 与 Instance 的区别
- Webpack如何打包输出多个文件？
  - webpack打包时如何工作的？
  - 如何解决循环引用的问题
  - 在什么情况下需要打包输出多个文件？
  - loader和plugin的差别
  - 你觉得使用过什么高级技巧吗？
- (开放问题）React的生态你使用过哪些类库

```


# why react

ReactJS: ui 展示框架, 只负责 view 渲染的部分, 包括 npm 库 `react`(核心), `react-dom`(专门处理 dom)

reactjs 来自 facebook, 起初被创造是为了解决一个问题: 页面上的一个图标需要实时显示一个数字, 表示有多少消息未读, 但是始终做不到实时变化. 产生的原因是,

-   传统的 dom api 需要操作太多细节 (比如典型的 jQuery, api 巨多无比). react 实现了整体刷新, ui 渲染和数据绑定, ui 随数据实时变化

-   还有一个原因, 传统的 app 的状态分散不同的地方

so why react?

-   传统开发模式如 jQuery 直接操作 dom 树, 性能消耗大, react 使用 virtual dom 取而代之
-   传统开发直接操纵 dom, 需要关注太多 api 细节 (看看 jQuery 有多少 api 就知道了), react 实现了 ui 渲染 和 数据绑定, 不用手动操作渲染
-   传统开发 代码难以复用, react 提供组件化的开发方式

前端工程 and 模块化 -> js-proj-history.md

# jsx 

## 基本语法

JSX: 语法糖, 类似 xml, 但是进行了拓展, 加入 js 语法, 将 HTML 直接嵌入了 JS 代码里面, 最终需要“编译”输出 JS 代码才能使用;

使用 React 并非必须使用 JSX, JSX 只是一种直观的创建 React nodes 的方法，它是对 `React.createElement()`方法的封装

通过 Babel，JSX 语句也可以直接在浏览器中运行

看看 demo------- [🚪](https://github.com/xiaoyureed/react-note/blob/master/demo/demo.js)

```js
// 语义化且易懂的标签
if(this.state.liked) {
  return (<BlueLike />);
} else {
  return (<GrayLike />);
}

// jsx 如何转化为 js:
React.createElement(
  string/ReactClass, // 表示 HTML 元素或是 React Component
  [object props], // 属性值 object
  [children] // 接下来参数皆为元素子元素
)


// 使用标签的写法, 代码更加简洁
<a href="https://facebook.github.io/react/">Hello!</a>/* 可以直接在jsx中使用 */
// React.createElement(组件/HTML标签, 组件属性, 子元素)
React.createElement('a', {href: 'https://facebook.github.io/react/'}, 'Hello!')



// 结合js写法

// 解析时候, "{" 开始, 进入js, "<" 开始, 进入html
var text = 'Hello React';
// jsx 本身也是表达式
const element = <h1>{text}</h1>;
// 会替换为:
var text = 'Hello React';
React.createElement("h1", null, text);

// 在 html 标签属性 中使用 jsx
<h1 foo={1 + 2} />


// 使用数组
/* jsx中使用js, jsx中使用数组, <!-- 最好有使用index, 不然会报警告, 实际上是因为有损性能 --> */
var names = ['aaa', 'bbb', 'ccc'];
ReactDOM.render(
    <div>
        {
            names.map(function (name, index) {
                return <div key={index}>hello, {name}</div>
            })
        }
    </div>
    ,
    document.getElementById("array")
);

// -------------------------------------------

// /* 这里会将数组数据连起来, 这里没有index, 会警告, 不过不影响运行 */
var namesHtml = [<h1>h1...</h1>, <h2>h2...</h2>];
ReactDOM.render(
    <div>
        {namesHtml}
    </div>,
    document.getElementById("htmlArray")
);


// ----------------------

jsx 中的关键字变化

class 和 for 由于为 JavaScript 保留关键字用法，因此在 JSX 中使用 className 和 htmlFor 替代

Boolean 属性: 在 JSX 中预设只有属性名称但没有(无需)设值为 true

// ---------------------

自定义属性 data-xxx
<HelloMessage data-attr="xd" />

//--------------

使用 "..."遍历 props 属性

var props = {
    style: "width:20px",// 报错, 存疑; 可能style需要单独一个对象, 或者 <HelloMessage style={{ color: '#FFFFFF', fontSize: '30px'}} />
    className: "main",
    value: "yo",  // 会被替换
}

<HelloMessage  {...props} value="yo" />

// 等于以下
React.createElement("h1", React._spread({}, props, {value: "yo"}), "Hello React!");

// ==========================


直接显示原始 HTML
<div>{{ _html: "<h1>Hello World!!</h1>" }}</div>


//================

Inline Style
// 在 React Component 中 CSS 使用 Inline Style 写法，全都封装在 JavaScript 当中
const divStyle = {
    color: "red",
    backgroundImage: "url(" + imgUrl + ")"
};

ReactDOM.render(
    <div style={divStyle}>Hello World!</div>,
    document.getElementById("app")
);


```

## 使用 jsx 需要的环境配置

jsx 被浏览器识别需要先编译, 这里有 2 个方法:

一种是使用 browserify 或 webpack 等 CommonJS bundler 并整合 babel 预处理, 于浏览器端做解析, 相较于 server 端解析耗费时间, 但是适合整合老项目

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.0.1/react.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.0.1/react-dom.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.23/browser.min.js"></script>
<!-- babel.js与browser.js的关系: babel的浏览器版本为browser.js -->
```

第二种是结合 webpack, babel 预先编译再部署, 使用 babel.js, `babel src --out-dir build`



==================== 使用第一种方法使用 jsx, 如何引入 jsx

直接内嵌

```js
/* text/babel表示为jsx语法， 和js不兼容 */
<script type="text/babel">
    ReactDOM.render(
    <h1>Hello, world!</h1>, document.getElementById('example') );
</script>
```

从外部引入

```js
<script type="text/jsx" src="main.jsx" />
```




# 开发环境搭建

## 老项目接入

CDN-based(直接引入 script 的方式, 适合老项目, 见下节 demo)、 webpack([ref](https://github.com/carlleton/reactjs101/blob/zh-CN/Ch02/webpack-dev-enviroment.md)), TypeScript 的开发方式, browserify 搭配 Gulp 的方法([ref](https://github.com/carlleton/reactjs101/blob/zh-CN/Ch02/browserify-gulp-dev-enviroment.md))

## create-react-app 使用

也可以直接使用 create-react-app 官方脚手架

```sh
# 全局安装
npm i -g create-react-app
# 创建一个app
create-react-app demo-app

# 也可以一步到位
npx create-react-app demo-app

```

可以配合 vscode 的 chrome debugger 插件:

需要修改 url 为自己的，react 工程默认是http://localhost:3000

![alt](1.gif)

推荐先 `npm start` 启动 app, 然后 F5 启动调试服务器, 此时会新开一个 chrome, 访问配置的 url

## vscode 环境

eslint, prettier 插件, 和对应的 module

生成 .eslintrc.js 时候（eslint —init）的第一步选择，即"How would you like to use ESlint"，不要选择"enforce code style"，因为我们要交给 Prettier 去负责格式化代码，否则会有冲突

但如果我们选择了"enforce code style"，那么会多出一步"Which style guide do you want to follow? (Use arrow keys)"让你选，此时可以选择三种代码风格：即 Airbnb、Standard、Google。

[vscode 关闭 eslint](https://blog.csdn.net/lishuan182/article/details/89575678)

[eslint + airbnb 规范](https://segmentfault.com/a/1190000020379876)

[eslint + prettier 配合使用, 冲突解决, 使用 git hooks 在每次提交 commit 时 (pre-commit)格式化](https://thomlom.dev/setup-eslint-prettier-react/)

## webpack 环境搭建

```sh
yarn add webpack webpack-cli webpack-dev-server
touch webpack.config.js


```

```json
"scripts": {
        "dev": "webpack-dev-server --mode development",
        "build": "webpack --mode production",
 },
# yarn dev 去启动本地server
# yarn build 去生成生产代码



```

https://juejin.im/post/5d7a7e7e518825345a05c518

https://juejin.im/post/5cfb8c0051882541b24c3ed3 //todo



# 组件化

## 社区开源组件

- react-hook-form 表单 - https://github.com/react-hook-form/react-hook-form
- react-beautiful-dnd 拖拽
- https://github.com/vercel/swr 远程数据获取 remote data fetching

## Web Components

http://www.ruanyifeng.com/blog/2019/08/web_components.html

TODO


## 组件设计原则

props (外部传入) + state (内部维护) = View

Ract 组件一般不提供方法, 不同的 props 和 state 会产生 不同的 view, 相当于一个 pure function.

创一个组件考虑三点:

-   静态 ui
-   状态组成, 内部维护
-   由哪些交互方式

遵循的原则:

-   自定义组件名大写字母开头 (react 默认 大写为自定义, 小写为原生 dom 节点如 div), 另外 jsx 支持名称的属性语法, 如 `<menu.Item />`

-   组件应该足够小, 只做一件事 (这是为了性能考虑, 拆分成小组件后, 各个组件都有自己的 state, 变化时只会自己刷新, 不会使得整个拼成的大组件刷新)

-   state 能不要就不要, 能通过计算得到的状态就不要特地存储到 state, 尽量所有数据都通过 props 获取, 也就是 functional component 优于 state component (性能好, 易于重用)

## 带状态组件 纯函数组件 

两种写法 ✍:

ES6 语法(可以进行比较复杂的操作和组件生命周期的控制，相对于 Functional components 耗费资源)

```js
//  注意组件开头第一个字母都要大写
class MyComponent extends React.Component {

    // ES6 Class 中 render() 是唯一必要的方法（但要注意的是请保持 render() 的纯粹，不要在里面进行 state 修改或是使用非同步方法和浏览器互动，若需非同步互动请于 componentDidMount() 操作）
    render() {
        return <div>Hello, World!</div>;
    }
}

// 将 <MyComponent /> 组件插入 id 为 app 的 DOM 元素中
ReactDOM.render(<MyComponent />, document.getElementById("app"));
```

使用 Functional Component 写法（单纯地 render UI 的 stateless components，没有内部状态、没有 ref，没有生命周期函数。若非需要控制生命周期的话建议多使用 stateless components 获得比较好的性能）

```js
// 使用 arrow function 来设计 Functional Component 让 UI 设计更单纯（f(D) => UI），减少副作用（side effect）
const MyComponent = props => <div>Hello, World!</div>;

// 将 <MyComponent /> 组件插入 id 为 app 的 DOM 元素中
ReactDOM.render(<MyComponent />, document.getElementById("app"));
```

## 函数绑定方式

react 中由于类的方法默认不会绑定this，因此在调用的时候如果忘记绑定，this的值将会是undefined

```jsx
// 纯函数组件

const Edit = props => {
    const onFinish = (values) => {
        console.log('Received values of form: ', values);
    };

    return (
        <Form
            onFinish={onFinish}
        />
    );
}
```

```jsx
// class 组件

// 推荐
import React, { Component } from 'react';
class App extends Component {
  handleSubmit (e, args) {
    console.log(e, args)
  }

  render () {
      // 或者在构造函数中 bind
      // 如 this.handleClick = this.handleClick.bind(this);
    return (
      <div onClick={this.handleSubmit.bind(this, 'test')}>test</div>
    )
  }
}

// or

class App extends Component {
  handleSubmit = (e) => {
     console.log(e);
  }

  render () {
    return (
      <div onClick={this.handleSubmit}>test</div>
    )
  }
}

//or

class App extends Component {
  handleSubmit (e) {
     console.log(e);
  }
  render () {
    return (
      <div onClick={ () => this.handleSubmit }>test</div>
    )
  }
}
```

## hooks

https://reactjs.org/docs/hooks-reference.html

### 最佳实践

https://zhuanlan.zhihu.com/p/98554943
https://www.zhihu.com/search?type=content&q=react%20hooks%20%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5

### 为什么需要 hooks

https://www.javascriptc.com/2530.html

纯函数，不能包含状态，也不支持生命周期方法，因此无法取代类的写法, React Hooks 的设计目的，就是加强版函数组件，完全不使用"类"，就能写出一个全功能的组件

React Hooks 的意思是，组件尽量写成纯函数，外部功能和副作用放在钩子中。 React Hooks 就是那些钩子.需要什么功能，就使用什么钩子。React 默认提供了一些常用钩子. 钩子一律使用use前缀命名，便于识别

hooks 解决了几个问题:

- 容易对 state 操作逻辑进行复用
- 以前分布在周期函数中的操作集中到了一个地方 (useEffect 中)
- 屏蔽了 this

```js
四个最常用的狗子:

useState()
为函数组件引入状态（state）

useContext() 在组件之间共享状态, 不必为了参数传递进行复杂嵌套, 减少组件层级

useReducer() 为函数组件引入 action

useEffect() 引入具有副作用的操作，最常见的就是向服务器请求数据

useLayoutEffect() 同步执行副作用

useCallback() 记忆函数
useMemo() 记忆组件
可以完全替代 useCallback 的功能 
useCallback(fn, inputs) is equivalent to useMemo(() => fn, inputs).


useImperativeHandle() 组件可以获得子组件的 input 的 DOM 节点

```

由于 Hooks 可以提供共享状态和 Reducer 函数，所以它在这些方面可以取代 Redux。但是，它没法提供中间件（middleware）和时间旅行（time travel），如果你需要这两个功能，还是要用 Redux

### useRef

use to create a reference to refer to some object else;

useRef 仅能用在 FunctionComponent，(显然的，因为 Hooks 不能用在 ClassComponent), somehow equal to “this”; 避免在 Render 函数中直接修改 Ref, 最好放在某个函数中修改

createRef 仅能用在 ClassComponent。(cuz: createRef 并没有 Hooks 的效果，其值会随着 FunctionComponent 重复执行而不断被初始化：)  createRef 每次渲染都会返回一个新的引用，而 useRef 每次都会返回相同的引用

```js
//作为容器
export default () => {
  const refInput = useRef();
  return (
    <div>
      <input ref={refInput} />
      <button
        onClick={() => {
          refInput.current.focus();
        }}
      >
        focus
      </button>
    </div>
  );
};
```

cache prev value: (can be inproved by useEffect, see blow)

```js
export default () => {
  const [count, setCount] = useState(0);
  // 在两次不同的渲染间传递值
  // 相当于 类属性变量
  const prev = useRef(null);
  return (
    <div>
      <p>current: {count}</p>
      <p>prev: {prev.current}</p>
      <button
        onClick={() => {
          prev.current = count;
          setCount(count + 1);
        }}
      >
        +
      </button>
      <button
        onClick={() => {
          prev.current = count;
          setCount(count - 1);
        }}
      >
        -
      </button>
    </div>
  );
};
```


### useMemo and memo and useCallback

都是用来提升性能. 可以防止维护的对象重复创建



- memo() 决定一个组件的渲染是否重复执行

  - 常用来包装一个组件; euqals to 函数式组件中的  PureComponent

  - PureComponent: 通过浅层比较 state, props, 决定是否重新渲染; 也就是说, 优化只对于 浅层属性变化有效; 也可以自定义 shouldComponentUpdate()

- useMemo  一段函数逻辑是否重复执行; 接受一个函数, 和一个数组(依赖)，在后续 render 时，将依赖数组中的值取出来和上一次记录的值进行比较，如果不相等才会重新执行回调函数

  - useMemo 类似 useEffect, 区别是 effect 是 render 后执行， useMemo 是 render 前执行, 之后是否重复执行, 看依赖是否变化

  - usememo 本身也有开销, 所以使用 useMemo 考虑: 传递给 useMemo 的函数开销大不大? 如果大才用, 

  - 推荐使用 React.useMemo 而不是 React.memo，因为在组件通信时存在 React.useContext 的用法，这种用法会使所有用到的组件重渲染，只有 React.useMemo 能处理这种场景的按需渲染。

- useCallback 用来包装一个函数, 每次重渲染都新创建这个函数的实例

  - 若 useMemo 内的函数返回一个函数， 那么可以使用 useCallback直接传入这个函数 即 useMemo(() => fn) 等价 useCallback(fn)

  - 所有 Function Component 内函数必须用 React.useCallback 包裹，以保证准确性与性能

```js
const Demo = memo((props) => {
  console.log("demo render");
  return (
    <div>
      {props.name}, {props.person.age}
    </div>
  );
});

export default () => {
  const [count, setCount] = useState(0);
  const [person, setPerson] = useState({
    age: 11
  });
  return (
    <div>
      <span
        onClick={() => {
          setCount(count + 1);// 如果仅仅是 count 改变, Demo 不会rerender
          setPerson({// 如果person也改变, demo 会rerender
            ...person,
            age: person.age + 1
          });
        }}
      >
        {count}
      </span>
      <Demo name="demo" person={person} />
    </div>
  );
};

```


```js
/**
 * 
 */
export default () => {
  const [count, setCount] = useState(0);
  const memorizedText = useMemo(() => {
    console.log(`run memo func`);
    return `memo: ${Date.now()}`;
  }, [Math.floor(count / 10)]); // 每点击 10 次变化一次

  return (
    <div>
      {memorizedText}
      <p>count: {count}</p>
      <button
        onClick={() => {
          setCount(count + 1);
        }}
      >
        +
      </button>
      <button
        onClick={() => {
          setCount(count - 1);
        }}
      >
        -
      </button>
    </div>
  );
};

```


### useState

#### state 特性

每一次渲染都有它自己的 Props 和 State
每一次渲染都有它自己的事件处理函数
当点击更新状态的时候，函数组件都会重新被调用，那么每次渲染都是独立的，取到的值不会受后面操作的影响

Hook 内部使用 Object.is 来比较新旧 state 是否相等
与 class 组件中的 setState 方法不同，如果你修改状态的时候，传的状态值没有变化，则不重新渲染;
useState 不会自动合并更新对象, 二是替换

#### 传统写法

(由于带有 状态, 所以必须使用 类 的写法)


```js
class Button extends React.Component {
  constructor() {
    super();
    this.state = {
      content: "click me"
    };
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState({
      content: "hello btn ~~"
    });
  }

  render() {
    return <button onClick={this.handleClick}>{this.state.content}</button>;
  }
}
```

#### 使用 functional 组件的写法

```js
import React, { useState } from "react";

const ButtonHooks = () => {
  // 语法:
  // 参数为 某个 state 属性初始值
  // 返回值为数组, 
  //[0] 为 属性变量, 
  //[1] 为对应的 setter 方法, 约定是set前缀加上状态的变量名; 若 new state 和 old state有关联, 推荐 setXXX((old) => {return new})
  // 每用一次 useState , 就可以为一个 state 属性 创建 getter, setter, 顺序很重要, 不可改变(即无法放在条件判断内)
  const [content, setContent] = useState("click me");

  const handleClick = () => {
    return setContent("hello hah~~");
  };
  return <button onClick={handleClick}>{content}</button>;
};

export default ButtonHooks;

```

#### 延迟初始化

initialState 参数只会在组件的初始化渲染中起作用，后续渲染时会被忽略, 传入有个函数, 只会在初始渲染时执行一次, 后续不再执行

```js
export default (props) => {
  const [count, setCount] = useState(0);

  const [age, setAge] = useState(() => {
    console.log("延迟初始化, 初始化时, 即使用的时候才执行计算， 只会执行一次， 提高效率");
    return props.defaultAge || 0;
  });

  return (
    <div className="App">
      count: {count}, age: {age}
      <button onClick={() => setCount((count) => count + 1)}>Add</button>
    </div>
  );
};


```

#### 新旧 state 有关联

如果新的 state 需要通过使用先前的 state 计算得出，那么可以将回调函数当做参数传递给 setState。该回调函数将接收先前的 state，并返回一个更新后的值


### useContext

类似 redux 作用, 但是 redux 更强大, 

useReducer和useContext的结合可以取代Redux么？

- useContext的问题是如果数据发生变化，整个子树都会重新渲染，性能可能会有问题
- 使用useContext你只能在组件层写更改状态的逻辑，但是有的逻辑是全局的，应该写到一个单独的js文件而不是组件文件里，而要导出的话就必须自己封装，还不如用Redux
- Redux有thunk和saga之类的中间件支持async action，而useReducer没有，还得用其他库。

```js
import React, { useContext } from "react";
import ReactDOM from "react-dom";
import "./styles.css";

const AppContext = React.createContext({});

// 语法:
    // 参数为 react.context 变量
    // 返回 context 中存储的内容, 有两个地方可以指定存储的内容
    // - AppContext = React.createContext({}) // 初始化
    // - <AppContext.Provider value={{}} ... // 指定具体值
function App() {
  return (
    //   AppContext.Provider提供了一个 Context 对象，这个对象可以被子组件共享
    <AppContext.Provider value={{
      username: 'superawesome'
    }}>
      <div className="App">
        <Navbar />
        <Messages />
      </div>
    </AppContext.Provider>
  );
}

const Navbar = () => {
  const { username } = useContext(AppContext)

  return (
    <div className="navbar">
      <p>AwesomeSite</p>
      <p>{username}</p>
    </div>
  )
}

const Messages = () => {
  const { username } = useContext(AppContext)

  return (
    <div className="messages">
      <h1>Messages</h1>
      <p>1 message for {username}</p>
      <p className="message">useContext is awesome!</p>
    </div>
  )
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);


```

改变颜色主题 demo:

```js
const themes = {
  light: {
    fore: "#000000",
    back: "#eeeeee"
  },
  dark: {
    fore: "#ffffff",
    back: "#222222"
  }
};

const ThemeContext = createContext({
  theme: themes.light,
  toggle: () => {}
});

export default () => {
  const [theme, setTheme] = useState(themes.light);
  return (
    // 多个 context 嵌套即可， 顺序不重要
    <ThemeContext.Provider
      value={{
        theme,
        toggle: () => {
          setTheme((theme) => {
            setTheme(theme === theme.light ? themes.dark : themes.light);
          });
        }
      }}
    >
      <ToolBar />
    </ThemeContext.Provider>
  );
};

const ToolBar = () => {
  return <ThemedButton />;
};

const ThemedButton = () => {
  const context = useContext(ThemeContext);
  return (
    <button
      style={{
        color: context.theme.fore,
        backgroundColor: context.theme.back
      }}
      onClick={() => {
        context.toggle();
      }}
    >
      click
    </button>
  );
};
```

### useReducer

用于管理复杂的 state 变化

局部状态不推荐使用 useReducer ，会导致函数内部状态过于复杂，难以阅读。 useReducer 建议在多组件间通信时，结合 useContext 一起使用

```js
import React, { useReducer } from "react";

// reducer 负责根据 state 和 action 计算出新的 state
const btnReducer = (state, action) => {
  let result;

  switch (action.type) {
    case "countUp":
      result = {
        ...state,
        count: state.count + 1
      };
      break;

    default:
      // result = state;
      // break;
      throw new Error("xxx")
  }
  return result;
};

const ActionDemo = () => {
    // 参数 1: reducer
    // 参数 2: state 初始化
    // 返回值: [当前状态, dispatch 函数]
  const [state, dispatch] = useReducer(btnReducer, {
    count: 0
  });
  return (
    <div>
      <button
        style={{
          padding: "5px 20px",
          marginTop: "10px",
          marginBottom: "10px",
          color: "#fff",
          backgroundColor: "#0066cc",
          borderRadius: "15px",
          border: "1px solid #fff"
        }}
        // dispatcher 负责发出 action, 
        onClick={() => dispatch({ type: "countUp" })}
      >
        up
      </button>
      <p>count: {state.count}</p>
    </div>
  );
};

export default ActionDemo;


```

### useEffect

#### effect 理解

effect（副作用）：指那些没有发生在数据向视图转换过程中的逻辑，如 ajax 请求、访问原生dom 元素、本地持久化缓存、绑定/解绑事件、添加订阅、设置定时器、记录日志等。

副作用操作可以分两类：需要清除的和不需要清除的

useEffect 接收一个函数，该函数会在组件渲染到屏幕之后才执行，该函数有要求：要么返回一个能清除副作用的函数，要么就不返回任何内容; 它跟 class 组件中的 componentDidMount、componentDidUpdate 和 componentWillUnmount 具有相同的用途，只不过被合并成了一个 API

与 componentDidMount 或 componentDidUpdate 不同，使用 useEffect 调度的 effect 不会阻塞浏览器更新屏幕，这让你的应用看起来响应更快。大多数情况下，effect 不需要同步地执行。在个别情况下（例如测量布局），有单独的 useLayoutEffect Hook 供你使用，其 API 与 useEffect 相同
  

#### effect 基本使用

```js
export default function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {// 相当于 componentDidMount
    console.log(`count: ${count}`);
  }, [count]);// 依赖 count 是否变化, [] 则没有依赖, 只在 render 后执行一次; 不带这个参数, 则每次 render 后都执行

  return (
    <div className="App">
      count: {count}
      <button onClick={() => setCount((count) => count + 1)}>Add</button>
    </div>
  );
}
```

#### 缓存上次值

useEffect 在 Render 完毕后才执行

```js
// 封装了一个简单的 Hooks 拿到上一次的值
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;//useEffect 在 Render 完毕后才执行, therefore ref 的值在当前 Render 中永远是上一次 Render 时候的
  });
  return ref.current;
}


```

#### 设置销毁回调函数例子

只有一个参数的例子:

```js
import React, { useState, useEffect } from "react";
let timer = null;
function App() {
  const [count, setCount] = useState(0);
  //一旦 count 值发生改变，则修改 documen.title 值
  useEffect(() => {
    document.title = "componentDidMount" + count;
  },[count]);

//数组没有传值，代表不监听任何参数变化，即只有在组件初始化或销毁的时候才会触发，用来代替 componentDidMount 和 componentWillUnmount
  useEffect(() => {
    timer = setInterval(() => {
      setCount(prevCount => prevCount + 1);
    }, 1000);
    // 返回值(如果有)则在组件销毁或者调用函数前调用
    return () => {
      document.title = "componentWillUnmount";
      clearInterval(timer);
    };
  }, []);
  
  return (
    <div>
      Count: {count}
      <button onClick={() => clearInterval(timer)}>clear</button>
    </div>
  );
}

```

#### 发送异步请求

```js
export default () => {
  const personList = usePerson();
  if (personList === null) {
    return <div>pending...</div>;
  }
  return (
    <div>
      {personList.map((person) => (
        <li key={person}>{person}</li>
      ))}
    </div>
  );
};

async function getPersonList() {
  await new Promise((resolve) => {
    setTimeout(() => {
      resolve();
    }, 200);
  });
  return ["aa", "bb", "cc"];
}

const usePerson = () => {
  const [personList, setPersonList] = useState(null);
  
  //useEffect 接收的函数，要么返回一个能清除副作用的函数，要么就不返回任何内容。而 async 返回的是 promise。
  useEffect(() => {
    async function req() {
      const personList = await getPersonList();
      setPersonList(personList);
    }
    req();
  }, []);
  return personList;
};

```

#### 使用 fetch api

```js
如何在 Hooks 中优雅的 Fetch Data
function App() {
  const [data, setData] = useState({ hits: [] });
  useEffect(() => {
    // 更优雅的方式
    const fetchData = async () => {
      const result = await axios(
        'https://api.github.com/api/v3/search?query=redux',
      );
      setData(result.data);
    };
    fetchData();
  }, []);
  return (
    <ul>
      {data.hits.map(item => (
        <li key={item.id}>
          <a href={item.url}>{item.title}</a>
        </li>
      ))}
    </ul>
  );
}


```

### 自定义 hook

#### 带有自增效果的 state hook

```js
export default () => {
  const [count, addCount] = useCountAdd(0);

  return (
    <div>
      count: {count}
      <button onClick={() => addCount()}>Add</button>
    </div>
  );
};

const useCountAdd = (init) => {
  const [count, setCount] = useState(init);
  return [
    count,
    () => {
      setCount((count) => count + 1);
    }
  ];
};
```

#### 从 usehooks 改造 responseComponent

实时更新的 response component:


```js
import React, { useState, useEffect } from "react";

const Response = ({param}) => {
    // 标识请求是否完成
  const [ok, setOk] = useState(false);
  // http 的响应
  const [response, setResponse] = useState({});

  // 参数 1: 是一个函数，异步操作的代码放在里面
  // 参数 2: 是一个数组，用于给出 Effect 的依赖项，只要这个数组发生变化，useEffect()就会执行。
  //第二个参数可以省略，这时每次组件渲染时，就会执行useEffect()。
  useEffect(() => {
    fetch(`http://www.httpbin.org/anything/${param}`)
      .then(resp => resp.json())
      .then(data => {
        setResponse(data);
        setOk(true);
      });
  }, [param]);

  if (!ok) {
    return <p>Nothing...</p>;
  }

  return (
    <div>
      <p>your IP: {response.origin}</p>
      <p>request url: {response.url}</p>
    </div>
  );
};

const EffectDemo = () => {
  const [param, setParam] = useState("one");

  return (
    <div>
      <Response param={param} />
      <p>
        you can choose:
        <button onClick={() => setParam("one")}>one</button>
        <button onClick={() => setParam("two")}>two</button>
        <button onClick={() => setParam("three")}>three</button>
      </p>
    </div>
  );
};

export default EffectDemo;


```

改造后:

```js
import React, {useState, useEffect } from "react";

const useResponse = param => {
  const [ok, setOk] = useState(false);
  const [response, setResponse] = useState({});

  useEffect(() => {
    setOk(false);
    fetch(`http://www.httpbin.org/anything/${param}`)
      .then(resp => resp.json())
      .then(data => {
        setResponse(data);
        setOk(true);
      });
  }, [param]);
  
  return [ok, response];
};

const Response = ({param}) => {
  const [ok, response] = useResponse(param);
  if (!ok) {
    return (
      <p>Nothing...</p>
    );
  }
  return (
    <div>
      <p>your IP: {response.origin}</p>
      <p>request url: {response.url}</p>
    </div>
  );
};


```

#### 定时器 and 自动销毁

定时器 (需要设置销毁函数)

```js
const useCountAdd = (init) => {
  const [count, setCount] = useState(init);
  return [
    count,
    () => {
      setCount((count) => count + 1);
    }
  ];
};

const useInterval = (callback, interval) => {
  useEffect(() => {
    const I = setInterval(callback, interval);
    return () => { // 相当于 componentWillunmount
      clearInterval(I);
    };
  }, []);
};

export default () => {
  const [count, addCount] = useCountAdd(0);

  useInterval(() => {
    addCount();
  }, 1000);

  return (
    <div>
      count: {count}
      <button onClick={addCount}>add</button>
    </div>
  );
};
```

定时自动销毁

```js
const useTimerCount = (initCount) => {
  const [count, setCount] = useState(initCount);

  const ref = useRef();
  useEffect(() => {
    ref.current = setInterval(() => {
      setCount((count) => count + 1);
    }, 1000);
  }, []);
  useEffect(() => {
    if (count > 6) {
      clearInterval(ref.current);
    }
  });

  return [count, setCount];
};

const useCounter = (count) => {
  return <div>timer count: {count}</div>;
};

export default () => {
  const [count, setCount] = useTimerCount(0);
  return useCounter(count);
};
```

### 综合例子

#### 点赞按钮

```js
import React, { useReducer, useState } from "react";
import "./styles.css";

const initState = {
  likes: 100,
  dislikes: 5
};

const appReducer = (state, action) => {
  switch (action.type) {
    case "click_like":
      return {
        ...state,
        likes: state.likes + action.payload
      };
    case "click_dislike":
      return {
        ...state,
        dislikes: state.dislikes + action.payload
      };
    default:
      return state;
  }
};

export default function App() {
  const [state, dispatch] = useReducer(appReducer, initState);
  const { likes, dislikes } = state;
  const [status, setStatus] = useState(null);

  const handleLike = () => {
    if (status === "like") {
      setStatus(null);
      dispatch({
        type: "click_like",
        payload: -1
      });
    } else if (status === null) {
      setStatus("like");
      dispatch({
        type: "click_like",
        payload: 1
      });
    } else {
      setStatus("like");
      dispatch({
        type: "click_dislike",
        payload: -1
      });
      dispatch({
        type: "click_like",
        payload: 1
      });
    }
  };

  const handleDisLike = () => {
    if (status === "dislike") {
      setStatus(null);
      dispatch({
        type: "click_dislike",
        payload: -1
      });
    } else {
      if (status === "like") {
        dispatch({
          type: "click_like",
          payload: -1
        });
      }

      setStatus("dislike");
      dispatch({
        type: "click_dislike",
        payload: 1
      });
    }
  };

  return (
    <div className="App">
      <button
        className={status === "like" ? "btn active" : "btn"}
        onClick={handleLike}
      >
        赞<span>{likes}</span>
      </button>
      <button
        className={status === "dislike" ? "btn active" : "btn"}
        onClick={handleDisLike}
      >
        踩<span>{dislikes}</span>
      </button>
    </div>
  );
}
```

```css
.App {
  display: flex;
  height: 100vh;
  align-items: center;
  justify-content: center;
}

.btn {
  cursor: pointer;
  border: none;
  font-size: 18px;
  line-height: 30px;
  background-color: transparent;
  color: #747777;
  outline: none;
  padding: 0 20px;
}

.btn:first-of-type {
  border-right: 2px solid #747777;
}

.btn:hover {
  color: #282a2e;
}

.active,
.active:hover {
  color: #1b81f5;
}

span {
  margin-left: 6px;
}
```

## 高阶组件 and 函数作为子组件

高阶组件 (HOC) 接受组件作为参数, 返回新的组件, 一般以函数的形式呈现

```jsx
export default function withTimer(WrappedComponent) {
    return class extends React.Component {
        state = { time: new Date() };
        componentDidMount() {
            this.timerID = setInterval(() => this.tick(), 1000);
        }

        componentWillUnmount() {
            clearInterval(this.timerID);
        }

        tick() {
            this.setState({
                time: new Date()
            });
        }
        render() {
            // props 原封未动
            // 增加了一个 time 属性
            return <WrappedComponent time={this.state.time} {...this.props} />;
        }
    };
}

// 使用的时候可以这样使用:
export class TabSelectorSample extends PureComponent {
    state = {
        color: null
    };
    render() {
        return (
            <div>
                Select color:{" "}
                <TabSelector
                    options={options}
                    value={this.state.color}
                    onChange={c => this.setState({ color: c })}
                />
                {/* time 属性是通过包装添加进来的 */}
                <p>{this.props.time.toLocaleTimeString()}</p>
            </div>
        );
    }
}

export default withTimer(TabSelectorSample);
```

函数作为子组件:

```jsx
const FunctionAsSonComponent = props => {
    return (
        <MyComponent>
        {/* 传入一个函数 */}
            {name => {
                return <div>{name}</div>;
            }}
        </MyComponent>
    );
};

// 使用: 

const MyComponent = props => {
    // 调用传入的函数, hello 就是函数的参数
    return <div>{props.children("hello")}</div>;
};
```

## Fragment

Fragments 看起来像空的 JSX 标签, 可以将多个元素作为返回值

```js
class Table extends React.Component {
  render() {
    return (
      <table>
        <tr>
          <Columns />
        </tr>
      </table>
    );
  }
}

class Columns extends React.Component {
  render() {
    return (
        // 这里就是一个 Fragment, jsx 规定无法直接返回多个元素, 通过 Fragment 可规避这个限制
        // 其实就是 <React.Fragment/> 的语法糖
        // 但是 <></> 语法不能接受键值或属性, 只有  <React.Fragment/> 可接受
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}

```

接受 key 属性的 demo:

```js
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // 没有`key`，将会触发一个key警告
        // key 是唯一可以传递给 Fragment 的属性
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}

```



## StrictMode

与 Fragment 类似，StrictMode 不呈现任何可见 UI。它为它的后代启动额外的检查和警告。(严格模式检查仅在开发模式下运行；它们不会影响生产构建。

)

```js
import React from 'react';

function ExampleApplication() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <ComponentOne />
          <ComponentTwo />
        </div>
      </React.StrictMode>
      <Footer />
    </div>
  );
}

```

## 示例 

每个 demo 都提供 3 种写法, 供对比.

### 使用 props

```js
// 组件类的第一个字母必须大写，否则会报错，比如HelloMessage不能写成helloMessage。另外，组件类只能包含一个顶层标签，否则也会报错。

/*************************
      demo1: 使用props
***************************/

// react api 写法

var HelloMsg = React.createClass({
    render: function() {
        return <h1>Hello, {this.props.name}</h1>;
    }
});

// state component 写法

class HelloMsg extends React.Component {
    constructor(props) {
        super(props);
        this.state = {};
    }
    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}

// Functional component , 单纯的ui, 没有 state, 没有ref, 仅仅一个构造函数

const HelloMsg = props => (
    // 注意这边 props 是传入函式的参数，因此取用 props 不用加 this
    <h1>Hello, {props.name}</h1>
);

ReactDOM.render(
    <HelloMsg name="xiaoyu"></HelloMsg>,
    document.getElementById("component")
);
```

### 遍历子元素

```js
// ------------------------------------------------------

/******************************
        demo2: 遍历子元素
*******************************************/

/* 遍历子元素, this.props.children获取到所有子元素, React.Children.map遍历 */

/* this.props.children可能有3种可能值 undefined: 没有子node, object: 仅仅一个子node, array: 多个子node */

var ChildNode = React.createClass({
    render: function() {
        return (
            <ol>
                {// React.Children 工具类, 还有很多工具方法, 参见文档
                React.Children.map(this.props.children, function(child) {
                    return <li>{child}</li>;
                })}
            </ol>
        );
    }
});

class ChildNode extends React.Component {
    // 这里为了方便, 没有 constructor, 因为没有设置props

    render() {
        return (
            <ol>
                {// React.Children 工具类, 还有很多工具方法, 参见文档
                React.Children.map(this.props.children, function(child) {
                    return <li>{child}</li>;
                })}
            </ol>
        );
    }
}

const ChildNode = props => (
    <ol>
        {// React.Children 工具类, 还有很多工具方法, 参见文档
        React.Children.map(props.children, function(child) {
            return <li>{child}</li>;
        })}
    </ol>
);
ReactDOM.render(
    <ChildNode>
        <div>test childNode div1</div>
        <div>test childNode div2</div>
    </ChildNode>,
    document.getElementById("childNode")
);

// ---------------------------------------------------------
```

### 找到真实的 dom(this.ref.xxx)

```js
/**********************************
        demo3: find a DOM node 找到真实的dom
*************************************/

/* find a DOM node 找到真实的dom */

var TrueDom = React.createClass({
    render: function() {
        return (
            <div>
                <input type="text" ref="textInput" />
                <input
                    type="button"
                    value="click to focus"
                    onClick={this.handleClick}
                />
            </div>
        );
    },
    handleClick: function() {
        // onclick 事件保证了只有虚拟dom插入document后才会使用this.refs属性, 就像放入到了 lifeCycle方法componentDidMount中一样
        this.refs.textInput.focus();
    }
});

class TrueDom extends React.Component {
    constructor(props) {
        super(props);
        this.state = {};
        // 与 ES5 React.createClass({}) 不同的是 component 内自定义的方法需要自行绑定到 this context, 如果去掉, 会找不到相应的refs
        this.handleClick = this.handleClick.bind(this);
    }
    render() {
        return (
            <div>
                <input type="text" ref="textInput" />
                <input
                    type="button"
                    value="click to focus"
                    onClick={this.handleClick}
                />
            </div>
        );
    }
    handleClick() {
        this.refs.textInput.focus();
    }
}
ReactDOM.render(<TrueDom></TrueDom>, document.getElementById("trueDom"));

// -------------------------------------------------------------------
```

### 使用 state(this.state)

```js
/*************************************
        demo4: 状态 this.state
***************************************/

/* 状态 this.state */

var MyState = React.createClass({
    getInitialState: function() {
        // 定义初始状态的state, getInitialState 方法用于定义初始状态，也就是一个对象，这个对象可以通过 this.state 属性读取
        return {
            liked: false
        };
    },
    render: function() {
        var text = this.state.liked ? "like" : "还没有开始喜欢";
        // 样式: 第一重大括号表示这是 JavaScript 语法，第二重大括号表示样式对象
        return (
            <p onClick={this.handleClick}>
                你<span style={{ color: "red" }}>{text}</span>这里(点击切换)
            </p>
        );
    },
    handleClick: function() {
        // 改变state
        this.setState({
            liked: !this.state.liked
        });
    }
});

class MyState extends React.Component {
    constructor(props) {
        super(props);
        this.handleClick = this.handleClick.bind(this);
        this.state = {
            liked: false
        };
    }
    render() {
        var text = this.state.liked ? "like" : "还没有开始喜欢";
        // 样式: 第一重大括号表示这是 JavaScript 语法，第二重大括号表示样式对象
        return (
            <p onClick={this.handleClick}>
                你<span style={{ color: "red" }}>{text}</span>这里(点击切换)
            </p>
        );
    }
    handleClick() {
        this.setState({
            liked: !this.state.liked
        });
    }
}
ReactDOM.render(<MyState></MyState>, document.getElementById("myState"));
```

### 一个表单示例

```js
/* 表单 */

var Form = React.createClass({
    getInitialState: function() {
        return {
            value: "hello"
        };
    },
    handleChange: function(event) {
        // 处理变化事件, 注意有方法参数: event, event.target.value 读取用户输入的值
        this.setState({
            value: event.target.value
        });
    },
    render: function() {
        // this.props 表示那些一旦定义，就不再改变的特性，而 this.state 是会随着用户互动而产生变化的特性。
        var value = this.state.value;
        return (
            <div>
                <input type="text" value={value} onChange={this.handleChange} />
                <p>{value}</p>
            </div>
        );
    }
});

class Form extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            value: "hello"
        };
        this.handleChange = this.handleChange.bind(this);
    }
    handleChange(event) {
        // 有event参数, 否则报错: ... is changing a controlled input of type text to be uncontrolled
        this.setState({
            value: event.target.value
        });
    }
    render() {
        // this.props 表示那些一旦定义，就不再改变的特性，而 this.state 是会随着用户互动而产生变化的特性。
        var value = this.state.value;
        return (
            <div>
                <input type="text" value={value} onChange={this.handleChange} />
                <p>{value}</p>
            </div>
        );
    }
}
ReactDOM.render(<Form></Form>, document.getElementById("form"));
```

### 登录界面

https://www.jianshu.com/p/efc080e6ecd9 //todo

### todo list app

https://www.jianshu.com/p/523eb1fc1504 //todo

# 组件的生命周期

## 生命周期介绍

http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/ ----- 交互式的演示

![](./Snipaste_2019-10-22_21-00-02.png)

Component 就像个状态机（State Machine），根据不同的 state（可通过`setState()` 修改）和 props（由父元素传入），Component 会出现对应的显示结果。

就像是 设计模式中的模板方法模式, react 提供给我们一系列 模板方法, 这些模板方法中的内容 会在组件生命周期中的某个阶段被调用


组件的生命周期大致可分为三个阶段:

-   render 阶段: 纯净无副作用, 渲染的是 virtual dom
-   pre-commit 阶段: 将 state 转换为 dom 操作, 这时还没有执行 dom 操作, 真实 dom 中的节点还是来自上一版 state/props 
-   commit 阶段: 执行 dom 操作, 将 virtual dom 真实反映到真实 dom 上 (这时 可以使用更新后的 dom 节点执行一些自定义操作)



对于一个组件, 可能接收三种操作, 因此 api 也可以分为三类:

-   被挂载
-   被更新
-   被卸载




## 组件被挂载

依次执行这些方法

1. constructor(props)  会在创建 组件时 (被挂载之前) 调用, 用于初始化 state, 还有组件方法的 binding

    - 如果组件有父类, 首先需要 `super(props)`

    - 如果 组件是 functional component, 则不需要 构造函数 (因为没有内状态)

    - 唯一可以直接修改 state 对象的地方 (通过 `this.state`), 其他地方想要修改 state 需要通过 `setState()`.

    - 要避免在构造函数中引入任何副作用(如 获取外部数据), `componentDidMount` 才是应该干这些事的地方

    - 避免将 props 的某个值复制给 state, 因为更新 props 的值并不会将这个值同步更行到 state 中. 可以直接使用 this.props.xxx

1. (不常用 )static getDerivedStateFromProps(props, state) 每次 render() 前都会触发 ; 返回一个对象来更新 state ,如果返回 null 则不更新任何内容 ; ([ref](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops))//todo

    - 如: state 需要从 props 计算得出时, 可以使用 ;

        比如: 表单控件获取默认初始值 (此时 state 中的表单项默认值是通过 props 传入的)

    - 不推荐使用 ( 因为 手动维护 state 和 props 的对应关系增加了复杂性 )

1. render() 渲染 ui, 唯一必须实现的方法; 每次 state 变化, 都会触发

    - 返回值只有四种:

        react 元素. (原生还是通过 jsx 自定义均可)

        数组 or [fragments](https://zh-hans.reactjs.org/docs/fragments.html) //todo

        [Portals](https://zh-hans.reactjs.org/docs/portals.html) //todo

        字符串 or 数字 (在真实 dom 中会被渲染成文本节点)

        boolean 类型 or null (用于支持返回 test &&  `<Child />` 的模式)

    - 应该为 pure function, 保证渲染结果仅仅和 state 有关   

    - 若 shouldComponentUpdate() 返回 false，则不会调用 render()

1. React 更新 真实 ­D­O­M 和 refs

1. componentDidMount() 进行依赖于 dom 节点 的初始化操作; 会在组件挂载后 (被插入 dom 树中) 调用

    - 如: 通过 ajax 获取数据; 添加订阅 (不要忘记在 componentWillUnmount() 里取消订阅)

    - 可以直接调用 setState() 触发渲染, 但此渲染会发生在浏览器更新屏幕之前 (因此用户不会感知到两次渲染)

    - 整个生命周期只执行一次

```js
constructor(props) {
    super(props);
    // 不要在这里调用 this.setState()
    this.state = { counter: 0 };
    this.handleClick = this.handleClick.bind(this);
}
```



## 组件被卸载

1. componentWillUnMount() 会在组件卸载之前调用; 可以 此方法中执行必要的清理操作, 用于资源释放

    - 例如，清除 timer，取消网络请求或清除在 componentDidMount() 中创建的订阅

    - 不应调用 setState()，因为该组件即将被卸载, 将永远不会重新渲染


## 组件被更新

1. static getDerivedStateFromProps(props, state)

2. (不常用) shouldComponentUpdate(nextProps, nextState) 在 state/props 发生变化后, render() 前触发; 用来自定义判断组件是否需要重新渲染

    - 默认返回值为 true, 也就是 state 每次发生变化组件都会重新渲染, 大多数时候默认即可

    - 首次渲染或 or 用 forceUpdate() 时不会调用该方法, 表示一律渲染

    - [性能调优](https://zh-hans.reactjs.org/docs/react-component.html#shouldcomponentupdate)

        可以使用 Pure Component, 默认实现了这个方法, 只对 props/state 进行浅层比较, 适用于 props/state 数据结构简单的情况

        手动编写此函数: 比对前后的 state/props, 返回 false以告知 React 可以跳过更新, 返回 false 并不会阻止子组件在 state 更改时重新渲染。react 不推荐 进行深层比较,会损害性能。

        若采用 immutable 可以使用 nextProps === this.props 比对是否有变动：

1. render()

1. (不常用) getSnapshotBeforeUpdate(prevProps, prevState) 在 render() 后, 在渲染输出到真实 dom 前触发; 用于在组件发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）

    - 此时 state 已经更新了

    - 返回值将作为最后一个参数传递给 componentDidUpdate(prevProps, prevState, [snapshot])。应返回 snapshot 的值（或 null）

    - 一般出现在ui处理中

1. React 更新 真实 ­D­O­M 和 refs

1. componentDidUpdate(prevProps, prevState, [snapshot]) 在渲染输出到真实dom后触发, 首次渲染不会执行此方法; 用来对更新后的 dom 进行操作

    - 场景: 页面需要根据 props 变化来重新获取数据 (如当 props 未发生变化时，则不执行网络请求)

    - 地位类似 组件被挂载时的 componentDidMount()

    - 可以直接调用 setState()，但它必须被包裹在一个条件语句，否则会导致死循环

    - 第三个参数 是 getSnapshotBeforeUpdate() 的返回值, 可能为 undefined

    - 如果 shouldComponentUpdate() 返回值为 false，则不会调用 componentDidUpdate()


```js
class ScrollingList extends React.Component {
  constructor(props) {
    super(props);
    this.listRef = React.createRef();
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    // 我们是否在 list 中添加新的 items ？
    // 捕获滚动​​位置以便我们稍后调整滚动位置。
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current;
      return list.scrollHeight - list.scrollTop;
    }
    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    // 如果我们 snapshot 有值，说明我们刚刚添加了新的 items，
    // 调整滚动位置使得这些新 items 不会将旧的 items 推出视图。
    //（这里的 snapshot 是 getSnapshotBeforeUpdate 的返回值）
    if (snapshot !== null) {
      const list = this.listRef.current;
      list.scrollTop = list.scrollHeight - snapshot;
    }

    // 典型用法（不要忘记比较 props）：
    if (this.props.userID !== prevProps.userID) {
        this.fetchData(this.props.userID);
    }

  }

  render() {
    return (
      <div ref={this.listRef}>{/* ...contents... */}</div>
    );
  }
}
```



## 生命周期函数 demo

demo1: 一个定时浮现隐藏的图标

```js
var Hello = React.createClass({
    getInitialState: function() {
        return {
            opacity: 1.0
        };
    },

    componentDidMount: function() {
        // 设定 定时器
        this.timer = setInterval(
            function() {
                var opacity = this.state.opacity;
                opacity -= 0.05;
                if (opacity < 0.1) {
                    opacity = 1.0;
                }
                this.setState({
                    opacity: opacity
                });
            }.bind(this),
            100
        ); // 每隔100ms, 就重新设置组件的透明度，从而引发重新渲染
        // 回调函数一定要加.bind(this)方法，原因是：在setInterval()中定义的回调函数，是在同步代码执行完后，随着事件触发来异步执行的，
        // 此时函数的上下文Context已经由定义该函数的Script文件变为全局变量，
        // 如果不通过bind(this)来指定由组件实例作为上下文的话，回调函数中的this会指向全局变量中的Window变量，显然不对
    },

    render: function() {
        // 样式: 写成 style="opacity:{this.state.opacity};" 是错误的
        return (
            <div>
                Hello {this.props.name}{" "}
                <span style={{ opacity: this.state.opacity }}>👈</span>
            </div>
        );
    }
});
class Hello extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            opacity: 1.0
        };
        this.intervalCallback = this.intervalCallback.bind(this); // 需要bind到当前组件(默认是绑定到全局window), 否则报错: Cannot read property 'opacity' of undefined
    }
    render() {
        return (
            <div>
                Hello {this.props.name}{" "}
                <span style={{ opacity: this.state.opacity }}>👈</span>
            </div>
        );
    }
    componentDidMount() {
        this.timer = setInterval(this.intervalCallback, 100);
    }
    intervalCallback() {
        var opacity = this.state.opacity;
        opacity -= 0.05;
        if (opacity < 0.1) {
            opacity = 1.0;
        }
        this.setState({
            opacity: opacity
        });
    }
}

ReactDOM.render(<Hello name="world" />, document.getElementById("lifeCycle"));
```

# virtual dom 和 diff 算法

## virtual dom

如果有大面积的操作 DOM，性能会是一个很大的问题，所以 React 实现了一个*Virtual DOM*，充当浏览器真实 dom 和 react 之间的中间层

这个 Virtual DOM 是一个纯粹的 JS 数据结构，在这个结构上, 实现了 diff 算法



 ## react 的 diff 算法
 
复杂度 O(n)

拿到前后两个状态的 virtual dom 树后, 会一层一层对每个节点进行比较; 

并且只会对比同一层的节点. 对于跨层级的差异, 会采用 新建 + 删除 操作, 不会遍历比较子节点

-   如果某一层, 对比前后状态发现, 只是节点顺序变化, 则操作真实 dom, 交换节点顺序,

-   如果某一层, 前后状态 某节点消失不见 or 新增, 则在真实 dom 上, 直接删除/新增该节点及所有子节点. 这样减少了很多不必要的遍历.

-   如果某一层, 发生节点跨层移动, 比如: 前后状态,发现 子节点 node1 移动到 兄弟节点 node2 下了, 那么 diff 算法会在 node2 下创建 node1 及其子结点, 创建成功后, 然后直接删除 node1 (新建 + 删除)


===================对于拥有不同 class 的2个组件

react 根本不会走到 diff 比较 virtual dom 这一步:

- 拥有不同类的两个组件有特殊规则: 前后两个组件将会生成不同的 dom 树结构，当 react 判断组件为 dirty component，从而替换整个组件下所有子节点

    如: 在开发中我们需要将 A 组件换成 B 组件，A 与 B 不是同类组件时，react 会毫不犹豫的删除 A，新建 B 及其子节点。不管 A 与 B 是否有相同的结构

    带来的问题: 假如不同的类，但是结构相似的组件相互替换，将会大大影响 react 性能。

    如何解决这个问题呢, 通过 shouldComponentUpdate()来手动判断是否需要更新组件



## key 属性有什么作用

key属性 用来唯一标识一个组件, 若渲染前后组件具有相同的key 则 react认为是同一个组件

- 如果渲染前后某个元素具有相同key, 若元素属性有所变化，则React只更新该组件对应的属性, 不涉及组件的挂载卸载，这种情况下，性能开销会相对较小

- 如果元素前后有不同的key, 那么在前后两次渲染中就会被认为是不同的元素，这时候旧的那个元素会被unmount，新的元素会被mount

所以为元素添加 key属性, 有助于提升性能; 且 key 需要满足 唯一, 稳定不变

不推荐使用数组/集合的index来做key值, 因为数组在执行插入、排序等操作之后，index并不再对应到原先的值，那么这个key就失去了本身的意义

不推荐使用 随机生成值 来做 key

## 设置组件时怎么提高性能


因此 diff 算法高效 有两个前提:

-   组件的 dom 结构相对稳定 (很少发生跨层移动, 增加/删除)

-   同类型的兄弟节点可以被唯一标识 (通过 key 属性)

所以设计组件, 应该遵守这样的原则:

-   ui 中的元素尽量只发生移动, 属性变化, 少发生 增加, 删除, 少发生跨层移动

    比如, 对于增加/删除节点, 可以使用样式控制节点的 display 实现

-   不同类的组件不要存在相似 DOM

    如: 对于两个组件具有相似的 dom 结构, 应该设计为同一个 组件类

# context-api 使用


# state 状态

## state 介绍

state 是组件内部维护的状态, props 是外部传入的属性; state 一般是真正的业务数据, props 一般是组件的配置, 从父组件传入, 定义后一般不变化

当组件状态 state 有更改的时候(通过 `setState()`更改, 直接修改的话, 组件不会重绘)，React 会自动调用组件的 render 方法重新渲染 (重绘 redraw) 整个组件的 UI

React Component 可以管理自己的内部 state, 在 constructor(props) 构造函数 中初始化 state, 并用 `this.state`来获取 state。通过 `setState(newState)` 方法更新 state, 更新后将重新调用 render() 方法，重新绘制 component 内容

```js
class Timer extends React.Component {
    constructor(props) {
        super(props);
        // 与 ES5 React.createClass({}) 不同的是 component 内自定义的方法需要自行绑定 this context，或是使用 arrow function
        this.tick = this.tick.bind(this);
        // 初始 state，等于 ES5 中的 getInitialState
        this.state = {
            secondsElapsed: 0
        };
    }
    // 累加器方法，每一秒被呼叫后就会使用 setState() 更新内部 state，让 Component 重新 render
    tick() {
        this.setState({ secondsElapsed: this.state.secondsElapsed + 1 });
    }
    // componentDidMount 为 component 生命周期中阶段 component 已插入节点的阶段，通常一些非同步操作都会放置在这个阶段。这便是每1秒钟会去呼叫 tick 方法
    componentDidMount() {
        this.interval = setInterval(this.tick, 1000);
    }
    // componentWillUnmount 为 component 生命周期中 component 即将移出插入的节点的阶段。这边移除了 setInterval 效力
    componentWillUnmount() {
        clearInterval(this.interval);
    }
    // render 为 class Component 中唯一需要定义的方法，其回传 component 欲显示的内容
    render() {
        return <div>Seconds Elapsed: {this.state.secondsElapsed}</div>;
    }
}

ReactDOM.render(<Timer />, document.getElementById("app"));
```

## state 和组件的普通属性区别

在 state 中的变量是和 ui 渲染相关的, 总是会在 render() 中使用到

组件的普通属性和 state 一样会在组件内部用到, 但是不是在 render() 中使用, 和 ui 渲染无关. 如: 组件中用到的定时器

```js
// 初始化
  constructor(props) {
    super(props);
    this.state = {}; // state 变量
    this.timer = 0; // 普通属性
  }
```

## 如何更新 state 

通过 setState 方法, 有两种使用方法

- setState(newState)

- setState((preState, props) => newState)

  可以同时传递一个 callback 回调函数, setState((preState, props) => newState, callback)




当State中的某个状态发生变化，我们应该重新创建这个状态对象，而不是直接修改原来的状态. 为什么呢

- 有利于性能提升.

    如果 整个 state 是可变对象, 那么说不定在哪个地方就改变了 state, 造成组件不必要的 重绘

    当对象组件状态都是不可变对象时，我们在组件的shouldComponentUpdate方法中，仅需要比较状态的引用就可以判断状态是否真的改变，从而避免不必要的render调用。

    我们使用React 提供的PureComponent时，更是要保证组件状态是不可变对象，否则在组件的shouldComponentUpdate方法中，状态比较就可能出现错误，因为PureComponent执行的是浅比较（比较对象的引用）。

- 不可变对象方便管理和调试

所以 state 应该是 Immutable 的, 目前是我们自己保证它是不可变的, 后面可以考虑使用 Immutable.js

```js
// 当状态发生变化时，如何创建新的状态呢？根据状态的类型，可以分成三种情况：

// 1. 状态的类型是不可变类型（数字，字符串，布尔值，null， undefined）

// 最简单, 直接给要修改的状态赋一个新值即可
this.setState({
  count: 1,
  title: 'Redux',
  success: true
})

////////////////////////////////////////////////////////////

// 2. 状态的类型是数组
//注意不要使用push、pop、shift、unshift、splice等方法修改数组类型的状态，因为这些方法都是在原数组的基础上修改，而concat、slice、filter会返回一个新的数组。


// 如有一个数组类型的状态books，当向books中增加一本书时，使用数组的concat方法或ES6的数组扩展语法（spread syntax）：

// 方法一：将state先赋值给另外的变量，然后使用concat创建新数组
var books = this.state.books; 
this.setState({
  books: books.concat(['React Guide']);
})
// 方法二：使用preState、concat创建新数组
this.setState(preState => ({
  books: preState.books.concat(['React Guide']);
}))
// 方法三：ES6 spread syntax
this.setState(preState => ({
  books: [...preState.books, 'React Guide'];
}))


// 当从books中截取部分元素作为新状态时，使用数组的slice方法：

// 方法一：将state先赋值给另外的变量，然后使用slice创建新数组
var books = this.state.books; 
this.setState({
  books: books.slice(1,3);
})
// 方法二：使用preState、slice创建新数组
this.setState(preState => ({
  books: preState.books.slice(1,3);
}))


//当从books中过滤部分元素后，作为新状态时，使用数组的filter方法：

// 方法一：将state先赋值给另外的变量，然后使用filter创建新数组
var books = this.state.books; 
this.setState({
  books: books.filter(item => {
    return item != 'React'; 
  });
})
// 方法二：使用preState、filter创建新数组
this.setState(preState => ({
  books: preState.books.filter(item => {
    return item != 'React'; 
  });
}))

////////////////////////////////////////////////////////////



// 3. 状态的类型是普通对象（不包含字符串、数组）



//使用ES6 的Object.assgin方法

// 方法一：将state先赋值给另外的变量，然后使用Object.assign创建新对象
var owner = this.state.owner;
this.setState({
  owner: Object.assign({}, owner, {name: 'Jason'});
})
// 方法二：使用preState、Object.assign创建新对象
this.setState(preState => ({
  owner: Object.assign({}, preState.owner, {name: 'Jason'});
}))


// 使用对象扩展语法（object spread properties）

// 方法一：将state先赋值给另外的变量，然后使用对象扩展语法创建新对象
var owner = this.state.owner;
this.setState({
  owner: {...owner, name: 'Jason'};
})
// 方法二：使用preState、对象扩展语法创建新对象
this.setState(preState => ({
  owner: {...preState.owner, name: 'Jason'};
}))

```


## setState 是不是异步的


经典例子:

```js
// demo.js
class Demo extends PureComponent {
  state={
    count: 0,
  }
  // 钩子函数 (在这里是异步的)
  componentDidMount() {
    console.log('pre state', this.state.count);
    this.setState({
      count: this.state.count + 1
    });
    console.log('next state', this.state.count);//前后没变化

    //测试setTimeout (这里是同步的)
    setTimeout(() => {
      console.log('setTimeout pre state', this.state.count);
      this.setState({
        count: this.state.count + 1
      });
      console.log('setTimeout next state', this.state.count);// 前后有变化
    }, 0);
  }

  onClick = (event) => {
    // 测试合成函数 (合成事件触发的函数) 中setState (这里是异步的)
    console.log(`${event.type} pre state`, this.state.count);
    this.setState({
      count: this.state.count + 1
    });
    console.log(`${event.type} next state`, this.state.count);// 前后没变化
  }

  render() {
    return <button onClick={this.onClick}>count+1</button>
  }
}
```

调用setState，组件的state并不会立即改变，setState只是把要修改的状态放入一个队列中，React会优化真正的执行时机，并且 React 会将多次setState的状态修改合并成一次状态修改. 

>所以不要依赖当前的State，计算下个State; 前后两个 state 应该没有任何联系; 同样不能依赖当前的Props计算下个 state，因为Props一般也是从父组件的State中获取，依然无法确定获取的当前 props 就是最新的

==================真的是异步的吗

setState本身的执行过程是同步的

但是在react的`合成函数 (自定义函数)`与`钩子函数 (生命周期函数)`中的 setState() 不会立马更新 state，所以不能立即拿到更新后的 state，形成了所谓的异步；


==================为什么需要有异步呢 //todo

- 保持 和 props 的一致性

    就算让 state 同步更新，props 也不行，因为当父组件重渲染（re-render ）了你才知道 props。

- 性能优化

    React 会给不同的 setState() 调用分配不同的优先级, 而不是立马更新 state, 防止并发的动作阻塞线程

- 可以实现 更新 state, 在幕后渲染新的页面, 还保持旧的页面可以交互



=======================可以是同步的吗

可以 

- 在原生事件触发的函数, setTimeout, async函数中是同步的

- 生命周期函数和合成函数中都是异步更新


## setState 更新是一个合并过程

State 的更新是一个浅合并（Shallow Merge）的过程, 所以 当调用setState修改组件状态时，只需要传入发生改变的State，而不是组件完整的State

```js
// setState 等价于
Object.assign(preState, newState)



// 例如
// 初始化 state
this.state = {
  title : 'React',
  content : 'React is an wonderful JS library!'
}
// 更新 state
this.setState({title: 'Reactjs'});
// 最终合并后的 state 是这样的:
{
  title : 'Reactjs',
  content : 'React is an wonderful JS library!'
}


```

## replaceState 更新是一个替换过程

replaceState()方法与setState()类似，但是方法只会保留nextState中状态，原state不在nextState中的状态都会被删除

```js
//使用语法：
replaceState(object nextState[, function callback])

// nextState，将要设置的新状态，该状态会整个替换当前的state。
// callback，可选参数，回调函数。该函数会在replaceState设置成功，且组件重新渲染后调用。
```


# props 属性

## 在 jsx 中遍历 props

使用 "..." 遍历 props

```js
var props = {
    style: "width:20px",// 报错, 存疑; 可能style需要单独一个对象, 或者 <HelloMessage style={{ color: '#FFFFFF', fontSize: '30px'}} />
    className: "main",
    value: "yo",  // 会被替换
}

<HelloMessage  {...props} value="yo" />

// 等于以下
React.createElement("h1", React._spread({}, props, {value: "yo"}), "Hello React!");
```

## PropTypes 类型检查

```js
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string

  // 限制某个props为js"原生类型"
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,// 不推荐
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,// 类似于字符型的数据类型, 独一无二的值, 给object新增属性名称 let s = Symbol();let s1 = Symbol('s1');(ref:https://www.jianshu.com/p/174f88d189c6)

  // 任何可被渲染的元素（包括数字、字符串、子元素或数组）。
  optionalNode: PropTypes.node,//  numbers, strings, elements or an array

  // 一个 React 元素
  optionalElement: PropTypes.element,// A React element.

  optionalMessage: PropTypes.instanceOf(Message),// 某个类的实例

  optionalEnum: PropTypes.oneOf(['News', 'Photos']),// [某个系列]中的一个, 类似枚举类型
  optionalUnion: PropTypes.oneOfType([// 几种类型中的一个
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),// 某种类型的数组

  // 一个指定类型的对象
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),// 对象, 所有属性都是某一特定类型
  // 一个指定属性及其类型的对象
  optionalObjectWithShape: PropTypes.shape({// 对象, 规定各个属性不同的类型
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // 必须， 这样如果这个属性父组件没有提供时，会打印警告信息
  requiredFunc: PropTypes.func.isRequired,
  requiredAny: PropTypes.any.isRequired,// any不推荐

   // 你也可以指定一个自定义验证器。它应该在验证失败时返回
  // 一个 Error 对象而不是 `console.warn` 或抛出异常。
  // 不过在 `oneOfType` 中它不起作用。
  customProp: function(props, propName, componentName) {// 自定义 validator
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },
  // 自定义 validator to `arrayOf` and `objectOf`.
  // The validator  will be called for each key in the array or object.
  // The first two arguments of the validator are the array or object itself, and the current item's key.
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```

for example:

```js
//  注意组件开头第一个字母都要大写
class MyComponent extends React.Component {
    // render 是 Class based 组件唯一必须的方法（method）
    render() {
        return <div>Hello, World!</div>;
    }
}

// PropTypes 验证，若传入的 props type 不符合将会显示错误
MyComponent.propTypes = {
    todo: React.PropTypes.object,
    name: React.PropTypes.string
};

// Prop 预设值，若对应 props 没传入值将会使用 default 值
MyComponent.defaultProps = {
    todo: {},
    name: ""
};
```

## 引入类型检查的其他方法

### 引入 TypeScript

JavaScript 的超集

https://react-typescript-cheatsheet.netlify.app/docs/basic/setup
https://www.typescriptlang.org/docs/handbook/interfaces.html

```sh
npx create-react-app hook-ts-demo --template typescript

```


### flow 类型检查

Facebook 自己的工具

## 设定 props 的默认值

```js
class Greeting extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}

// Specifies the default values for props:
Greeting.defaultProps = {
    name: "Stranger"
};
```

or

```js
class Greeting extends React.Component {
    static defaultProps = {
        name: "stranger"
    };

    render() {
        return <div>Hello, {this.props.name}</div>;
    }
}
```

## 有哪些属性

和 react-router 合作时候, 将 react-router 的 history,location,match 三个对象绑定到 props, 打印 props :

![alt](Snipaste_2018-06-25_17-20-23.png)

# refs 

表单处理与第三方 lib 集成

ref 介绍参见文档: https://react-cn.github.io/react/docs/more-about-refs.html

ref 在 redux 中使用: https://segmentfault.com/a/1190000015172005

和第三方 js lib 集也会用到 Refs

```js
class MarkdownEditor extends React.Component {
    constructor(props) {
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.rawMarkup = this.rawMarkup.bind(this);
        this.state = {
            value: "Type some *markdown* here!"
        };
    }
    // 另一种处理方法是: 借助 onChange(event), 通过 event.target.value 拿到实的值d
    handleChange() {
        this.setState({ value: this.refs.textarea.value });
    }
    // 将使用者输入的 Markdown 语法 parse 成 HTML 放入 DOM 中，React 通常使用 virtual DOM 作为和 DOM 沟通的中介，不建议直接操作 DOM。故使用时的属性为 dangerouslySetInnerHTML
    rawMarkup() {
        const md = new Remarkable();
        return { __html: md.render(this.state.value) };
    }
    render() {
        return (
            <div className="MarkdownEditor">
                <h3>Input</h3>
                <textarea
                    onChange={this.handleChange}
                    // ref 可以是一个id, 也可以是一个函数
                    ref="textarea"
                    defaultValue={this.state.value}
                />
                <h3>Output</h3>
                <div
                    className="content"
                    dangerouslySetInnerHTML={this.rawMarkup()}
                />
            </div>
        );
    }
}

ReactDOM.render(<MarkdownEditor />, document.getElementById("app"));
```


# 合成事件

react为了解决跨平台，兼容性问题，自己封装了一套事件机制，代理了原生的事件，像在jsx中常见的onClick、onChange这些都是合成事件

demo: TodoList app

```js
// TodoApp 组件中包含了显示 Todo 的 TodoList 组件，Todo 的内容透过 props 传入 TodoList 中。由于 TodoList 仅单纯 Render UI 不涉及内部 state 操作是 stateless component，所以使用 Functional Component 写法。

// 需要特别注意的是用 map function 来迭代 Todos，需要留意的是每个迭代的元素必须要有 unique key 不然会发生错误（可以用自定义 id，或是使用 map function 的第二个参数 index）

const TodoList = props => (
    <ul>
        {props.items.map(item => (
            <li key={item.id}>{item.text}</li>
        ))}
    </ul>
);

// 整个 App 的主要组件

// 怎么获取input表单项的value, 有多种方法, 这是第一种: 借助 onchange事件和state

class TodoApp extends React.Component {
    constructor(props) {
        super(props);
        this.onChange = this.onChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
        this.state = {
            items: [],
            text: ""
        };
    }
    onChange(e) {
        this.setState({ text: e.target.value });
    }
    handleSubmit(e) {
        e.preventDefault();
        const nextItems = this.state.items.concat([
            { text: this.state.text, id: Date.now() }
        ]);
        const nextText = "";
        this.setState({ items: nextItems, text: nextText });
    }
    render() {
        return (
            <div>
                <h3>TODO</h3>
                <TodoList items={this.state.items} />
                <form onSubmit={this.handleSubmit}>
                    <input onChange={this.onChange} value={this.state.text} />
                    <button>{"Add #" + (this.state.items.length + 1)}</button>
                </form>
            </div>
        );
    }
}

// 第二种获取input值的方法: 借助 submit事件

class MyOwnListApp extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            items: []
        };
        this.onSubmit = this.onSubmit.bind(this);
    }
    render() {
        return (
            <div>
                <h3>MyOwnListApp</h3>
                <MyOwnList items={this.state.items}></MyOwnList>
                <form onSubmit={this.onSubmit}>
                    <input type="text" />
                    <button>submit</button>
                </form>
            </div>
        );
    }
    onSubmit(event) {
        event.preventDefault();
        /* this.state.items.push([{// 这样子ui不会自动redraw
            id: Date.now(),
            value: event.target[0].value
        }]); */
        this.setState({
            // 现在才会auto redraw
            items: this.state.items.concat([
                {
                    id: Date.now(),
                    value: event.target[0].value
                }
            ])
        });
        event.target[0].value = ""; // 清空
    }
}

ReactDOM.render(<TodoApp />, document.getElementById("app"));
```

也可以自己绑定原生事件:

```js
//手动绑定mousedown事件
    ReactDom.findDOMNode(this).addEventListener(
      "mousedown",
      this.onClick.bind(this)
    );
```


# 状态管理

Data Flow 和 Flux/Redux

Flux 架构: 一种方便应用状态管理的架构 -- “单向数据流”, 是 React 推崇的一种应用架构的方式, app 足够复杂才能体现方便 (React 本身只涉及 UI 层，如果搭建大型应用，app 的 state, 组件之间的通信 会变得无比复杂, 这时 Flux 架构就有了用武之地).

核心思想: 当 view 上用户发生了操作, app 会通过 "Action Creator" 创建一个 "Action", 通过 "Dispatcher" 分发出去, 由 "Store" 进行处理. "Store" 和 "View" 是绑定的,Store 产生变化,View 也会同步变化

Redux: 是 flux 架构的一个具体实现， 将 Flux 架构 与函数式编程结合一起

同类: mobx ,Reflux,Flummox。

同时, 如果 app 比较简单, 是不需要的 redux 来管理 state 的, redux 只适用于"多交互、多数据源"的情况

## flux 架构的思想

https://www.zhihu.com/question/33864532/answer/57667838

```
  ╔═════════╗       ╔════════╗       ╔═════════════════╗
  ║ Actions ║──────>║ Stores ║──────>║ View Components ║
  ╚═════════╝       ╚════════╝       ╚═════════════════╝
       ^                                      │
       └──────────────────────────────────────┘
```

将各个组件的 state 集中到 Store, 统一管理, 各个组件和 Store 交互, 简化组件之间的通信

View 层不能直接对 state 进行操作，而需要依赖 Actions 派发指令来告知 Store 修改状态，Store 接收 Actions 指令后发生相应的改变，View 层同时跟着 Store 的变化而变化

举个例子：A 组件要使 B 组件发生变化。首先，A 组件需要将这个意图封装为一个 action 执行，告知绑定 B 组件的 Store 发生变化，Store 接收到派发的指令后改变，那相应的 B 组件的视图也就发生了改变。假如 C，D，E，F 组件绑定了和 B 组件相同的 Store，那么 C，D，E，F 也会跟着变化。

## Flux 官方实现

Advantages:

-   让开发者可以快速了解整个 App 中的行为(在 store 中看就行了)

-   数据和业务逻辑统一存放好管理

-   让 View 单纯化只负责 UI 的排版不需负责 state 管理

Disadvantages:

-   代码忒复杂了, 写 ✍ 小应用不合算

有哪些概念?

-   actions ：一些列动作(就是函数), 负责定义所有改变 state（状态）的行为，可以让开发者快速了解 App 的各种功能，若你想改变 state 你只能发 action。注意 action 可以是同步或是非同步

-   Dispatcher : 是 Flux 架构的核心，每个 App 只有一个 Dispatcher，并负责向所有 store 发送 action 事件(store.dispatch())

-   Store: 一个 App 通常会有多个 store 负责存放业务逻辑，根据不同业务会有不同 store; store 负责操作和储存数据(state);

    -   值得注意的是 store 只提供 getter API 读取资料，若想改变 state 一律发送 action;

    -   提供 API 让 store 可以注册 callback function(就是注册 listener, 指定 state 改变的回调函数(一般是 setState()触发 redraw), EventEmitter.on(actionType, callbackFn))。

    -   需要在 store 里, 通过 AppDispatcher.register(fn) 注册处理 action 的函数(因为 store 模块这里才能直接访问 store 内部的 state), 处理完毕, 通过 EventEmitter.emit(actionType) 触发 listener

-   View: 这部份是 React 负责的范畴，负责提供监听事件的 callback function，当事件发生时重新取得数据并重绘 View

## Redux

### 为什么要使用 redux

下图展示了使用 redux 和不使用 redux 时, state 和组件间的交互关系:

![alt](Snipaste_2018-06-27_12-45-41.png)

-   整合了 react-redux 后，我们的 React App 就可以解决传统跨 Component 之间传递 state 的问题和困难。只要通过 Provider 就可以让每个 React App 中的 Component 取用 store 中的 state，非常方便
-   数据和业务逻辑放到了一个地方, 一目了然

### 基本概念

http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html

-   State: Store 对象可以包含多个 节点 。如果想得到某个时刻的数据，就要对 Store 生成快照。这种时间切片的数据集合，就叫做 State;

    -   其本质就是一个 js 对象, `const state = store.getState();` 可以得到当前时刻的 state

        ```js
        {
          counter: 0,
          todos: [] // 待办项
        }
        ```

    -   一个 State 对应一个 View。只要 State 相同，View 就相同.

    -   若要改变 state，必须 dispatch 一个 action

-   Action: 就是 View 发出的通知，表示 State 应该要发生变化了

    -   本质就是一个 js 对象, 但是一定要包含"type "属性, 其他没有要求, 但是也有规范

        ```js
        {
          type: 'ADD_TODO',
          payload: {
            id: 1,
            content: '待办事项1',
            completed: false
          }
        }
        ```

        如果需要新增一个 todo 项，实际上就是将 payload 写入到 state.todos 数组中

-   Action Creator: 定义一个函数来生成 Action

    -   示例:

        ```js
        var id = 1;
        function addTodo(content) {
            return {
                type: "ADD_TODO",
                payload: {
                    id: id++,
                    content: content, // 待办事项内容
                    completed: false // 是否完成的标识
                }
            };
        }
        ```

    -   Action Creator 可以是同步的，也可以是异步的

-   Reducer: Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。

    -   Reducer 必须是同步的纯函数，它接受 Action 和当前 State 作为参数，返回一个新的 State(Reducer 函数里面不能改变 State，必须返回一个全新的对象)。

    -   store.dispatch(action)之后, 会触发 reducer 执行(创建 store 时候, 将 reducer 传入了)

    -   最好把 State 对象设成只读。你没法改变它，要得到新的 State，唯一办法就是生成一个新对象。这样的好处是，任何时候，与某个 View 对应的 State 总是一个不变的对象

        -   (可以借助 ImutableJs 库)

        -   可以使用 lodash 的 cloneDeep

        -   也可以使用 Object.assign / map / filter/ ... 等返回副本的函数

    -   示例:

        ```js
        var initState = {
            counter: 0,
            todos: []
        };

        function reducer(state, action) {
            // ※ 应用的初始状态是在第一次执行 reducer 时设置的 ※
            if (!state) state = initState;

            switch (action.type) {
                case "ADD_TODO":
                    var nextState = _.cloneDeep(state); // 用到了 lodash 的深克隆
                    nextState.todos.push(action.payload);
                    return nextState;

                default:
                    // 由于 nextState 会把原 state 整个替换掉
                    // 若无修改，必须返回原 state（否则就是 undefined）
                    return state;
            }
        }
        ```

![alt](Snipaste_2018-06-27_09-07-13.png)

> Redux 数据流的模型大致上可以简化成：首先，用户和 view 交互发出 Action (包含 type, data 这两个基本属性): `store.dispatch(action);`然后，Store 自动调用 Reducer (即处理 action 的函数)，并且传入两个参数：当前 State 和收到的 Action。 Reducer 会返回新的 State : `let nextState = todoApp(previousState, action);` State 一旦有变化，Store 就会调用监听函数, 也就是 `store.subscribe(listener);` 中的 listener, listener 可以通过`store.getState()`得到当前状态然后 set 回去, 如果使用的是 React，这时可以触发重新渲染 View。

-   Web 应用是一个状态机，视图与状态是一一对应的。
-   所有的状态，保存在一个对象里面 (Store)。
-   此外, redux 有 synchronous（同步）的情形，asynchronous（非同步）情形

### 基本 api

https://github.com/kenberkeley/redux-simple-tutorial/blob/master/redux-advanced-tutorial.md

#### store

`const store = createStore(reducer)`

-   getState()
-   dispatch(action)
-   subscribe(listener)

下面看看详细解释:

Store: Redux 只有一个 Store 负责存放整个 App 的 State, 一个 Store 可以包含多个 state 节点, 一个 state 节点 对应一个 reducer; 唯一能改变 State 的方法只有通过 store 发送 action。

怎么创建?

createStore(reducer, [preloadedState], [enhancer]) 生成 store, 常用 `const store = createStore(reducer);` createStore 函数接受另一个函数作为参数，返回新生成的 Store 对象

-   第一个参数放入我们的 reducer 或是有多个 reducers combine（使用 combineReducers()）在一起的 rootReducers。

-   第二个参数我们会放入希望预先载入的 state 例如：user session 等。(如果提供了这个参数，它会覆盖 Reducer 函数的默认初始值。) 主要用于前后端同构时的数据同步

-   第三个参数通常会放入我们想要使用用来增强 Redux 功能的 middlewares，若有多个 middlewares 的话，通常会使用 applyMiddleware 来整合。

看看示例:

```js
import { createStore } from 'redux'
...
const store = createStore(reducer, initialState) // initialState 主要用于前后端同构的数据同步（详情请关注 React 服务端渲染）
```

属于 Store 的四个方法:

-   getState() 获取当前时刻的 state

-   dispatch(action) View 发出 Action 的唯一方法

    -   内部会触发 Reducer 的执行, 为此, Store 需要知道 Reducer 函数, 做法就是在生成 Store 的时候, 将 Reducer 传入 createStore 方法.

-   subscribe(listener) 设置监听函数，一旦 State 发生变化，就自动执行这个函数

    -   只要把 View 的更新函数（对于 React 项目，就是组件的 render 方法或 setState 方法）放入 listen，就会实现 View 的自动渲染。

    -   store.subscribe 方法返回一个函数，调用这个函数就可以解除监听。

-   replaceReducer(nextReducer) 一般在 Webpack Code-Splitting 按需加载的时候用

#### reducer

reducer 就是一个函数, 接收 old state 和 action, 生成 new state;

combineReducers：combineReducers(reducers) , combineReducers 可以将多个 reducers 进行整合并回传一个

```js
import { combineReducers } from 'redux';

// 这种写法有一个前提，就是 State 的属性名必须与子 Reducer 同名
const chatReducer = combineReducers({
  chatLog,
  statusMessage,
  userName
})

// 如果不同名，就要采用下面的写法
// 根据 State 的 key 去执行相应的子 Reducer，并将返回结果合并成一个大的 State 对象。
const reducer = combineReducers({
  a: doSomethingWithA,
  b: processB,`
  c: c
})
// 等同于
function reducer(state = {}, action) {
  return {
    a: doSomethingWithA(state.a, action),
    b: processB(state.b, action),
    c: c(state.c, action)
  }
}

export default todoApp;
```

#### applyMiddleware

redux 提供的工具函数

applyMiddleware(...middlewares) 让开发者可以在 req 和 res 之间进行一些操作。在 Redux 中 Middleware 则是扮演 action 到达 reducer 前的第三方扩充。而 applyMiddleware 可以将多个 middlewares 整合并回传一个 Function，便于使用。

#### bindActionCreators

工具函数

bindActionCreators(actionCreators, dispatch) bindActionCreators 可以将 actionCreators 和 dispatch 绑定，并回传一个 Function 。但若是使用 react-redux 可以用 connect 让 dispatch 行为更容易管理

-   compose(...functions) 将 function 由右到左合并并回传一个 Function

    ```js
    const store = createStore(
        reducer,
        compose(applyMiddleware(thunk), DevTools.instrument())
    );
    ```

### 纯 redux 的 api 示例(同步版本的 api)

```js
import React from "react";
import { createStore, combineReducers, bindActionCreators } from "redux";

function run() {
    // Store initial state
    const initialState = { count: 0 };

    // reducer
    // 代表 state 的处理逻辑
    const counter = (state = initialState, action) => {
        switch (action.type) {
            case "PLUS_ONE":
                return { count: state.count + 1 };
            case "MINUS_ONE":
                return { count: state.count - 1 };
            case "CUSTOM_COUNT":
                return {
                    count: state.count + action.payload.count
                };
            default:
                break;
        }
        return state;
    };

    const todos = (state = {}) => state; // 待添加到 Store 中的新节点

    // Create store
    const store = createStore(
        // combineReducers 可以帮助维护多个 state 进 store 中
        //这样 Store 中就有 两个节点了
        combineReducers({
            todos,
            counter
        })
    );

    // Action creator
    function plusOne() {
        // action
        return { type: "PLUS_ONE" };
    }

    function minusOne() {
        return { type: "MINUS_ONE" };
    }

    function customCount(count) {
        return { type: "CUSTOM_COUNT", payload: { count } };
    }

    // bindActionCreators 是 redux 提供的工具函数, 将 dispatcher 绑定到 action 内部
    // 省掉了手动 dispatch 这直接调用action 即可
    plusOne = bindActionCreators(plusOne, store.dispatch);

    store.subscribe(() => console.log(store.getState()));
    // store.dispatch(plusOne());
    plusOne();
    store.dispatch(minusOne());
    store.dispatch(customCount(5));
}
export default () => (
    <div>
        <button onClick={run}>Run</button>
        <p>* 请打开控制台查看运行结果</p>
    </div>
);
```

### react-redux

react-redux 提供 一个函数 `connect`, 通过高阶组件的方式, 将 redux 中的 store 和 react 中的 component 链接起来

```jsx
import React from "react";
import { bindActionCreators, createStore } from "redux";
import { Provider, connect } from "react-redux";

// Store initial state
const initialState = { count: 0 };

// reducer
const counter = (state = initialState, action) => {
    switch (action.type) {
        case "PLUS_ONE":
            return { count: state.count + 1 };
        case "MINUS_ONE":
            return { count: state.count - 1 };
        case "CUSTOM_COUNT":
            return { count: state.count + action.payload.count };
        default:
            break;
    }
    return state;
};

// Create store
const store = createStore(counter);

// Action creator
function plusOne() {
    // action
    return { type: "PLUS_ONE" };
}

function minusOne() {
    return { type: "MINUS_ONE" };
}

export class Counter extends React.Component {
    render() {
        // 在这里通过 props 可以拿到 state 中我们关心的数据 和 actions
        const { count, plusOne, minusOne } = this.props;
        return (
            <div className="counter">
                <button onClick={minusOne}>-</button>
                <span style={{ display: "inline-block", margin: "0 10px" }}>
                    {count}
                </span>
                <button onClick={plusOne}>+</button>
            </div>
        );
    }
}

/**
 * bind state data of which wo take care to props
 * 绑定state中我们关心的数据到 props (不能整个 store 绑定到 props, 因为 state 中和当前组件无关的数据更新无需出发组件重绘
 * @param state store.getState()
 */
function mapStateToProps(state) {
    return {
        count: state.count
    };
}

/**
 * 绑定 action 到 props
 * @param dispatch
 */
function mapDispatchToProps(dispatch) {
    return bindActionCreators({ plusOne, minusOne }, dispatch);
}

// 连接
const ConnectedCounter = connect(mapStateToProps, mapDispatchToProps)(Counter);

export default class CounterSample extends React.Component {
    render() {
        return (
            // 根节点通过 Provider 提供 store, 子组件就可以随时访问 store 中的数据
            <Provider store={store}>
                <ConnectedCounter />
            </Provider>
        );
    }
}
```

### 异步 Action

要使用 asynchronous（非同步）的行为的话需要使用其中一种 middleware： redux-thunk、redux-promise 或 redux-promise-middleware ，这样可以让你在 actions 中 dispatch Promises 而非 function。

![alt](./Snipaste_2019-10-26_21-38-03.png)

过程是这样的: 用户在 view 触发一个 action (就是普通 action 并没有所谓的 异步 action), 并没有被 dispatch 给 reducer, 而是被 Middlewares 拦截, 内部处理后, 生成新的标准的 action ,给 reducer;

所谓的 异步 action, 并不是特殊的 action, 而是多个 action 的组合使用

Middleware 在 dispatcher 中对 action 进行拦截进行特殊处理

### thunk, saga

todo

### 调试工具

Redux 开发调试工具：redux-devtools
React 应用无刷新保存工具：react-transform

### demo(一个 todo-app)

![alt](Snipaste_2018-06-27_11-57-23.png)

整个 React Redux App 的资料流程图（使用者与 View 互动 => dispatch 出 Action => Reducers 依据 action tyoe 分配到对应处理方式，回传新的 state => 透过 React Redux 传送给 React，React 重新绘制 View）：

react-redux 整合 react 和 redux,

![alt](Snipaste_2018-06-27_12-43-37.png)

## Redux 和 Flux 的一些差异

![alt](Snipaste_2018-06-27_00-31-39.png)

-   只使用一个 store 将整个应用程式的状态 (state) 用物件树 (object tree) 的方式储存起来; 原生的 Flux 会有许多分散的 store 储存各个不同的状态

    ```js
    //原生 Flux 的 store
    const userStore = {
        name: ""
    };
    const todoStore = {
        text: ""
    };

    // Redux 的单一 store
    const state = {
        userState: {
            name: ""
        },
        todoState: {
            text: ""
        }
    };
    ```

-   Redux 并没有像 Flux 设计有 Dispatcher(唯一可以改变 state 的方法就是发送 action，这部份和 Flux 类似, action 都是一个包含 type 和 payload 的对象); Redux 拥有 Flux 所没有的 Reducer。Reducer 根据 action 的 type 去执行对应的 state 做变化的函式叫做 Reducer。你可以使用 switch 或是使用函式 mapping 的方式去对应处理的方式

-   Redux 拥有许多方便好用的辅助测试工具（例如：redux-devtools、react-transform-boilerplate），方便测试和使用 Hot Module Reload。

## ImmutableJS

https://www.youtube.com/watch?v=I7IdS-PbEgI
http://react-immutable-proptypes/ (适用于 imutable 的 proptypes 验证)

为什么需要 不可变数据?

-   优化性能 (react 判断组件是否要刷新是根据 state 是否有变化来决定的, 采用不可变数据那么 state 一旦, react 可以比较 新旧 两个 state 是否是一个对象来决定是否刷新组件, 而不必进入 state 内部去比较属性是否有变化)
-   容易调试跟踪 (随时可以看到 新旧 stat 的具体值, 可以方便的对比)

redux 运行的基础需要保 state 是不可变数据

es6 原生提供 api 不可变数据修改:

```js
const state = {
    filter: "completed",
    todos: ["a"]
};
// 写法1:
const newState = {
    ...state,
    todos: [...state.todos, "b"]
};
// 写法2
const newState2 = Object.assign({}, state, { todos: [...state.todos, "b"] });
```

ImmutableJs 提供了修改工具类:

```js
import update from 'immutability-helper';

...
```

immer 也有相关工具类:

```js
import produce from 'immer';

...
```

ImmutableJS 封装了自己的数据结构: 7 种不可修改的类型：List、Map、Stack、OrderedMap、Set、OrderedSet、Record。

```js
import Immutable from "immutable";

var map1 = Immutable.Map({ a: 1, b: 3 });
var map2 = map1.set("a", 2); // 返回一个新的map

map1.get("a"); // 1
map2.get("a"); // 2
```

```js
const Map= Immutable.Map;

// 1. Map 大小
const map1 = Map({ a: 1 });
map1.size
// => 1

// 2. 新增或取代 Map 元素
// set(key: K, value: V)
const map2 = map1.set('a', 7);
// => Map { "a": 7 }

// 3. 删除元素
// delete(key: K)
const map3 = map1.delete('a');
// => Map {}

// 4. 清除 Map 内容
const map4 = map1.clear();
// => Map {}

// 5. 更新 Map 元素
// update(updater: (value: Map<K, V>) => Map<K, V>)
// update(key: K, updater: (value: V) => V)
// update(key: K, notSetValue: V, updater: (value: V) => V)
const map5 = map1.update('a', () => (7))
// => Map { "a": 7 }

// 6. 合并 Map
const map6 = Map({ b: 3 });
map1.merge(map6);
// => Map { "a": 1, "b": 3 }
--------------------------
const List= Immutable.List;

// 1. 取得 List 长度
const arr1 = List([1, 2, 3]);
arr1.size
// => 3

// 2. 新增或取代 List 元素内容
// set(index: number, value: T)
// 将 index 位置的元素替换
const arr2 = arr1.set(-1, 7);
// => [1, 2, 7]
const arr3 = arr1.set(4, 0);
// => [1, 2, 3, undefined, 0]

// 3. 删除 List 元素
// delete(index: number)
// 删除 index 位置的元素
const arr4 = arr1.delete(1);
// => [1, 3]

// 4. 插入元素到 List
// insert(index: number, value: T)
// 在 index 位置插入 value
const arr5 = arr1.insert(1, 2);
// => [1, 2, 2, 3]

// 5. 清空 List
// clear()
const arr6 = arr1.clear();
// => []
------------------------
const Set= Immutable.Set;

// 1. 建立 Set
const set1 = Set([1, 2, 3]);
// => Set { 1, 2, 3 }

// 2. 新增元素
const set2 = set1.add(1).add(5);
// => Set { 1, 2, 3, 5 }
// 由于 Set 为不能重复集合，故 1 只能出现一次

// 3. 删除元素
const set3 = set1.delete(3);
// => Set { 1, 2 }

// 4. 取联集
const set4 = Set([2, 3, 4, 5, 6]);
set1.union(set4);
// => Set { 1, 2, 3, 4, 5, 6 }

// 5. 取交集
set1.intersect(set4);
// => Set { 2, 3 }

// 6. 取差集
set1.subtract(set4);
// => Set { 1 }
```

# 路由 react-router

路由(routing)库, 通过 URL 的变化来管理对应的状态和组件.

传统的 Web 开发主要是由服务器管理 URL Routing 和渲染 HTML 页面，过往每次 URL 一换, 就需要重新 server 端重新载入页面。现在进化到由前端负责 URL 的 routing 管理. 在 React 开发世界中主流是使用 react-router

最新版: React Router4(https://github.com/ReactTraining/react-router), 2.x 本版是 static route, 4.x 版本是 dynamic route

React Router 被拆分成三个包：react-router(最小核心, 包含进后面两个了),react-router-dom(web)和 react-router-native(app)。react-router 提供核心的路由组件与函数。其余两个则提供运行环境（即浏览器与 react-native）所需的特定组件

## 路由标签的使用

### Router-路由容器

有几个实现:

-   <BrowserRouter>：使用 HTML5 提供的 history API 来保持 UI 和 URL 的同步；需要 server 端的配合

-   <HashRouter>：使用 URL 的 hash, 带有 "#"的 url (例如：window.location.hash); 用于支持低版本的浏览器

-   <MemoryRouter>：能在内存保存你 “URL” 的历史纪录(并没有对地址栏读写)；页面变动不会反映到浏览器地址栏

-   <NativeRouter>：为使用 React Native 提供路由支持；

-   <StaticRouter>：从不会改变地址；

### Route-路由组件映射

负责 URL 和对应的组件映射，可以有多个 Route 规则也可以有嵌套（nested）Routing

#### 属性

-   path（string）: 路由匹配路径, 支持 regex(path="/order/:direction(asc|desc)")。

    -   没有 path 属性的 Route 总是会 匹配(渲染)）；

    -   有多个 path 匹配当前 url, 则对应的多个 component 都会显示 ("多匹配")

-   component (表达式): 对应的组件

- render: 函数, 接受一个 props, 返回 component

-   exact（bool）：出现时，则要求路径与 location.pathname 必须完全匹配, 对应组件才会显示

-   strict（bool）：true 的时候，有结尾斜线的 path 路径只能匹配有斜线的 location.pathname；

-   location: object `<Route>`元素尝试将其路径与当前 history location（通常是当前浏览器 URL）相匹配。但是，也可以人为指定特定的 location 进行匹配

    > Switch 中的 location 会覆盖 Route 中的 location

-   sensitive: bool 路径大小写敏感

#### 通过 url 传递参数给 Route

传递: `<Route path="/topic/:id"></Route>`

获取: `this.props.match.params`

能够放入 url 参数中的 state 信息尽量放 url 参数中

```js
import React from "react";
import { BrowserRouter as Router, Route, Link } from "react-router-dom";

const Topic = ({ match }) => <h1>Topic {match.params.id}</h1>;

export default class RouterParams extends React.PureComponent {
    render() {
        return (
            <Router>
                <div>
                    <ul id="menu">
                        <li>
                            <Link to="/topic/1">Topic 1</Link>
                        </li>
                        <li>
                            <Link to="/topic/2">Topic 2</Link>
                        </li>
                        <li>
                            <Link to="/topic/3">Topic 3</Link>
                        </li>
                    </ul>

                    <div id="page-container">
                        <Route path="/topic/:id" component={Topic} />
                    </div>
                </div>
            </Router>
        );
    }
}
```

#### 嵌套路由

```js
import React from "react";
import { BrowserRouter as Router, Route, Link } from "react-router-dom";

const SonCategory = ({ match }) => <h1>Sub Category {match.params.subId}</h1>;

const SubCategory = ({ match }) => (
    <div>
        <h1>Category {match.params.id}</h1>

        <ul id="menu">
            <li>
                <Link to={`/category/${match.params.id}/sub/1`}>
                    Sub Category 1
                </Link>
            </li>
            <li>
                <Link to={`/category/${match.params.id}/sub/2`}>
                    Sub Category 2
                </Link>
            </li>
            <li>
                <Link to={`/category/${match.params.id}/sub/3`}>
                    Sub Category 3
                </Link>
            </li>
        </ul>
        <div id="page-container-2">
            <Route path="/category/:id/sub/:subId" component={SonCategory} />
        </div>
    </div>
);

export default class NestedRoute extends React.PureComponent {
    render() {
        return (
            <Router>
                <div>
                    <ul id="menu">
                        <li>
                            <Link to="/category/1">Category 1</Link>
                        </li>
                        <li>
                            <Link to="/category/2">Category 2</Link>
                        </li>
                        <li>
                            <Link to="/category/3">Category 3</Link>
                        </li>
                    </ul>

                    <div id="page-container">
                        <Route path="/category/:id" component={SubCategory} />
                    </div>
                </div>
            </Router>
        );
    }
}
```

#### Route 三种渲染方式

Route 三种渲染内容的方法, 不可同时使用, 均会设置 match, location, history 在 props 中

-   `<Route component=xxx类>`：当 location 匹配到 path 时候, 会 render，route props 也会随着一起被渲染；

    > 不要提供 inline function, 因为如果这样, 每次 render, 都会创建一个新的 component(the router uses React.createElement to create a new React element from the given component. That means if you provide an inline function to the component prop, you would create a new component every render.)

-   `<Route render=xxx函数>`：这种方式对于内联渲染和包装 Route 却不引起意料之外的重新挂载特别方便；

    > Route component 的优先级要比 Route render 高

-   `<Route children=xxx函数>`：与 render 属性的工作方式基本一样，除了 `它是不管地址匹配与否都会被调用`；

    当没有匹配上, match 为 null, 可以借助这一特性, 更具是否匹配上了设置动态的样式

### Switch-解决多匹配问题

内部包含的 Router/Redirect 匹配第一个就不再继续往下匹配了

-   location: object 可以认为的指定 location 来决定哪个 child 被 render, 一般不指定, 根据浏览器 location 决定是哪个 child 被 render

    > 这里的 location 会覆盖 Route 中的 location

### Redirect-重定向当前页面

典型场景: 登陆判断

-   to 重定向路径

### Link-超链接

不会触浏览器刷新

-   to（string/object）：要跳转的路径或地址；

-   replace（bool）：为 true 时，点击链接后将使用新地址替换掉访问历史记录里面的原地址；为 false(默认) 时，点击链接后将在原有访问历史记录的基础上添加一个新的纪录

### NavLink: 带样式的 Link

会添加选中状态的样式

-   activeClassName: string 激活时候的 className, 默认值 active(外部引入 css 文件的时候貌似不好使, 存疑)

-   activeStyle: object 通过 inline 方式引入样式

-   exact: bool 精确匹配模式, strict: bool url 末尾需要"/", 比如 to="/events/"

-   isActive: func(match, location) 自定义函数判断是否激活状态

-   location: object 可以设定一个 location 对象

### Prompt-提示

满足条件时会提示用户是否离开当前页面

-   when 条件逻辑
-   message 提示信息

## history

history is mutable， 因此推荐这样获取 location: this.props.location

```js
class Comp extends React.Component {
    componentWillReceiveProps(nextProps) {
        // will be true
        const locationChanged = nextProps.location !== this.props.location;

        // INCORRECT, will *always* be false because history is mutable.
        const locationChanged =
            nextProps.history.location !== this.props.history.location;
    }
}
```

## location

location 长这样

```js
{
  key: 'ac3df4', // not with HashHistory!
  pathname: '/somewhere'
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    xxx: xxx
  }
}
```

router 将在这些地方提供 location 对象:

-   `Route component` as `this.props.location`

-   `Route render` as `({ location }) => ()`

-   `Route children` as `({ location }) => ()`

-   `withRouter` as `this.props.location`

A location object is never mutated , 所以可以通过它在 lifecycle hooks 中来决定何时怎么导航(获取数据和动画过渡特别有用)

```js
componentWillReceiveProps(nextProps) {
  if (nextProps.location !== this.props.location) {
    // navigated!
  }
}
```

这些地方可以设置 location 对象来代替 string 形式的 url patern:

-   Web `Link to`
-   Native Link to
-   `Redirect to`
-   history.push
-   history.replace

```js
const location = {
    pathname: "/somewhere",
    state: { fromDashboard: true }
};
```

## match

# 调试和测试

chrome 调试插件: React DevTool, Redux DevTool

格式化: Prettier (可以自动修正)

代码风格约束: ESLint

测试: Jest, Enzyme

mock 假数据, 模拟假的 api: json-server配合faker.js

# GraphQL 和 Relay

GraphQL 是一种数据查询语言（Data Query Language）, 主要是想解决传统 RESTful API 所遇到的一些问题

Relay 是搭配 GraphQL 用于 React 的一个宣告式数据框架，可以降低 Ajax 的请求数量

# CSS-Module 模块化

解决了 css 类名的冲突问题, https://zhuanlan.zhihu.com/p/20495964

业界另一种方案是 BEM 类命名方法 ([bem 命名](https://www.ibm.com/developerworks/cn/web/1512_chengfu_bem/index.html))

如果使用 webpack, css-loader 需要如此配置: `'css-loader?modules&localIdentName=[name]__[local]-[hash:base64:5]'`

可以和 `react-css-modules` 配合使用 ([ref](https://segmentfault.com/a/1190000004530909))

https://github.com/camsong/blog/issues/5

http://www.alloyteam.com/2017/03/getting-started-with-css-modules-and-react-in-practice/

css 模块化进化：【搜索 “css modules styled-components 区别”】 [1](https://www.zcfy.cc/article/css-evolution-from-css-sass-bem-css-modules-to-styled-components)，[2](http://hkongm.github.io/2018/07/25/ReactStyleTypes/)，[3](http://www.alloyteam.com/2017/05/guide-styled-components/)

在 create-react-app 中如何使用? -- css 文件命名为 `<componentName>.module.css` 即可使用 css modules 特性

# css-in-js

[emotion 和 styled-components 两种解决方案](https://zhuanlan.zhihu.com/p/91498498)

好处是能通过 js 动态生成 css

推荐 emotion, emotion 同时支持 css props 写法和 styled 写法, styled-components 只有 styled 写法

总体不如 css-module, 因为 css 和 js 耦合, 将来如果迁移, 不方便

另一种 css 解决方案: https://tailwindcss.com/

[react + emotion 的 styled 写法](https://codedaily.io/tutorials/62/Create-a-Hoverable-Display-Card-with-React-and-Emotion)

# 最佳实践 best practice

https://github.com/camsong/blog
https://github.com/camsong/blog/issues/6

技术环境：

打包 es6 + Webpack + less
使用了 HapiJs 作为模拟 server，前端开发和后端彻底分离，后期集成时，使用自己开发的代理服务器调用真正的 API
运行库 React + Flux + React-router+ material-ui + ImmutableJS + 自己研发的控件
单测环境 Karma+mocha+chai+Istanbul
布局完全用 flex

使用 Flex 布局会大大降低 CSS 的难度
使用 Flux 可以很好的隔离业务（我们的业务逻辑都在 Store 里）,大大降低了单元测试的难度
基于 React 的控件（这里说的是基本控件）是对 React 理解的一个体现
尽量把 React 组件（Component）做到小，做到细，也就是尽量拆分 React 组件
基于数据驱动的方式（Data-Driven）开发，理解到这一点就会发现 React 很神奇，可能这也是最不好理解的地方
ImmutableJS 带来了大量的好处，所有组件都使用了 ShouldComponentUpdate 来优化
实现了 I18N 的动态加载
使用了 Url 来保存 Router 信息，没有使用 Hash，感觉很简洁

# 服务端渲染

最新的方案: https://github.com/rendora/rendora    (https://segmentfault.com/a/1190000017361781?utm_source=tag-newest)

## 什么是 ssr

以前的多页面站点是在服务端渲染的，输出到浏览器时已经有了完整的骨架（ html ），这样爬虫抓到时可以分析页面标题，内容等等，做 seo 容易，那时的技术栈是 jsp，php 等等。

现在流行的前端框架在这方面是开了倒车，一个空的骨架，一个很大的 js，抓到浏览器以后才开始渲染完整的页面，爬虫就很为难了，因为它没法执行 js 啊，此外，白屏时间也是由于前端渲染带来的问题。于是又有了服务端渲染，让渲染这一步再回到服务端，在服务端模拟浏览器环境，给 react 和 vue 等框架一个可执行的上下文，就是为了提前得到完整的 html, 这就是 server side  render

Isomorphic JavaScript : 指前后端（Client/Server）共用相同部分的 js 代码, 让 JavaScript 应用可以同时执行在浏览器端和 server 端，在 React 中可以使用 server 端渲染（server side rendering）静态 HTML 的方式达到 Isomorphic JavaScript 效果，让 SEO 和执行效率更加提升并让前后端共用代码

## ssr 的好处

- seo 友好
- 同时有助于改善首屏加载体验


## 解决方案

https://www.zhihu.com/question/51949678 想要 seo 的页面要静态分开部署, 不要使用 react
https://blog.coding.net/blog/React-Server-Side-Rendering-for-SPA-SEO,
https://blog.bluedgeusa.com/how-to-build-an-seo-friendly-website-with-react-ef42bb52c563#.ge090p9pu //todo ,
https://github.com/carlleton/reactjs101/blob/zh-CN/Ch10/react-redux-server-rendering-isomorphic-javascript.md
https://www.jianshu.com/p/56ca46c90ee2 使用类库


其实, 对于真正适合做成 SPA 的应用，SEO 反而通常不是问题。针对 marketing 的页面应该是静态分开部署的，app 本身则要登陆才能用，SEO 没有什么意义。对于少数同时需要 seo, 而且同时需要强交互性 spa 的场景, 有如下几种解决方案:

-   React 使用 Next.js 作服务器端渲染 https://nextjs.org/

-   使用 Node.js 作为前后端的中间层进行中间层渲染 ，但可能对项目改动有点大 (http://www.alloyteam.com/2015/10/8783/), 参考 https://juejin.im/post/6844903693033144333

    React 提供了俩个神奇的方法，renderToString 和 renderToStaticMarkup:

    renderToString 方法，只应用在服务器端，把一个 React Component 渲染成 HTMl ,可以将它通过 response 发送到浏览器端，达到 SEO 的目的。

    renderToStaticMarkup 方法，和 renderToString 类似，但是它生成的 HTML Dom 不包含类似 data-react-id 这样的额外属性，可以用于生成简单的静态页面，节省大量字符串。

- https://github.com/rendora/rendora 使用 headless Chrome

- https://github.com/jaredpalmer/razzle

### 配合 koa2 实现 ssr

https://github.com/yangfan0095/react-koa2-ssr


### egg 实现 ssr 

https://github.com/ykfe/egg-react-ssr 热度更高


# 性能优化

https://zhuanlan.zhihu.com/p/37148975

## 常见的性能问题场景

键盘输入卡顿

鼠标移动/滚动卡顿 (调整尺寸 resize)

组件拆分的要足够小, 减少 virtual dom 的 diff 操作负担

## 使用工具定位性能问题

react devtools (highlight update 功能)
chrome devtools

## 网络性能优化-自动按需加载

可以通过 react-loadable 组件 + webpack import api 来实现 react 异步加载

```js
import loadable from "react-loadable";

const TargetComponent = loadable({
    loader: () => import("./TargetComponent"),
    // 等待页面
    loading: () => <div>Loading....</div>
});
```

## 使用 Reselect 避免重复计算

reselect 库用来创建自动缓存的数据的处理函数

# react 动画

https://mp.weixin.qq.com/s/Ey24FrWN2SiLK2LAmPv7hA

TODO

# react native

https://reactnative.dev/docs/getting-started 官网
https://github.com/reactnativecn/react-native-guide 中文资源

美团外卖的实践: https://tech.meituan.com/2019/12/19/meituan-mrn-practice.html

更推荐 flutter

https://guoshuyu.cn/home/wx/
http://laomengit.com/guide/introduction/cross-platform.html

https://xiaozhuanlan.com/topic/8924513706 跨平台开发原理
https://exp.newsmth.net/topic/article/51b2b5d2371fc166e46ef0ff881b8506 flutter 对比
https://zhuanlan.zhihu.com/p/43957699

# 开发小程序

remax, taro, anujs, 推荐 Taro + typescript


https://zhuanlan.zhihu.com/p/24946190 申请步骤

https://github.com/didi/chameleon


# 小项目

## 实现 route

https://www.cnblogs.com/huyue/p/9463328.html
https://www.jianshu.com/p/014fd226352b


## 表单设计器


https://github.com/alibaba/form-render
https://github.com/rjsf-team/react-jsonschema-form json 渲染表单

## cms 系统 

(配合 antd)

```sh
npx create-react-app cms-article

# 样式 主题 支持 less 
# 自定义配置
npm i -S antd craco-less @craco/craco

# 路由
npm i -S react-router-dom

# ajax
npm i -S axios

# redux
npm i -S redux react-redux
```

# 部署到 github 上

```sh
# 在package.json配置文件中加一句, 处理跨域问题
#“homepage”: “https://fyxwanan.github.io/test” 

# or

# "proxy": {
#     "/api": {
#       "target": "http://localhost:8080",
#       "ws": true
#     }
#   }

# 生成一个build文件夹
npm run build

# 提交 master 代码
git push origin master

# build文件夹上传到另一个分支
git subtree push --prefix=build origin gh-pages

# 最后到 github 配置 github pages

```

# 和 springboot 打包单体应用

https://cloud.tencent.com/developer/news/603125

https://medium.com/@itzgeoff/including-react-in-your-spring-boot-maven-build-ae3b8f8826e
frontend-maven-plugin

https://blog.csdn.net/andy_zhang2007/article/details/89393005 和 webjars

# 和 typescript 一起使用


## ts 基本使用

https://github.com/xcatliu/typescript-tutorial

`npm install -g typescript` , https://www.typescriptlang.org/play/

### 枚举

```ts
const enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];
console.log(colorName);  // 显示'Green'因为上面代码里它的值是2

// 枚举相当于一个对象
// obj: { X: number }

// 枚举作为类型:
enum ShapeKind {
    Circle,
    Square,
}
interface Circle {
    kind: ShapeKind.Circle;
    radius: number;
}
interface Square {
    kind: ShapeKind.Square;
    sideLength: number;
}
let c: Circle = {
    kind: ShapeKind.Square,
    //    ~~~~~~~~~~~~~~~~ Error!
    radius: 100,
}

```

### 使用 interface 定义对象类型

```js

// similar to bean in java
interface SquareConfig {
  // optional properties, 
  color?: string;
  width?: number;
  readonly xxx: number;// readonly property, 
  //最简单判断该用readonly还是const的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用 const，若做为属性则使用readonly
}

//ReadonlyArray<T>类型，它与Array<T>相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改：
let ro: ReadonlyArray<number> = a;
const a = [];
a = ro; // error!
a = ro as number[];//ok



// 额外的属性检查
interface SquareConfig {
    color?: string;
    width?: number;
}
function createSquare(config: SquareConfig) {
}
let mySquare = createSquare({ colour: "red", width: 100 });//error, 参数 interface 中没有 colour, 即使 color 是 可选参数也不行
let mySquare = createSquare({  width: 100 });// ok
let mySquare1 = createSquare({ width: 100, opacity: 0.5 } );//error
let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);//ok 
// 若希望参数中带有其他属性, 最好的解决方式:
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;//可以有任意数量的属性，并且只要它们不是color和width，那么就无所谓它们的类型是什么
}

//还有一种方法: 将可能有错的参数对象赋值给一个另一个变量, 然后传入方法中
let squareOptions = { colour: "red", width: 100 };
let mySquare = createSquare(squareOptions);

```

### interface 定义函数类型

```js
interface SearchFunc {
  (source: string, subString?: string): boolean;// 可选参数必须在最后
  // 可以不止有一个函数签名
  
}
let mySearch: SearchFunc;
mySearch = function(src, sub) {// 返回值类型, 参数类型可省略
    let result = src.search(sub);
    return result > -1;
}


// 混合类型
//一个对象可以同时做为函数和对象使用，并带有额外的属性
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}
function getCounter(): Counter {
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}
let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

### interface 定义可索引类型

类似数组, map...

```js
interface StringArray {
  [index: number]: string;// 索引支持字符串和数字。可以同时使用, 但是数字索引的返回必须是字符串索引的返回值的子类型
}


  // 底层实际只支持 字符串作为索引, 若是数字作为索引, 实际还是先转为字符串, 然后去查找, 所以为了能够查到, number 对应的返回类型必须和 string 对应的返回值类型相同 or 为子类型
//
class Animal {
    name: string;
}
class Dog extends Animal {
    breed: string;
}
// 错误：使用数值型的字符串索引，有时会得到完全不同的Animal!
interface NotOkay {
    [x: number]: Animal;
    [x: string]: Dog;
}


// 一旦指定了索引类型, 当前对象的其他索性返回类型必须符合这个索引类型
interface NumberDictionary {
  [index: string]: number;
  length: number;    // 可以，length是number类型
  name: string       // 错误，`name`的类型与索引类型返回值的类型不匹配
}

//可以将索引签名设置为只读，这样就防止了给索引赋值, 也就阻止了数组扩展
interface ReadonlyStringArray {
    readonly [index: number]: string;
}
let myArray: ReadonlyStringArray = ["Alice", "Bob"];
myArray[2] = "Mallory"; // error! 不能设置myArray[2]，因为索引签名是只读的
```

### interface 定义 class 类

```js
interface ClockInterface {
    currentTime: Date;// 将作为子类成员变量, 子类必须有
    setTime(d: Date);
}

class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
```

### interface 继承

interface 可以继承 多个其他 interface, 逗号分离

亦可继承其他 class: (interface 自动拥有 class 的属性, 即使是 private)

```js
class Control {
    private state: any;
}

interface SelectableControl extends Control {// 拥有了 state 
    select(): void;
}

// 可以实现 interface, 因为 button 拥有 state 
class Button extends Control implements SelectableControl {
    select() { }
}
// ok
class TextBox extends Control {
    select() { }
}

// 错误：“Image”类型缺少“state”属性。
class Image implements SelectableControl {
    select() { }
}


```

### class 使用

```js
class Greeter {
  // 比较两个 object 是否相等, 
  //只有实例的成员会被比较。 静态成员和构造函数不在比较的范围内。
  // 如果属性不带修饰符, 只需要各个属性类型都兼容相等, 则 object 相等
  // 若属性带有修饰符(private或 protected), 则两个 object 中的 field 必须都有相同的修饰符, 且两个 field 声明自同一个地方(比如通过继承得到的 field)

  public name: string;
         greeting: string;  //// 默认 public
  protected name: string;//protected成员在派生类中仍然可以访问; 构造函数也可以被标记成 protected。 这意味着这个类不能在包含它的类外被实例化，但是能被继承, 可以在子类中调用 super()
  private name: string; //无法在类外部访问
  readonly xiaoyu: string = '';// 只读属性必须在声明时或构造函数里被初始化
      
  static origin = {x: 0, y: 0};// 静态属性, 通过类名访问 


  // 参数属性: 通过给构造函数参数前面添加一个访问限定符来声明。 使用 private限定一个参数属性会声明并初始化一个私有成员；对于 public和 protected来说也是一样
  constructor(message: string, readonly xiaoyu: string = 'XIAOYU') {// 这里是为同名参数 xiaoyu 初始化, 可以省掉一些代码 
    // 若有继承, 在构造函数里访问 this的属性之前，我们 一定要调用 super()
      this.greeting = message;
  }

  // getter, setter
  private _fullName: string;

  get fullName(): string {
      return this._fullName;
  }

  set fullName(newName: string) {
      if (passcode && passcode == "secret passcode") {  
          this._fullName = newName;
      }
      else {
          console.log("Error: Unauthorized update of employee!");
      }
  }
}

// 使用 getter, setter
let employee = new Employee();
employee.fullName = "Bob Smith";//setter
if (employee.fullName) {//getter
    alert(employee.fullName);
}
```

### 泛型使用

```js
function identity<T>(arg: T): T {
    return arg;
}
let myIdentity: <U>(arg: U) => U = identity;
let myIdentity: {<T>(arg: T): T} = identity;// 相当于定义了一个匿名 interface/object 来定义函数类型


let output = identity<string>("myString");
let output = identity("myString");// 简写


//泛型约束
function create<T>(c: {new(): T; }): T {
    return new c();
}
function createInstance<A extends Animal>(c: new () => A): A {
    return new c();
}
```

### 交叉类型

```js
// 通过泛型定义混合额类型
function extend<T, U>(first: T, second: U): T & U {
    let result = <T & U>{};
    for (let id in first) {
        (<any>result)[id] = (<any>first)[id];
    }
    for (let id in second) {
        if (!result.hasOwnProperty(id)) {
            (<any>result)[id] = (<any>second)[id];
        }
    }
    return result;
}

class Person {
    constructor(public name: string) { }
}
interface Loggable {
    log(): void;
}
class ConsoleLogger implements Loggable {
    log() {
        // ...
    }
}
var jim = extend(new Person("Jim"), new ConsoleLogger());
var n = jim.name;
jim.log();
```

### 联合类型

```ts
function padLeft(value: string, padding: string | number) {// padding can be string | number type
    // ...
}
let indentedString = padLeft("Hello world", true); // errors during compilation


//如果一个值是联合类型，只能访问联合类型里的公共部分
interface Bird {
    fly();
    layEggs();
}
interface Fish {
    swim();
    layEggs();
}
function getSmallPet(): Fish | Bird {
    // ...
}
let pet = getSmallPet();
pet.layEggs(); // okay
pet.swim();    // errors

////////////////////////////////////////
// 类型断言
let pet = getSmallPet();
if ((<Fish>pet).swim) { // 显式指定一个类型
    (<Fish>pet).swim();
} else {
    (<Bird>pet).fly();
}

// 简化:
// 将断言抽出来成为函数
function isFish(pet: Fish | Bird): pet is Fish {// 返回值为类型谓词, 语法为 param is Type
    return (<Fish>pet).swim !== undefined;
}
if (isFish(pet)) {
    pet.swim();
}
else {
    pet.fly();
}


// 感叹号 (!): 手动去除null或 undefined的联合类型
function fixed(name: string | null): string {
  function postfix(epithet: string) {
    return name!.charAt(0) + '.  the ' + epithet; // ok, 通过 ! 去掉了 null 类型
  }
  name = name || "Bob";
  return postfix("great");
}


// 可辨识联合
interface Square {
    kind: "square";
    size: number;
}
interface Rectangle {
    kind: "rectangle";
    width: number;
    height: number;
}
interface Circle {
    kind: "circle";
    radius: number;
}
type Shape = Square | Rectangle | Circle;
function area(s: Shape) {
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.height * s.width;
        case "circle": return Math.PI * s.radius ** 2;
    }
}
```

### 类型保护

```js
// 对于类型
function padLeft(value: string, padding: string | number) {
    if (typeof padding === "number") {//必须是 "number"， "string"， "boolean"或 "symbol"
        return Array(padding + 1).join(" ") + value;
    }
    if (typeof padding === "string") {
        return padding + value;
    }
    throw new Error(`Expected string or number, got '${padding}'.`);
}

// 对于实例
// 类型为SpaceRepeatingPadder | StringPadder
let padder: Padder = getRandomPadder();
if (padder instanceof SpaceRepeatingPadder) { // 右侧必须为构造函数
    padder; // 类型细化为'SpaceRepeatingPadder'
}
if (padder instanceof StringPadder) {
    padder; // 类型细化为'StringPadder'
}
```

### 类型别名

```ts
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
type Tree<T> = {
    value: T;
    left: Tree<T>;
    right: Tree<T>;
}

type LinkedList<T> = T & { next: LinkedList<T> };
interface Person {
    name: string;
}
var people: LinkedList<Person>;
var s = people.name;
var s = people.next.name;
var s = people.next.next.name;
var s = people.next.next.next.name;


// 和接口的区别
type Alias = { num: number }//类型别名并不创建新名字
interface Interface {
    num: number;
}
declare function aliased(arg: Alias): Alias;//将鼠标悬停在 interfaced上，显示它返回的是 Interface，但悬停在 aliased上时，显示的却是对象字面量类型
declare function interfaced(arg: Interface): Interface;

//类型别名不能被 extends和 implements
```

### 空类型

--strictNullChecks标记 效果:

```ts
let s = "foo";
s = null; // 错误, 'null'不能赋值给'string', 若果是 js 规则/ts 不加 strict 标记, 则可以

// TypeScript会把 null和 undefined区别对待。 string | null， string | undefined和 string | undefined | null是不同的类型
let sn: string | null = "bar";
sn = null; // 可以

sn = undefined; // error, 'undefined'不能赋值给'string | null'



//可选参数会被自动地加上 | undefined
function f(x: number, y?: number) {
    return x + (y || 0);
}
f(1, 2);
f(1);
f(1, undefined);
f(1, null); // error, 'null' is not assignable to 'number | undefined'
```

## 集成 ts 环境

https://create-react-app.dev/docs/adding-typescript/

```sh
npx create-react-app my-app --template typescript

# default use yarn to manage package, specify use npm
npx create-react-app my-app --use-npm


```

全局声明文件  `react-app-env.d.ts`, ts 配置 `tsconfig.js` 见 https://blog.csdn.net/qq_41831345/article/details/106843875

## 为样式文件编写 types

https://blog.csdn.net/Dilomen/article/details/100837293
https://segmentfault.com/q/1010000017979602/a-1020000017980147


## 配置 css 跳转

https://www.jianshu.com/p/641bcac7754b

# 使用 debugger for  Chrome

.vscode 配置文件中的专有变量:

```
${workspaceFolder} :表示当前workspace文件夹路径，就是根目录, .vscode 所在目录

${workspaceRootFolderName}:表示workspace的文件夹名，也即Test

${file}:文件自身的绝对路径，也即/home/Coding/Test/.vscode/tasks.json

${relativeFile}:文件在workspace中的路径，也即.vscode/tasks.json

${fileBasenameNoExtension}:当前文件的文件名，不带后缀，也即tasks

${fileBasename}:当前文件的文件名，tasks.json

${fileDirname}:文件所在的文件夹路径，也即/home/Coding/Test/.vscode

${fileExtname}:当前文件的后缀，也即.json

${lineNumber}:当前文件光标所在的行号

${env:PATH}:系统中的环境变量
```
