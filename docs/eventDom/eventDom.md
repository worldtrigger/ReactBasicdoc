# 事件处理

- React 事件的命名采用小驼峰法则,而不是纯小写

- React 事件里面必须重新绑定 this

- 使用 JSX 语法时候你需要传一个函数作为事件处理函数,而不是一个字符串

## 事件的小驼峰法则

- 例如传统的 HTML

```html
<button onclick="activateLasers()">按钮</button>
```

- 而在 React 则不同

```javascript
<button onClick={this.activateLasers}> 按钮</button>
```

- 在 React 中你不能通过返回 false 来阻止默认行为,你必须使用 e.preventDefault()来阻止

```javascript
function handleClick(e)
{
  e.preventDefault();
  console.log("The Link was clicked")
}

render(){
  return (
    <a href="#" onClick="handleClick">
      Click me
    ></a>
  )
}
```

## 重新绑定 this

> 事件使用之前必须要绑定 this,要不然方法里面的 this 就是 undefined,绑定 this 写在构造函数里

```javascript
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      flag: true
    };
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    this.setState({
      flag:true
    })
  }
  render(){
    return {
      <button onClick={this.handleClick}>
          点击我
      </button>
    }
  }
}
ReactDOM.render(
  <Toggle/>,
  document.getElementById("root")
)
```

## 向事件里面传递参数

```javascript

class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      flag: true
    };
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick(str) {
    console.log(str);
    this.setState({
      flag:true
    })
  }
  render(){
    return {
      <button onClick={()=>{this.handleClick("123")}}>
          点击我
      </button>
    }
  }
}
ReactDOM.render(
  <Toggle/>,
  document.getElementById("root")
)
```

## 总结

1. 在 React 中 不能返回 false 来阻止默认行为,你必须使用 e.preventDefault()来阻止

2. 事件使用之前必须要绑定 this,要不然方法里面的 this 就是 undefined 绑定 this 写在构造函数里面

3.事件里传入参数必须是以 函数的形式
