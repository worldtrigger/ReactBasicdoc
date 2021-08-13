# React 之组件&Props

## 组件的核心

> 组件允许你将 UI 拆分成独立的可复用的代码片段,并且对每个片段进行独立的思考

`组件从概念上类似JShanshu,它接受任意的入参('props'),并返回用于描述页面展示内容的React元素`

## 函数组件与 Class 组件

组件的定义有两种方法

- 编写函数

- 编写类

### 编写函数

```javascript
function Welcome(props) {
  return <h1>{props.name}</h1>;
}
```

> 该函数是一个有效的 React 组件,因为它接受唯一带有数据的 props(代表属性)对象并且返回一个 React 元素,这些组件被称为'函数组件',因为它本身就是 JS 函数

### 编写类

- 通过 ES6 来定义组件

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello,{this.props.name}</h1>;
  }
}
```

## 渲染组件

- React 也可以让用户自定义组件

```javascript

function Welcome(props){
  return <h1>Hello,{props.name}</h1>
}

const element = <Welcome name="Sara">

ReactDom.render(element,document.getElementById('root'))

```

> 这段代码最后的结果就是 Sara

1. 调用 ReactDom。render()函数传入作为参数
2. React 调用 Welcome 组件并且将{name:'Sara'}作为 props 传入
3. Welcome 组件将 `<h1>Hello,Sara</h1>`作为返回值
4. ReactDom 将 DOM 高效的更新为 `<h1>Hello,Sata</h1>`

> 特别注意,组件名称必须以大写字母开头,React 会将小写字母开头的组件视作原生 DOM 标签,例如<div/> 代表 HTML 的 div 标签,而则代表了一个组件,并且在需要的作用域内使用 Welcome

## 组件组合

- 组件可以在其输出引用其他组件,这就可以让我们用同一组来抽象出任何层次的细节。按钮，表单，对话框.甚至整个屏幕的内容,在 React 应用程序中,这些都可以当做组件

- 例如我们可以多次渲染组件

```javascript
function Welcome(props) {
  return <h1>Hello,{props.name}</h1>;
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
ReactDOM.render(<App />, document.getElementById("root"));
```

## 提取组件

- 有的时候一个组件放不下那么多的逻辑代码所以需要分成几大块

## 全部组件

```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img
          className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">{props.author.name}</div>
      </div>
      <div className="Commet-text">{props.text}</div>
      <div className="Commet-date">{formatDate(props.date)}</div>
    </div>
  );
}
```

> 这个组件用于描述一个社交网站的评论功能,它接收 author(对象),text(字符串),以及 data(日期)作为 props,该组件由于嵌套关系变得难以维护,所以我们需要拆分出来

## 拆分开始

- (1) 拆分 Avatar 组件

Avatar 组件

```javascript
function Avatar(props) {
  return <img src={props.user.avatarUrl} alt={props.user.name} />;
}
```

- (2) 拆分 UserInfo 组件

```javascript
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">{props.user.name}</div>
    </div>
  );
}
```

- (3) 最后简化

```javascript
function Commet(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">{props.text}</div>
      <div className="Comment-date">{formatDate(props.date)}</div>
    </div>
  );
}
```

## Props 的可读性

`组件无论是使用函数声明还是通过class声明 都绝对不能修改自身的Props`

```javascript
function sum(a, b) {
  return a + b;
}
```

> 我们管这样的函数叫纯函数,因为该函数不会尝试更改入参,并且多次调用下相同的入参始终返回相同的结果

```javascript
function withdraw(account, amount) {
  account.total -= amount;
}
```

## 总结

1. props 里面的数据绝对不能更改
2. 组件的定义里面必须要有 return,要是一行就不用()要是多行就必须加()
3. 组件里面可以嵌套组件,没有个数限制。为了更好的使用，建议拆分组件
