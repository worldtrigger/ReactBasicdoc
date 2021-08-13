# ReactHooks 之 Ref

> 在 React 中想要获取到节点只有 useRef

- 获取本组件的元素

- 获取子组件的元素

## 获取本组件的元素

- 创建 useRef

- 绑定到元素上

```javascript
import React, { useState, useRef, useEffect } from "react";
const APP = () => {
  const [age, setage] = useState(0);
  const elementRef = useRef();
  function handleClick() {
    setage(() => {
      return age + 1;
    });
  }
  useEffect(() => {
    console.log(elementRef);
    console.log(elementRef.current);
  }, []);
  return (
    <div>
      <div ref={elementRef}>{age}</div>
      <button onClick={handleClick}>点击改变</button>
    </div>
  );
};

export default APP;
```

## 获取子组件的元素

> 要是想获取子组件的元素,那么子组件的类型必须是类,而且不能是函数

### 函数肯定会出错,所以必须是类

- 父元素

```javascript
import React, { useState, useRef, useEffect } from "react";
import HeaderComponent from "./component/Header";
const APP = () => {
  const [age, setage] = useState(0);
  const elementRef = useRef();
  function handleClick() {
    setage(() => {
      return age + 1;
    });
  }
  useEffect(() => {
    console.log(elementRef);
    console.log(elementRef.current);
  }, []);
  return (
    <div>
      <div>{age}</div>
      <button onClick={handleClick}>点击改变</button>
      <HeaderComponent ref={elementRef}></HeaderComponent>
    </div>
  );
};

export default APP;
```

- 子元素

```javascript
import React, { Component } from "react";
class Header extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "头部元素",
    };
  }
  render() {
    return (
      <div>
        <span>{this.state.message}</span>
      </div>
    );
  }
}

export default Header;
```
