# ReactHooks 之 useState

> React 以前都是以类来实现,新版本实现是以函数来实现。React 现在重点以函数为主,函数是 React 的第一公民

- 利用函数不用在改变 this 指针

## useState

- 它的主要作用就是保存状态,他接受一个参数就是默认值,它返回一个数组,数组第一个就是数据,第二个就是改变数据的方法

- 改变数据的方法里面的参数是函数，函数必须有 return

```javascript
import React, { useState } from "react";
const APP = () => {
  const [name, setname] = useState("首页");
  function changename() {
    setname(() => {
      let name = "首页";
      return name + parseInt(10 * Math.random());
    });
  }
  return (
    <div>
      <span>{name}</span>
      <button onClick={changename}>点击修改</button>
    </div>
  );
};

export default APP;
```
