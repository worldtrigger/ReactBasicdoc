# Redux 基础版

> Redux 类似 Vuex 就是存放公共数据的地方,方便取出数据

## Redux 流程和架构

### 架构(四个文件)

- store 就是仓库

- reducer (处理车间)这个是主要的文件 它里面写了逻辑操作

- actionTypes 发射出去的货物名称

- actionCreater 发送走货物(里面必须有返回值),返回值是一个对象,里面有两个属性(一个是 type,一个是 value) ,value 就是要修改的值。可以没有

### 流程很简单

用户操作后--->派发走 action(子弹)(里面必须有返回值)--->仓库收到后给 reducer(处理车间)-->(车间处理后改变仓库里面的值)--->前面的仓库监听仓库里面的值改变

## (1) Redux 安装

```javascript
cnpm i redux -S
```

## (2) 在 src 目录下创建一个 Store 文件夹,里面创建一个 index.js 文件作为公共仓库

- 公共模板

```javascript
import { createStore } from "redux";
// 引入reducer
import reducer from "./reducer";
// 放入reducer
const store = createStore(reducer);
export default store;
```

## (3) 然后在同级目录下创建 reducer 文件 (处理工厂)

- 必须有 return

- 无论 actiontype 是什么 必须有返回值

```javascript
import { CHANGE_VALUE, CLICK_CHANGE, DELETE_ITEM } from "./actiontypes";

// 可以理解为state里面的数据
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
//操作的方法
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

## (4) 在同级目录下 创建货物两个文件 actiontypes.js 和 actionCreater.js

- actiontypes.js 可以理解为货物名称

```javascript
export const CHANGE_VALUE = "change_value";
export const CLICK_CHANGE = "click_change";
export const DELETE_ITEM = "delete_item";
```

- actioncreater.js 发送走货物(必须是方法,里面返回一个对象)

```javascript
import { CHANGE_VALUE, CLICK_CHANGE, DELETE_ITEM } from "./actiontypes";

export const ACTION_CHANGE_VALUE = (value) => {
  return {
    type: CHANGE_VALUE,
    value,
  };
};
export const ACTION_CLICK_CHANGE = () => {
  return {
    type: CLICK_CHANGE,
  };
};
export const ACTION_DELETE_ITEM = (value) => {
  return {
    type: DELETE_ITEM,
    value,
  };
};
```

## (5) 调试的时候要借助 redux 工具

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

## (6) 组件里面发射的时候,dispatch

```javascript
//先引入子弹
import {
  ACTION_CHANGE_VALUE,
  ACTION_CLICK_CHANGE,
  ACTION_DELETE_ITEM
} from './store/actionCreater'

 <Button
  type="primary"
  style={{
    width: '150px',
    height: '30px',
    marginTop: '20px',
    marginLeft: '50px'
  }}
  onClick={this.handClick}
>
  请输入文字
</Button>

 // 发射出去子弹

 handClick() {
    let result = ACTION_CLICK_CHANGE()
    store.dispatch(result)
  }
```

## (7) 组件里面使用的时候

- subscribe 监听变化

- 在组件加载里面监听数据的改变

- 在组件卸载里面在执行一次空函数

```javascript
import React, { Component } from "react";

//引入公共仓库
import Store from "./store/index";
//引入子弹
import { ACTION_CHANGE_VALUE } from "./store/actionCreaters";
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
    //监听数据变化
    this.cancleSub = () => {};
  }
  componentDidMount() {
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
    let result = this.state.showinputValue;
    let action = ACTION_CHANGE_VALUE(result);
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
