# ReactHooks 之 useMemo 和 useCallback

> useMemo 和 useEffect 差不多。但是还是有区别,简单来说还是缓存

- 传递两个参数,第一个参数是处理逻辑,第二个参数是什么时候执行

> 它的第二个条件判断的不是真还是假。它判断的是否改变,改变就执行,不改变就不执行 比如 假变真它执行,真变假它也执行

# useMemo

## 本组件

- age 为 1 执行,age 为 2 不执行,age 为 3 又执行了。age 为 4 以及以上就都不执行了

- 顺序是 假变真执行，真变假也执行

- 有返回值

```javascript
import React, { useState, useMemo } from "react";

const APP = () => {
  let [age, setage] = useState(0);
  function handleClick() {
    setage(() => {
      return age + 1;
    });
  }
  const double = useMemo(() => {
    return age * 2;
  }, [age == 3]);
  return (
    <div>
      <div>
        {age} --{double}
      </div>
      <button onClick={handleClick}>点击改变</button>
    </div>
  );
};

export default APP;
```

## 有子组件

- 父元素

```javascript
import React, { useState, useMemo } from "react";
import HeaderContent from "./component/Header";
const APP = () => {
  let [age, setage] = useState(0);
  function handleClick() {
    setage(() => {
      return age + 1;
    });
  }
  const double = useMemo(() => {
    return age * 2;
  }, [age == 3]);
  return (
    <div>
      <div>
        {age} --{double}
      </div>
      <button onClick={handleClick}>点击改变</button>
      <HeaderContent resultNum={double}></HeaderContent>
    </div>
  );
};

export default APP;
```

- 子组件(必须用 memo 包裹起来)

```javascript
import React, { memo } from "react";
const HeaderContent = memo((props) => {
  return (
    <div>
      <span>{props.resultNum}</span>
    </div>
  );
});

export default HeaderContent;
```

# useCallback

- 这个和 useMemo 一样，只不过它缓存的不是值而是事件

## 父组件

```javascript
import React, { useState, useMemo, useCallback } from "react";
import HeaderContent from "./component/Header";
const APP = () => {
  let [age, setage] = useState(0);
  function handleClick() {
    setage(() => {
      return age + 1;
    });
  }
  const double = useMemo(() => {
    return age * 2;
  }, [age == 3]);
  const handleClick2 = useCallback(() => {
    console.log("点击了渲染");
  }, []);
  return (
    <div>
      <div>
        {age} --{double}
      </div>
      <button onClick={handleClick}>点击改变</button>
      <HeaderContent
        resultNum={double}
        changeresult={handleClick2}
      ></HeaderContent>
    </div>
  );
};

export default APP;
```

## 子组件

```javascript
import React, { memo } from "react";
const HeaderContent = memo((props) => {
  console.log(props);
  return (
    <div>
      <span onClick={props.changeresult()}>{props.resultNum}</span>
    </div>
  );
});

export default HeaderContent;
```
