# React 使用 Lazy

## 懒加载

- 必须使用 lazy,Suspense 做到懒加载

## 引入

```javascript
import React, { Component, Fragment, lazy, Suspense } from "react";
```

## 懒加载 lazy 里面传递函数

```javascript
//懒加载
const SecondComponent = lazy(() => {
  return import("./Second.js");
});
```

## 使用懒加载的时候必须用 Suspense

- 用了 Suspense 必须用 fallback,它的作用就是当懒加载没有加载出来的时候,它显示的内容

```javascript
import React, { Component, Fragment, lazy, Suspense } from "react";
//懒加载
const SecondComponent = lazy(() => {
  return import("./Second.js");
});
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "首页",
      hasError: false,
    };
  }
  //捕获异常
  componentDidCatch() {
    this.setState({
      hasError: true,
    });
  }
  render() {
    if (this.state.hasError) {
      return <div>渲染失败</div>;
    } else {
      return (
        <Fragment>
          <div>{this.state.message}</div>
          <Suspense fallback={<div>loading</div>}>
            <SecondComponent></SecondComponent>
          </Suspense>
        </Fragment>
      );
    }
  }
}

export default App;
```
