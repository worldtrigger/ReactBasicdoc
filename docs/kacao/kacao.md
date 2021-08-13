# 卡槽

- 不具名卡槽

- 具名卡槽

- 传值和卡槽

## 不具名卡槽

> 在 React 中要是想传递 html 必须借助 children,props 来渲染

- 例如父组件

```javascript
function FancyBorder(props) {
  return (
    <div className={"FancyBorder-" + props.color}>
      /*重点来了*/
      {props.children}
    </div>
  );
}
```

- 子组件

```javascript
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">welcome</h1>
      <p className="Dialog-message">Thank you for you visting!</p>
    </FancyBorder>
  );
}
```

> 这样的话 子组件通过传递 props.children 来渲染父组件传递的 html

## 带有名称的卡槽

有的时候我们需要传递的 html 不仅仅是一处,所以需要命名卡槽

> 通过 left,right 来传值

- 通过 props.left

- 通过 props.right

```javascript
function SolitPane(props){
   return (
     <div class="SplitPane">
      <div class="left">
          {props.left}
      </div>
         <div class="right">
        {props.right}
         </div>
     </div>
   )
}
function App(){
 return (
    <SolitPane
    left={<Contacts/>}
    right={<Chat/>}
    >
 )
}
```

## 卡槽和属性一起传递

```javascript
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">{props.title}</h1>
      <p className="Dialog-message">{props.message}</p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = { login: "" };
  }

  render() {
    return (
      <Dialog
        title="Mars Exploration Program"
        message="How should we refer to you?"
      >
        <input value={this.state.login} onChange={this.handleChange} />

        <button onClick={this.handleSignUp}>Sign Me Up!</button>
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({ login: e.target.value });
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }
}
```

## 总结

1.利用 props.children 来传递 html

2.利用属性名字 left={}来传递
