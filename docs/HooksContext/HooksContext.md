# ReactHooks -- 爷孙数据(Context)

> Context 主要是为了方便爷孙之间的传值,有的时候不希望一层层传递了,需要跨级传递

## (1)创建并暴露出 Context

```javascript
import React, { useState, createContext } from "react";
export const ResultContext = createContext();
```

## (2) 父组件

- 里面必须包含子组件

- 并且用 provider 包裹

```javascript
import React, { useState, createContext } from "react";
import HeaderComponent from "./component/Header";
export const ResultContext = createContext();
const APP = () => {
  const [age, setage] = useState(0);
  function handleClick() {
    setage(() => {
      return age + 1;
    });
  }
  return (
    <div>
      <div>{age}</div>
      <button onClick={handleClick}>点击改变</button>
      <ResultContext.Provider value={age}>
        <HeaderComponent></HeaderComponent>
      </ResultContext.Provider>
    </div>
  );
};

export default APP;
```

## (3) 子组件使用

- 函数 必须使用 useContext

```javascript
import React, { useContext } from "react";
import { ResultContext } from "../App";
const HeaderComponent = () => {
  const data = useContext(ResultContext);
  return <div>子属性{data}</div>;
};

export default HeaderComponent;
```

- 要是类 就必须用 Provider 和 Consumer

```javascript
import React, { Component, Fragment } from "react";
import { PublicDataOne, PublicDataTwo } from "./App";
class ThirdWrapper extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "第三层使用公共数据",
    };
  }
  render() {
    return (
      <Fragment>
        <div>
          {this.state.message}
          <PublicDataOne.Consumer>
            {(value) => {
              return (
                <PublicDataTwo.Consumer>
                  {(valueTwo) => {
                    return (
                      <h1>
                        这就是第一个公共数据{value} 这就是第二个公共数据
                        {valueTwo}
                      </h1>
                    );
                  }}
                </PublicDataTwo.Consumer>
              );
            }}
          </PublicDataOne.Consumer>
        </div>
      </Fragment>
    );
  }
}

export default ThirdWrapper;
```
