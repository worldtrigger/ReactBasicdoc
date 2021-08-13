# 阶段总结

## ToDoList 概括

### 父组件

- App.js

```javascript
import React, { Component } from "react";
import HeaderCompoent from "./components/Header";
import ListComponent from "./components/List";
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "首页",
      headerinputContent: "显示的内容",
      list: [
        {
          id: 0,
          message: "第一条数据",
        },
        {
          id: 1,
          message: "第二条数据",
        },
      ],
    };
    this.changeValue = this.changeValue.bind(this);
    this.delItem = this.delItem.bind(this);
  }
  //改变value方法
  changeValue(result) {
    const resultList = JSON.parse(JSON.stringify(this.state.list));
    let id = 0;
    if (resultList.length > 0) {
      id = resultList[resultList.length - 1].id + 1;
    } else {
      id = 0;
    }

    let obj = {
      id,
      message: result,
    };
    resultList.push(obj);
    this.setState({
      headerinputContent: result,
      list: resultList,
    });
  }
  //删除数据
  delItem(index) {
    let result = JSON.parse(JSON.stringify(this.state.list));

    result.splice(index, 1);
    console.log(result);
    this.setState({
      list: result,
    });
  }
  render() {
    return (
      <div>
        <HeaderCompoent
          value={this.state.headerinputContent}
          changeValue={this.changeValue}
        ></HeaderCompoent>
        <ListComponent
          listData={this.state.list}
          delIndex={this.delItem}
        ></ListComponent>
      </div>
    );
  }
}

export default App;
```

### 子组件

- Header.js

```javascript
import React, { Component } from "react";
class Header extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "头部",
      InputContent: props.value,
    };
    this.handleChange = this.handleChange.bind(this);
    this.handleFocus = this.handleFocus.bind(this);
    this.handlesubmit = this.handlesubmit.bind(this);
  }
  handlesubmit() {
    const message = this.state.InputContent;
    this.props.changeValue(message);
    this.setState({
      InputContent: "",
    });
  }
  handleChange(e) {
    const result = e.target.value;
    this.setState({
      InputContent: result,
    });
  }
  handleFocus() {
    const result = "";
    this.setState({
      InputContent: result,
    });
  }
  render() {
    return (
      <div>
        <input
          type="text"
          value={this.state.InputContent}
          onChange={this.handleChange}
          onFocus={this.handleFocus}
        ></input>
        <button onClick={this.handlesubmit}>点击</button>
      </div>
    );
  }
}

export default Header;
```

- List.js

```javascript
import React, { Component } from "react";
class List extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "列表页",
    };
  }
  handleClick(index) {
    this.props.delIndex(index);
  }
  getList() {
    const listResult = this.props.listData;
    return (
      <ul>
        {listResult.map((content, index) => {
          return (
            <li
              key={index}
              onClick={() => {
                this.handleClick(index);
              }}
            >
              序号是:{content.id}---内容是{content.message}
            </li>
          );
        })}
      </ul>
    );
  }
  render() {
    const result = this.getList();
    return <div>{result}</div>;
  }
}

export default List;
```
