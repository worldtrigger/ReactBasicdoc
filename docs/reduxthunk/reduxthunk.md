# Redux -- Thunk

> 使用了 thunk 就是为了返回内容除了对象还可以是方法

## 安装 redux

```javascript
cnpm i redux -S
```

## 安装 redux-thunk 插件

```javascript
cnpm i redux-thunk -S
```

## 使用中间件改变仓库 Store 下面的 index.js

```javascript
import { createStore, applyMiddleware, compose } from "redux";
import thunk from "redux-thunk";
import reducer from "./reducer";
const middleware = [thunk];
const composeEnhancers =
  typeof window === "object" && window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__
    ? window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({})
    : compose;

const enhancer = composeEnhancers(
  applyMiddleware(...middleware)
  // other store enhancers if any
);
const store = createStore(reducer, enhancer);

export default store;
```

## 创建 reducer 文件(处理工厂)

- 必须有返回值

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
  return state;
};
```

## 创建 actionTypes 和 actionCreaters 文件

- actionTypes

```javascript
export const CHANGE_VALUE = "change_value";
export const CLICK_CHANGE = "click_change";
export const DELETE_ITEM = "delete_item";
```

- actionCreaters (使用了 thunk 所以能返回一个方法)

```javascript
import { CHANGE_VALUE } from "./actiontypes";
export const ACTION_INIT_LIST = (value) => {
  return {
    type: CHANGE_VALUE,
    value,
  };
};
//利用redux-thunk 返回方法
export const ACTION_METHOD_INIT = () => {
  return (dispatch) => {
    setTimeout(function () {
      let value = "异步改变的值"; //获取到数据
      let result = ACTION_INIT_LIST(value); //把数据封装到actions里面
      dispatch(result); //然后发射出去
    }, 3000);
  };
};
```

## 组件里面使用

- 和基础版使用一样

```javascript
import React, { Component } from "react";

//引入公共仓库
import Store from "./store/index";
//引入子弹
import { ACTION_METHOD_INIT } from "./store/actionCreaters";
class App extends Component {
  constructor(props) {
    super(props);
    console.log(Store.getState());
    const StoreData = Store.getState();
    this.state = {
      message: "首页",
      inputValue: StoreData.inputValue,
      showinputValue: "哈哈",
    };
    this.handleChange = this.handleChange.bind(this);
    this.handleClick = this.handleClick.bind(this);
    this.cancleSub = () => {};
  }
  componentDidMount() {
    //监听数据变化
    this.cancleSub = Store.subscribe(() => {
      this.setState(Store.getState());
    });
  }
  componentWillUnmount() {
    this.cancleSub();
  }
  render() {
    return (
      <div>
        <input type="text" onChange={this.handleChange}></input>
        <button onClick={this.handleClick}> 点击改变</button>
      </div>
    );
  }
  handleClick() {
    let action = ACTION_METHOD_INIT();
    console.log(action);
    Store.dispatch(action);
  }
  handleChange(e) {
    const result = e.target.value;
    this.setState({
      showinputValue: result,
    });
  }
}

export default App;
```

## 总结

1. 常用方法

2. store.dispatch(action) 发射出去

3. store.getState()获取 redux 数据

4. store.subscribe() 监听数据变化,里面必须传递一个函数()=>
