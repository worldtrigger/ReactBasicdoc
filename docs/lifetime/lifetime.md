# State&生命周期

## 创建一个 class 组件

- 创建一个同名的 ES6 class 并且继承于 React.Component

- 添加一个 render()方法

- 在 render 中使用 this.props 替换 props

```javascript
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello,world!</h1>
        <h2>It is {this.props.data.toLocaleTimeString()}</h2>
      </div>
    );
  }
}
```

> 每次组件更新的时候 render 方法都会被调用,但只要在相同的 DOM 节点中渲染仅有一个 Clock 组件的 class 实例被创建使用,这样就使我们可以用 state 或者生命周期方法等很多特性

## 向 class 组件中添加局部的 state

- 把 render()方法中的 this.props.data 替换成 this.state.date

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

- 添加一个 class 构造函数,然后在该函数中为 this.state 赋初值

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props); //通过props传递到父类的构造函数中
    this.state = { date: new Date() };
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
ReactDOM.render(<Clock />, document.getElementById("root"));
```

# 将生命周期方法添加到 Class 中

生命周期函数指的是在特定的时候才行运行，React 有八大钩子函数

- componentWillMount 组件被挂载的时候执行 render 之前

- componentDidMount 组件被挂载之后执行 render 之后,这里一般放请求数据的异步操作

- shouldComponentUpdate 无论是 props 还是 state，组件被更新前一定会执行它。它必须有返回值,它就是开关.false 就不更新了。它里面有两个参数,第一个是 nextProps,第二个是 nextState,nextProps 表示下一个要传递的父数据,nextState 表示下一个 State 改变。这样子组件里面的 render 就不会重新渲染

> shouldComponentUpdate 只是对本组件使用要是 false 对于 props 来说它仅仅只是渲染了 props 当父组件除了 props 以外的元素更新的话，它不会在更新.要是对于 state 来说,他不会渲染。所以价格判断比较好

```javascript

shouldComponentUpdate(nextProps,nextState) {
  if(nextProps.xxx!=this.props.xxx)
  {
       return true
  }else{
    return false
  }
}
```

- componentWillUpdate 组件被更新前,并且 shouldComponentUpdate 返回 true 以后

```javascript
//只有shouldComponentUpdate
// 组件被更新之前,并且shouldComponentUpdate返回true执行

componentWillUpdate(){
  console.log("组件更新之前")
}
```

- componentDidUpdate 组件更新之后

```javascript
componentDidUpdate(){
  console.log("组件更新之后")
}
```

- componentWillReceiveProps

> 当一个组件从父组件接收了参数,只要父组件的 render 函数被重新执行了(也就是大于第一次) 子组件这个生命周期函数就执行

- componentWillUnmount 组件被卸载

```javascript
componentWillUnmount(){
  console.log("组件被卸载");
}
```

- render 渲染节点里的函数,里面必须有 return

```javascript
render(){
  return (
       <h1>哈哈哈</h1>
  )
}
```

## 生命周期的函数执行顺序

```bash
//第一步  componentWillMount
//第二步  componentDidMount
//第三步  render
//第四步  shouldComponentUpdate
//第五步  componentWillUpdate
//第六步  componentDidUpdate
//第七步  componentWillReceiveProps
//第八步  componentWillUnmount
```

## 更新数据的时候 必须要用 this.setState()来更新组件

```javascript
class Clock extends React.Component {
  //构造函数
  constructor(props) {
    super(props);
    this.state = {
      date: new Date(),
    };
  }
  //更新数据 获取数据
  componentDidMount() {
    let _this = this;
    this.timeID = setInterval(function () {
      _this.tick();
    }, 1000);
  }
  //卸载组件的时候清除定时器
  componentWillUnmount() {
    clearInterval(this.timeID);
  }
  //核心函数,利用setState改变
  tick() {
    this.setState({
      date: new Date(),
    });
  }
}
```

## 正确的使用 setState

- 不要直接修改 State(错误案例)

```javascript
this.state.commet = "Hello";
```

- 正确使用

```javascript
this.setState({
  commet: "Hello",
});
```

- 当我们更新完 State,希望触发点动作,就需要一个回调函数,第二个参数表示回调函数

```javascript
this.setState(
  {
    foo: 123,
  },
  () => {
    console.log(foo);
  }
);
```

## 切记 数据是向下流动的

> 不管父组件还是子组件都无法知道某个组件是有状态还是无状态的

`子组件要是想改变父组件的值 必须调用父组件传递给子组件的方法`

组件可以选择它的 state 作为 props 向下传递到它的子组件中

```javascript
<h2>It is {this.state.data.toLocaleTimeString()}</h2>
```

- 对于自定义组件可以同样使用

```html
<FormatteDate date="{this.state.date}" />
```

## 总结

- 每一个组件里面必须要有构造函数 super(props)

- 生命周期函数

```bash
//第一步  componentWillMount
//第二步  componentDidMount
//第三步  render
//第四步  shouldComponentUpdate
//第五步  componentWillUpdate
//第六步  componentDidUpdate
//第七步  componentWillReceiveProps
//第八步  componentWillUnmount
```

- 数据流由父传子(单向)
