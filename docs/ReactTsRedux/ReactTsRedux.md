# React 在 TS 中使用 Redux

- 这里我选择了 react-redux ,redux-thunk,redux

## 安装包依赖

```javascript
cnpm install @types/react-redux
cnpm i redux
cnpm i react-redux
cnpm i redux-thunk
```

## (特别重要) 修改 react-app-end.d.ts

- 在 src 目录下面有一个文件 react-app-env.d.ts

```javascript
/// <reference types="react-scripts" />
// 修改ReduxTools工具
interface Window extends REDUXTOOS {
  __REDUX_DEVTOOLS_EXTENSION_COMPOSE__:
    | string
    | __REDUX_DEVTOOLS_EXTENSION_COMPOSE__;
}

declare var window: Window;
```

## 在 src 目录下新建一个 store 文件夹,接着创建下面几个文件

- actiontypes.tsx

- actioncreaters.tsx

- reducers.tsx

- store.tsx

### actiontype.tsx

- 规定 action 的名字

```javascript
export const ADD = "ADD";
export const DELETE = "DELETE";
export const CHANGE_VALUE = "CHANGE_VALUE";
```

### actioncreaters.tsx

- 创建数据处理工厂

```javascript
import { ADD, DELETE, CHANGE_VALUE } from './actiontypes'
//定义返回类型
interface ADD_RESULT {
  type: string
}
interface DELETE_RESULT {
  type: string
  value: number
}

interface CHANGE_VALUE_RESULT {
  type: string
  value: string
}

//定义方法

export const add_action = (): ADD_RESULT => {
  return {
    type: ADD,
  }
}

export const delete_action = (value: number): DELETE_RESULT => {
  return {
    type: DELETE,
    value: value,
  }
}

export const change_action = (value: string): CHANGE_VALUE_RESULT => {
  return {
    type: CHANGE_VALUE,
    value: value,
  }
}
```

### reducers.tsx

- 写获取到数据后的处理方法

- 定义数据类型

- 绑定 action 数据类型

- 最后暴露类型

```javascript
import { ADD, DELETE, CHANGE_VALUE } from './actiontypes'

//定义数据

export interface StateResult {
  value: string
  list: (string | number)[]
}

const defaultResult: StateResult = {
  value: '',
  list: [],
}

export default (state = defaultResult, action: any): StateResult => {
  switch (action.type) {
    case ADD:
      let newresult = JSON.parse(JSON.stringify(state))
      newresult.list.push(state.value)
      newresult.value = ''
      return newresult
    case DELETE:
      let newresult2 = JSON.parse(JSON.stringify(state))
      newresult2.list.splice(action.value, 1)
      return newresult2
    case CHANGE_VALUE:
      let newresult3 = JSON.parse(JSON.stringify(state))
      newresult3.value = action.value
      return newresult3
    default:
      return state
  }
}
```

### store.tsx

- 改变 store 里面的内容

```javascript
import { createStore, compose, applyMiddleware } from "redux";
import reducer from "./reducers";
import thunk from "redux-thunk";

const composeEnhancers =
  typeof window === "object" && window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__
    ? window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({
        // Specify extension’s options like name, actionsBlacklist, actionsCreators, serialize...
      })
    : compose;

const enhancer = composeEnhancers(applyMiddleware(thunk));

const store = createStore(reducer, enhancer);

export default store;
```

### 组件里面使用

- 必须继承 RouteComponentProps 否则一定会报错

```javascript

import * as React from 'react'
import { withRouter, Link, RouteComponentProps } from 'react-router-dom'
import { connect } from 'react-redux'
import { StateResult } from '../../store/reducers'
import {
  add_action,
  delete_action,
  change_action,
} from '../../store/actioncreaters'
interface IHomeProps extends RouteComponentProps {
  value: string
  list: (string | number)[]
  handleAdd: () => void
  handleDel: (value: number) => void
  handleChange: (value: any) => void
}

const Home: React.FunctionComponent<IHomeProps> = (props) => {
  const { value, list, handleAdd, handleChange, handleDel } = props
  return (
    <div>
      <span>这就是首页 </span>
      <input type="text" onChange={handleChange} value={value}></input>
      <button onClick={handleAdd}>增加</button>
      <ul>
        {list.map((item, index) => {
          return (
            <li
              key={index}
              onClick={() => {
                handleDel(index)
              }}
            >
              {' '}
              {item}
            </li>
          )
        })}
      </ul>
      <Link to="/second">点击跳转</Link>
    </div>
  )
}
const mapStateToProps = (state: StateResult) => {
  return {
    value: state.value,
    list: state.list,
  }
}

const mapDispatchToProps = (dispatch: any) => {
  return {
    handleAdd() {
      dispatch(add_action())
    },
    handleDel(value: number) {
      dispatch(delete_action(value))
    },
    handleChange(e: any) {
      let result = e.target.value
      dispatch(change_action(result))
    },
  }
}
export default connect(mapStateToProps, mapDispatchToProps)(withRouter(Home))

```

### index.tsx 里面代码引入 store

```javascript
import React from "react";
import ReactDOM from "react-dom";
import "./reset.css";
import RouterComponent from "./router";
import { Provider } from "react-redux";
import store from "./store/store";
ReactDOM.render(
  <Provider store={store}>
    <RouterComponent />
  </Provider>,

  document.getElementById("root")
);
```
