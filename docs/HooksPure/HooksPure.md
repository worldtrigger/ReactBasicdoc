# ReactHooks 之 PureComponent

> 当父元素的数据更新的时候,子元素不需要重新渲染,可现在的机制他被强制渲染了。针对函数可以 memo,针对类只有 PureComponent

## PureComponent

- 它主要用于类，当组件是一个类的时候,使用它可以做到不渲染

- 子组件这里必须要继承 React.PureComponent

```javascript
import React, { Fragment } from "react";
class SecondComponent extends React.PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      message: "子组件",
    };
  }
  render() {
    console.log("重新被渲染");
    return (
      <Fragment>
        <div>{this.state.message}</div>
      </Fragment>
    );
  }
}

export default SecondComponent;
```

- 父组件

```javascript
import React, { Component, Fragment } from "react";
import SecondFragment from "./Second.js";
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "首页",
      count: 0,
    };
    this.handleClick = this.handleClick.bind(this);
  }
  render() {
    return (
      <Fragment>
        <div>{this.state.message}</div>
        <div>{this.state.count}</div>
        <button onClick={this.handleClick}>点击</button>
        <SecondFragment time="22"></SecondFragment>
      </Fragment>
    );
  }
  handleClick() {
    let value = this.state.count;
    this.setState({
      count: value + 1,
    });
  }
}

export default App;
```
