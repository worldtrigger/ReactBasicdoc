# React 使用 Typescript

## 创建一个 React 的 TS 项目

```javascript
create-react-app demo --template typescript
```

## 安装支持的插件

> Typescript React code snippets

## 使用 Typescript 编写一个类组件

- 类组件可以接受 props,并且每个类组件都有 state 数据,所以它需要两个数据规范 IAppProps,IAppState

- Vscode 装完插件后 tsrcfull 缩略版

## 父组件

```javascript

import * as React from 'react'
import SecondComponent from './component/Second1'
export interface ISecondProps {}

export interface ISecondState {
  count: number
  title: string
}

export default class Second extends React.Component<
  ISecondProps,
  ISecondState
> {
  constructor(props: ISecondProps) {
    super(props)

    this.state = {
      count: 0,
      title: 'Second标题',
    }
    this.changeCount = this.changeCount.bind(this)
  }
  changeCount() {
    let result = this.state.count + 1
    this.setState({
      count: result,
    })
  }
  public render() {
    return (
      <div>
        {this.state.title}--{this.state.count}
        <button onClick={this.changeCount}>点击增加</button>
        <SecondComponent count={this.state.count}></SecondComponent>
      </div>
    )
  }
}

```

## 子组件

```javascript

import * as React from 'react'

export interface ISecond1Props {
  count: number
}

export interface ISecond1State {
  title: string
}

export default class Second1 extends React.Component<
  ISecond1Props,
  ISecond1State
> {
  constructor(props: ISecond1Props) {
    super(props)

    this.state = {
      title: '子组件标题',
    }
  }

  public render() {
    return (
      <div>
        {this.state.title}---{this.props.count}
      </div>
    )
  }
}

```

## 使用 Typescript 编写一个函数组件

- 因为使用了 hooks 所以它只需要一个接口标注

- 要是使用 vscode 快捷键就是 tsrsfc

### 父组件

```javascript
import * as React from "react";
import { useState, useEffect } from "react";
import Home1 from "./component/Home1";
interface IHomeProps {
  childcount: number;
}

const Home: React.FunctionComponent<IHomeProps> = (props) => {
  const [count, setCount] = useState < number > 0;
  function addcount() {
    setCount(count + 1);
  }
  return (
    <div>
      <span>Home父组件内容数字是{count}</span>
      <button onClick={addcount}>点击增加数字</button>
      <Home1 childcount={count}></Home1>
    </div>
  );
};

export default Home;
```

### 子组件

```javascript
import * as React from "react";

interface IHome1Props {
  childcount: number;
}

const Home1: React.FunctionComponent<IHome1Props> = (props) => {
  const { childcount } = props;
  return <div>Home组件1--{childcount}</div>;
};

export default Home1;
```
