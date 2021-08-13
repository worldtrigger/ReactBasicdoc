# React 中 写一个 Todolist

## 子组件 input 组件

- props 里面定义的就是父组件传过来的值

- 子组件自己的方法比如说 state 里面的值需要在 useState 里面声明

```javascript
import * as React from "react";
import { useState, useEffect } from "react";
interface IIndexinputProps {
  changeValue: Function;
}

const Indexinput: React.FunctionComponent<IIndexinputProps> = (props) => {
  const [value, setValue] = useState < string > "";
  function sendmessage() {
    props.changeValue(value);
    setValue("");
  }
  function changeInputvalue(e: any) {
    console.log(e.target.value);
    setValue(e.target.value);
  }
  return (
    <div>
      <input
        type="text"
        placeholder="请输入内容"
        onChange={changeInputvalue}
        value={value}
      ></input>
      <button onClick={sendmessage}>点击提交</button>
    </div>
  );
};

export default Indexinput;
```

## 子组件 list 列表

```javascript

import * as React from 'react'

interface IIndexComponentProps {
  listAll: (string | number)[]
  changeList: Function
}

const IndexComponent: React.FunctionComponent<IIndexComponentProps> = (
  props
) => {
  function deleteItem(index: number): void {
    console.log(index)
    props.changeList(index)
  }
  return (
    <ul>
      {props.listAll.map((item, index) => {
        return (
          <li
            onClick={() => {
              deleteItem(index)
            }}
            key={index}
          >
            {item}
          </li>
        )
      })}
    </ul>
  )
}

export default IndexComponent

```

## 父组件

- 父组件这里特别注意的就是改变 LIST 的值不能在原先的值上面改,一定要实例化,然后在赋值

```javascript

import * as React from 'react'
import Component1 from './component/IndexInput'
import Component2 from './component/IndexComponet'
import { withRouter, Link } from 'react-router-dom'
import { useState, useEffect } from 'react'
interface IIndexallProps {}

const Indexall: React.FunctionComponent<IIndexallProps> = (props) => {
  console.log(props)
  const [list, setList] = useState<(string | number)[]>([])
  const [value, setValue] = useState<string>('')

  function printresult(value2: string): void {
    setValue(value2)
    list.push(value2)
    setList(list)
  }
  function printList(value: number): void {
    console.log(value) //获取到index
    let result2 = JSON.parse(JSON.stringify(list))
    result2.splice(value, 1)
    console.log(result2)
    setList(result2)
  }
  return (
    <div>
      <span>首页</span>
      <Component1 changeValue={printresult}></Component1>
      <Component2 listAll={list} changeList={printList}></Component2>
      <Link to="/index">点击跳转到第二页</Link>
    </div>
  )
}

export default withRouter(Indexall)

```
