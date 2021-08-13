# 父子组件传值

- 在 React 中父子传值一定会利用 props

> React 中 props 在 react 中父给子传递的都是属性,方法也可以传递,子属性要是想改变父组件的值 只有调用父组件的方法,而不是像 vue 那样发射出去事件让父组件监听

- 父组件

```javascript
import React, { Component, Fragment } from "react";
import "./Todolist.css";
import TodoItem from "./Todoitem.js"; // 引入子组件
class Todolist extends Component {
  constructor(props) {
    super(props);
    this.state = {
      value: "请输入内容",
      list: [],
    };
  }
  render() {
    return (
      <Fragment>
        <input
          type="text"
          value={this.state.value}
          onChange={this.changeall.bind(this)}
        />
        <button className="btn" onClick={this.dianji.bind(this)}>
          请点击
        </button>
        <ul>
          {this.state.list.map((value, index) => {
            return (
              <TodoItem
                data={value}
                index={index}
                deleteItem={this.deleteDate.bind(this)} //props绑定事件,必须把bind this传进来
              />
            );
          })}
        </ul>
      </Fragment>
    );
  }
  dianji() {
    let result = [...this.state.list, this.state.value];
    this.setState({
      list: result,
      value: "",
    });
  }
  changeall(e) {
    let value = e.target.value;
    this.setState({
      value,
    });
  }
  deleteDate(index) {
    let result = [...this.state.list];
    result.splice(index, 1); //删除数组制定项
    this.setState({
      list: result,
    });
  }
}

export default Todolist;
```

- 子组件

```javascript
import React, { Component, Fragment } from "react";
class Todoitem extends Component {
  constructor(props) {
    super(props);
    this.deleteData = this.deleteData.bind(this);
  }
  render() {
    return (
      <Fragment>
        <li onClick={this.deleteData}>{this.props.data}</li>
      </Fragment>
    );
  }
  deleteData() {
    let index = this.props.index;
    this.props.deleteItem(index);
  }
}

export default Todoitem;
```

## 总结

1. 利用 props 来进行给子组件传值
