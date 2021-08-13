# 拆分 redux

> 有的时候逻辑非常复杂,我们需要将各个部件分成多个 reducer

## 建立里面的小的 reducer

- 创建小的 reducer.js

```javascript
import { CHANGE_VALUE } from "../actiontypes";

const defaultState = {
  inputValue: "123",
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

## 然后在总的 reducer.js 合成

```javascript
import { combineReducers } from "redux";
import headerReducer from "./modules/partOne";
export default combineReducers({
  header: headerReducer,
});
```

## 组件里面使用的时候

- state 的时候变量名称必须变了

- 方法不用改

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
    inputValue: state.header.inputValue,
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
