## 在 TS 中使用 react-router

### 首先安装 react-router-dom

```javascript

cnpm i  react-router-dom -S

cnpm i @types/react-router-dom -S
```

### 路由使用

- 使用 withRouter,RouteComponentProps

- 结果的时候必须用 withRouter 包裹

- 接口 Props 必须继承 RouteComponentProps 否则会报错

```javascript

import * as React from 'react'
import SecondComponent from './component/Second1'
import { withRouter, RouteComponentProps, Link } from 'react-router-dom'
interface ISecondProps extends RouteComponentProps {}

interface ISecondState {
  count: number
  title: string
}

class Second extends React.Component<ISecondProps, ISecondState> {
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
        <Link to="/">第一页</Link>
      </div>
    )
  }
}

export default withRouter(Second)

```
