# JSX 简介

- JSX 是一个 Javascript 的语法扩展,JSX 可能会让人联想到模板语言,但它具有 Javascript 的全部功能 JSX 可以生成 React 元素

```javascript
const element = <h1>Hello, world!</h1>;
```

# JSX 使用

## (1) 在 JSX 中嵌入表达式,大括号必须有

- 举个例子我们声明了一个 name 的变量,然后在 JSX 中使用它,并将它包裹在大括号中

```javascript
const name = "John Perez";

const element = <h1> hello,{name}</h1>;

ReactDOM.render(element, document.getElementById("root"));
```

> 在 JSX 语法中你可以在大括号中放置任何有效的 JS 表达式例如 2+2,firstName 或者 formatName(user) 都是有效的 JS 表达式

- 在下面的例子中我们将 formatName(user)，并将结果嵌套在 h1 元素中

```javascript

function formatName(user) {
  return user.firstname + " " + user.lastname;
}
const user = {
  firstname:"John",
  lastname:"smisth"
}
const element = {
  <h1>
    Hello,{formatName(user)}!
  </h1>
}
ReactDOM.render(
  element,
  document.getElementById("root")
)
```

> 在编译后 JSX 表达式会被转为普通的函数调用,并且会得到 JS 对象,也就是说里面必须有 return

## (2) JSX 可以嵌套语句

- 你也可以在 if 语句或者 for 循环的代码块中使用 JSX

```javascript
function getUser(user) {
  if (user) {
    return <h1>Hello,{formatName(user)}</h1>;
  } else {
    return <h1>Hello,String</h1>;
  }
}
```

## (3) JSX 特定属性

- 通过引号将属性值变成字符串

```javascript
const element = <div tabindex="0"></div>;
```

- 也可以使用大括号在属性中插入一个 JS 表达式

```javascript
const element = <img src={user.img}></img>;
```

> 千万记得大括号或者引号只能使用其中的一个,引号就表示字符串,大括号就表示变量,对同一属性就不能同时使用这两种符号

`JSX在这里使用的是小驼峰法命名,比如class变成了className,而tabindex变成了tabIndex`

## (4) 使用 JSX 指定子元素

- 假如一个标签里面没有内容,你可以使用/>来闭合标签,就像 xml 语法一样

```javascript
const element = <img src={user.imgurl} />;
```

- JSX 标签里包含很多子元素

```javascript
const element = {
  <div>
     <h1>Hello</h1>
     <h2>Good to see you </h2>
  </div>
}
```

## (5)JSX 防止注入攻击

> ReactDom 在渲染所有输入内容之前,默认就是进行转义,它可以确保在你的应用中,永远不会注入哪些并非自己明确写的内容,所有的内容在渲染之前都转换成了字符串,这样防止攻击

## (6)JSX 表示对象

- Babel 会把 JSX 转译成一个名为 React.createElement()函数调用

以下两种实例代码等效

```javascript
const element = {
  <h1 className="greeting">
     Hello,world!
   </h1>
}
```

等价于

```javascript
const element = React.createElement(
  "h1",
  { className: "greeting" },
  "Hello,world!"
);
```

- 所以 React.createElement()会预先执行一些检查,以帮助你编写无措代码,但实际上他创建了这样一个对象，这些对象被称为 React 元素,他们描述了你屏幕上看到的内容，然后使用它们来构建 DOM 以及保持更新

## 总结

1. JSX 语法里可以{}来表示变量, 表达式

2.引号和{}只能二选一,引号就表示字符串,大括号就表示变量或者表达式.要是表达式外面套了层引号就表示字符串

3.在 JSX 里面 class 必须写成 className,否则它不识别 class,而且命名采用小驼峰法则

4.JSX 里面的元素必须有个包裹层,而且需要闭合
