# 元素是构成 React 应用的最小砖块

- 元素描述了你在屏幕上想看的内容

```javascript
const element = <h1>Hello,world!</h1>;
```

> 与浏览器 DOM 不同,React 元素是创建开销极小的普通对象,ReactDom 会负责更新 DOM 来与 React 元素保持一致

# 将一个元素渲染为 DOM

假设你的 HTML 文件有一个 div

```html
<div id="root"></div>
```

- 我们将其称为根节点,因为根节点所有的内容都是由 ReactDom 组成的

- 仅仅使用 React 构建的应用通常只有单一的根节点,但是如果你将 React 集成到一个已有的应用中,那么你可以在应用中包含任意多的独立跟 DOM 节点

- 想要一个 React 元素渲染到根 DOM 节点中,必须要通过 ReactDOM.render()

```javascript
const element = <h1> Hello,world!</h1>;
ReactDOM.render(element, document.getElementById("root"));
```

# 更新已渲染的元素

- React 元素是不可变对象,一旦被创建,你就无法更改它的子元素或者属性,一个元素就像电影里面的单帧,他表示某个特定时刻的 UI

- 而更新 UI 唯一的方式就是创建一个全新的元素,并且将其传入 ReactDOM.render()

```javascript

function tick(){
  const element = {
    <div>
      <h1>Hello,world</h1>
      <h2>It is {new Date().toLocaleTimeString()}</h2>
    </div>
  };
  ReactDOM.render(element,document.getElementById("root"))
}

setInterval(tick,1000)
```

# React 只会更新它需要更新的部分

- ReactDOM 会将元素和它的子元素与它们之前的状态进行比较,并且只会进行必要的更新来使 DOM 达到预期的状态

# 总结

1. 元素是组件里面一个最小的部分

2. 元素必须放在 render 函数下面,并且 render 函数下面必须有 return 返回值

3. React 只有在数据发生改变的时候,并且最终在 render 函数里面渲染才会更新 DOM
