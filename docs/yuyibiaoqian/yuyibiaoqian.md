## Fragment 语义化标签

- 当最外层不想用 div 又必须用包裹层的时候

```javascript
import React, { Fragment } from "react";

function ListItem({ item }) {
  return (
    <Fragment>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </Fragment>
  );
}

function Glossary(props) {
  return (
    <dl>
      {props.items.map((item) => (
        <ListItem item={item} key={item.id} />
      ))}
    </dl>
  );
}
```

## htmlFor 比如 label 不能直接写 for

- 要用 htmlFor 来代替

```javascript
<label htmlFor="namedInput">Name:</label>
<input id="namedInput" type="text" name="name" />
```

## ref 来控制节点

- 给元素绑定 ref,ref 里面必须是方法

```bash
<input type="text" value={this.state.value} onChange = {this.changeall}
ref={(content)=>{this.content=content}} >
```

- 使用的时候

```bash

changeall() {
  console.log(this.content)
  let value = this.content.value
  /*这种情况就是异步操作*/
  this.setState(() => {
    return {
      value
    }
  })
}
```

## Context 可以传递很多层

> 有的时候父组件给子组件一层层传递数据非常麻烦,所以可以利用 Context 来传递

- 创建上下文的组件

```javascript
const { Provider, Consumer } = React.createContext(defaultValue);
```

- 父组件里面放置共享数据

```javascript
<Provider value={/*要共享的数据*/}>/*里面是要渲染的东西比如子组件*/</Provider>
```

- 子组件要使用父组件的数据了

```javascript
<Consumer>
  {(value) => {
    /*value就是父组件传递过来的数据子组件可以使用*/
  }}
</Consumer>
```

## 实例的例子

- App.js 父组件

```javascript
//App.js
import React from "react";
import Son from "./son"; //引入子组件
// 创建一个 theme Context,
export const { Provider, Consumer } = React.createContext("默认名称");
export default class App extends React.Component {
  render() {
    let name = "小人头";
    return (
      //Provider共享容器 接收一个name属性
      <Provider value={name}>
        <div
          style={{
            border: "1px solid red",
            width: "30%",
            margin: "50px auto",
            textAlign: "center",
          }}
        >
          <p>父组件定义的值:{name}</p>
          <Son />
        </div>
      </Provider>
    );
  }
}
```

- son.js 子组件

```javascript
//son.js 子类
import React from "react";
import { Consumer } from "./index"; //引入父组件的Consumer容器
import Grandson from "./grandson.js"; //引入子组件
function Son(props) {
  return (
    //Consumer容器,可以拿到上文传递下来的name属性,并可以展示对应的值
    <Consumer>
      {(name) => (
        <div
          style={{
            border: "1px solid blue",
            width: "60%",
            margin: "20px auto",
            textAlign: "center",
          }}
        >
          <p>子组件。获取父组件的值:{name}</p>
          {/* 孙组件内容 */}
          <Grandson />
        </div>
      )}
    </Consumer>
  );
}
export default Son;
```

- grandsom.js 孙组件

```javascript
//grandson.js 孙类
import React from "react";
import { Consumer } from "./index"; //引入父组件的Consumer容器
function Grandson(props) {
  return (
    //Consumer容器,可以拿到上文传递下来的name属性,并可以展示对应的值
    <Consumer>
      {(name) => (
        <div
          style={{
            border: "1px solid green",
            width: "60%",
            margin: "50px auto",
            textAlign: "center",
          }}
        >
          <p>孙组件。获取传递下来的值:{name}</p>
        </div>
      )}
    </Consumer>
  );
}
export default Grandson;
```

## 总结

1. Fragment 可以当包裹标签

2. ref 可以控制节点

3. 爷爷给孙组件传值利用 Context,爷爷利用 provider,孙子利用 consumer
   然后一级级传递
