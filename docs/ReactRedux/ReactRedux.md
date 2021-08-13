# React-Redux 版本

> 它前面的步骤和基础版差不多,也得创建仓库和模板,它的作用就是把一些需要处理公共数据的逻辑放到组件的外面,这些组件就变成 UI 组件

## 安装 Redux

```javascript
npm i redux -S
```

## 安装 React-redux

```javascript
npm i react-redux
```

## 创建仓库

- 在 src 目录下创建一个 store 文件夹,里面创建一个 index.js 文件

```javascript
import { createStore } from "redux";
// 引入reducer
import reducer from "./reducer";
// 放入reducer
const store = createStore(reducer);
export default store;
```

## 创建 reducer 文件(处理工厂)

- 处理工厂

```javascript
import { CHANGE_VALUE } from "./actiontypes";

const defaultState = {
  inputValue: "123",
  list: [
    "Racing car sprays burning fuel into crowd.",
    "Japanese princess to wed commoner.",
    "Australian walks 100km after outback crash.",
    "Man charged over missing wedding girl.",
    "Los Angeles battles huge wildfires.",
  ],
};
export default (state = defaultState, action) => {
  //这里判断action的type然后在返回state
  if (action.type === CHANGE_VALUE) {
    let newresult = JSON.parse(JSON.stringify(state)); //必须要重新生成一个新的对象,也不能使用Object.asign这样有的时候不起作用,而且每个判断里面必须有返回值
    newresult.inputValue = action.value;
    return newresult;
  }
  if (action.type === CLICK_CHANGE) {
    let newState = JSON.parse(JSON.stringify(state));
    if (newData.inputValue != null) {
      newData.list.push(newData.inputValue);
      newData.inputValue = "";
    }
    return newData;
  }
  // 这里就是删除方法,必须返回新数据
  if (action.type === DELETE_ITEM) {
    let newState = JSON.parse(JSON.stringify(state));
    newData.list.splice(action.value, 1);
    return newData;
  }
  return state;
};
```

## 调试的时候借助工具

```javascript
import { createStore } from "redux";
//引入reducer
import reducer from "./reducer";
//放入reducer
const store = createStore(
  reducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);
export default store;
```

## 货物类型和发射货物 actiontypes 和 actionCreaters.js

- actiontypes.js

```javascript
const CHANGE_VALUE = "change_value";
```

- actionCreaters.js

```javascript
import { CHANGE_VALUE } from "./actiontypes";

export const ACTION_INIT_LIST = (value) => {
  return {
    type: CHANGE_VALUE,
    value,
  };
};
```

## 在 index.js 第一个渲染的时候提供一个壳子

- 让所有的组件都使用 store ,必须使用 Provider

```javascript
import React from "react";
import ReactDOM from "react-dom";
import TodoList from "./Todolist";
//引入react-redux 提供的一个标签
import { Provider } from "react-redux";
import store from "./store/index";
const App = (
  // {Todolist组件还有下面的组件都可以使用store}
  <Provider store={store}>
    <TodoList />
    {/* {类似a组件之类的} */}
    {/* <a></a> */}
  </Provider>
);
ReactDOM.render(App, document.getElementById("root"));
```

## 组件里面使用

> 这样就能把操纵公共数据的方法都拿出来了

- connect

- 两个函数 第一个 state(里面传 state),第二个是方法(里面传 dispatch)

```javascript
import React, { Component } from "react";
import { connect } from "react-redux";
import { ACTION_INIT_LIST } from "./store/actionCreaters";
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "首页",
      inputContent: "显示的",
    };
    this.handleinputChange = this.handleinputChange.bind(this);
    this.handleChangeResult = this.handleChangeResult.bind(this);
  }
  render() {
    return (
      <div>
        <span>{this.state.message}</span>
        <input
          type="text"
          value={this.state.inputContent}
          onChange={this.handleinputChange}
        ></input>
        <button onClick={this.handleChangeResult}>发送</button>
      </div>
    );
  }
  handleinputChange(e) {
    console.log(e.target.value);
    this.setState({
      inputContent: e.target.value,
    });
  }
  handleChangeResult() {
    let result = this.state.inputContent;
    this.props.handleChange(result);
  }
}

const mapState = (state) => {
  return {
    inputValue: state.inputValue,
  };
};
const mapMutations = (dispatch) => {
  return {
    handleChange(value) {
      let result = ACTION_INIT_LIST(value);
      dispatch(result);
    },
  };
};

export default connect(
  mapState, //这里面放的是数据
  mapMutations //里面放的是操作的数据的方法
)(App);
```

> 如果要是使用了路由可以换成

```javascript
export default connect(
  mapState, //这里面放的是数据
  mapMutations //里面放的是操作的数据的方法
)(withRouter(App));
```
