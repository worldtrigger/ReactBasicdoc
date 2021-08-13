# React-Router-Dom(路由)

## 安装

```javascript
cnpm i  react-router-dom -S
```

- 必须用到 BrowserRouter,Route,Switch 等等

## 改造 App.js 和 index.js

- App.js

```javascript
import React, { Component, Fragment } from "react";
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {};
  }
  render() {
    return <Fragment>{this.props.children}</Fragment>;
  }
}

export default App;
```

- index.js

> 引入路由

```javascript
import React from "react";
import ReactDOM from "react-dom";
import "./reset.css";
import RouterDom from "./router/router";

ReactDOM.render(
  <React.StrictMode>
    <RouterDom />
  </React.StrictMode>,
  document.getElementById("root")
);
```

## 新建一个文件夹里面新建一个 router.js

- 代码如下

- 必须用 BrowserRouter 包裹

- Switch 优先选择

- Route 就是路由

- exact 表示精确 必须完全符合

- - 放到最后 表示全部匹配

```javascript
import React, { Component } from "react";
import App from "../App";
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";
import HomePage from "../pages/Home";
import AboutPage from "../pages/About";
import NotFound from "../pages/Notfound";
class RouterDom extends Component {
  constructor(props) {
    super(props);
    this.state = {};
  }
  render() {
    return (
      <Router>
        <App>
          <Switch>
            <Route exact path="/" component={HomePage}></Route>
            <Route path="/about" component={AboutPage}></Route>
            <Route path="*" component={NotFound}></Route>
          </Switch>
        </App>
      </Router>
    );
  }
}

export default RouterDom;
```

## 组件里面使用,点击跳转

- 点击后跳转

- 第一种 this.props.history.push (刷新后数据丢失)

```javascript
this.props.history.push({
  pathname: "/about",
  state: {
    name: "从Home页面传递过来",
  },
});
```

- 页面里面获取值

> 这里获取值必须也只能判断 this.props.location.state

```javascript
  getRoutername() {
    let RouterData = this.props.location.state;
    if (RouterData) {
      return <span>{RouterData.name}</span>;
    } else {
      return null;
    }
  }
```

- 要是想接收到路由里面的值

> 必须在导出的时候使用 withRouter

```javascript
import { withRouter } from "react-router-dom";
export default withRouter(HomePage);
```

## 完整版

```javascript
import React, { Component } from "react";
import { withRouter } from "react-router-dom";
class HomePage extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "首页",
    };
    this.handleUrlAbout = this.handleUrlAbout.bind(this);
  }
  handleUrlAbout() {
    this.props.history.push({
      pathname: "/about",
      state: {
        name: "从Home页面传递过来",
      },
    });
  }
  getRoutername() {
    let RouterData = this.props.location.state;
    if (RouterData) {
      return <span>{RouterData.name}</span>;
    } else {
      return null;
    }
  }
  render() {
    const result = this.getRoutername();
    return (
      <div>
        <span>{this.state.message}</span>
        <button onClick={this.handleUrlAbout}>点击跳转到About页面</button>
        {result}
      </div>
    );
  }
}

export default withRouter(HomePage);
```

## 验证路径

- 获取当前页面的路径和跳转的路径做比对，比对上了加类

```javascript
  componentDidMount() {
    console.log(this.props);
    let url = this.props.location.pathname;
    console.log(url);
    console.log("----");
    console.log(window.location.pathname);
    let nowurl = window.location.pathname;
    if (url == nowurl) {
      console.log("正确");
    }
  }
```

# 如果是嵌套路由

## 路由里面 render

```javascript
<Route
  path="/common"
  render={() => {
    return (
      <CommonPages>
        <Switch>
          <Route
            path="/common/orderdetail/:id"
            component={CommonDetail}
          ></Route>
        </Switch>
      </CommonPages>
    );
  }}
></Route>
```

## 组件里面也需要嵌套一个

- this.props.children

```javascript
import React, { Component } from "react";
import { withRouter } from "react-router-dom";
class AboutPage extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "About页面",
    };
    this.handleurl = this.handleurl.bind(this);
  }

  handleurl() {
    this.props.history.push({
      pathname: "/",
      state: {
        name: "About页面传递过来值",
      },
    });
  }
  getRoutername() {
    let RouterData = this.props.location.state;
    console.log(RouterData);
    if (RouterData) {
      return <span>{RouterData.name}</span>;
    } else {
      return null;
    }
  }
  render() {
    const RouterData = this.getRoutername();
    return (
      <div>
        <span>{this.state.message}</span>
        <button onClick={this.handleurl}>点击跳转</button>
        {RouterData}
        {this.props.children}
      </div>
    );
  }
}

export default withRouter(AboutPage);
```

## 组件里面接收参数

- 路径传过来的参数只能靠 this.props.match.params.

```javascript
import React, { Component } from "react";
class AboutComponents extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "About页面里面的组件",
    };
  }
  componentDidMount() {
    console.log(this.props.match.params.id);
  }
  render() {
    return (
      <div>
        <span>{this.state.message}</span>
      </div>
    );
  }
}

export default AboutComponents;
```

## 路由拦截后面补充
