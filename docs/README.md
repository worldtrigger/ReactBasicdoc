# 介绍

## React 简介

1. 数据绑定的时候,大量操作真实 DOM，性能成本太高

2. 网站的数据流向太混乱,不好控制

> React 把用户界面抽象成一个个组件，如按钮组件 Button、对话框组件 Dialog、日期组件 Calendar。开发者通过组合这些组件，最终得到功能丰富、可交互的页面。通过引入 JSX 语法，复用组件变得非常容易，同时也能保证组件结构清晰。有了组件这层抽象，React 把代码和真实渲染目标隔离开来，除了可以在浏览器端渲染到 DOM 来开发网页外，还能用于开发原生移动应用

## React 特点

- 声明式设计 React 采用声明范式,可以轻松描述应用(自动 DOM 操作)

- 高效 React 通过 DOM 的模拟(虚拟 DOM),最大限度的减少与 DOM 的交互

- 灵活 React 可以与已知的库或者框架很好的配合

- (重点)JSX -JSX 是 javascript 的语法拓展

- (重点)组件 - 通过 React 构建组件, 使得代码更加容易得到复用,能够很好的应用在大项目的开发

- (重点)单向响应的数据流 - React 实现了单向相应的数据流,从而减少了重复代码,这也是它比传统的数据绑定更简单

> React 的精髓是函数式编程, React 的核心是组件,组件的设计目的是提高代码复用率,降低测试难度和代码复杂度

## React 操作原理

- 传统 DOM 更新

> 在传统页面的开发模式中,每次更新页面都需要手动更新 Dom

- 虚拟 DOM

> DOM 操作非常昂贵,前端性能开发中性能消耗最大的就是 DOM 操作,而这一部分会让整体项目的代码变得难以维护,React 把真实的 Dom 树转换成 JS 对象,也就是和虚拟 DOM 存在

## React 函数式编程

> 它的主要思想就是把运算过程尽量写成一系列嵌套的函数调用

函数的编程好处

1. 代码的简介开发
2. 方便的代码管理
3. 代码的热开发

## React JSX 语法的由来

> JSX 将 HTML 语法直接加入到 JS 代码中,通过翻译器转换到纯 JS 后，由浏览器执行。实际开发中 JSX 在产品打包阶段都已经编译成纯 JS，反而让代码更加直观易于维护

1. 编译过程由 Babel 的 JSX 编译器来实现
2. JSX 的官方定义是类 XML 语法的 ECMASCRIPT 扩展
3. 组件就是一个集合体,就是把 HTML,CSS,JS 放在一起形成一个组件,组件的写法就是 jsx 语法的编写

## React 事件机制

- 事件必须要改变 this

- 函数声明需要与 render 函数同级

- 直接写事件机制 onClick = {this.函数名}

- 将函数变量封装到全局变量中

- 原型链写法

- 在 jsx 中写逻辑,必须要写在 render 和 return 之间

- 注释的标签写法 `{/* */}`

- 组件嵌套不能多个节点渲染,否则最后一个组件会覆盖前面的

## React 安装

### 安装(非 TS 版本)

- 如果已经安装了 create-react-app 那可以直接忽略第一步

```bash
npm install -g create-react-app
create-react-app 项目名称
```

### TS 版本

- 如果已经安装了 create-react-app 那可以直接忽略第一步

```bash
 npm install -g create-react-app
 npx create-react-app tsdemo1 --template typescript
```

- 特别注意的就是安装路由或者其他必须使用 typescript

```bash
cnpm i @types/react-router-dom -S
```
