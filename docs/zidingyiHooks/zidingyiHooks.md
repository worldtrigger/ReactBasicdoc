# 自定义 Hooks

- 自定义 Hooks 必须以 use 开头,否则不读取

- 必须有返回值

## 返回值或者方法

- 自定义 Hooks 返回数组

```javascript
import { useState, useEffect } from "react";
function useCustomHooks(defaultCount) {
  const [count, setcount] = useState(defaultCount);
  useEffect(() => {
    console.log("测试执行");
  }, [count]);

  return [count, setcount];
}

export default useCustomHooks;
```

- 使用的时候

```javascript
import React from "react";
import SecondComponent from "./Second";
import UseHooks from "./CutomHooks";
function App() {
  const [count, setcount] = UseHooks(0);
  function handleClick() {
    setcount(() => {
      return count + 1;
    });
  }
  return (
    <div>
      <span>{count}</span>
      <button onClick={handleClick}>点击</button>
      <SecondComponent count={count}></SecondComponent>
    </div>
  );
}
export default App;
```

## 自定义 Hooks 返回 jsx

- CutomHooks(自定义组件返回 html)

```javascript
import React, { useState, useEffect } from "react";
function useCustomHooks(defaultCount) {
  const [count, setcount] = useState(defaultCount);
  useEffect(() => {
    console.log("测试执行");
  }, [count]);
  function handleClick() {
    setcount(() => {
      return count + 1;
    });
  }
  return (
    <div>
      <span>{count}</span>
      <button onClick={handleClick}>点击</button>
    </div>
  );
}

export default useCustomHooks;
```

- 使用的时候

```javascript
import React from "react";
import UseHooks from "./CutomHooks";
function App() {
  return <div>{UseHooks(0)}</div>;
}
export default App;
```
