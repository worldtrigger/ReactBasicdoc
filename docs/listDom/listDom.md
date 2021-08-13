# 列表渲染

- 可以利用 map 循环 必须返回

- 返回的时候最外层必须有包裹层

- 列表循环必须要有 key,否则一定会报错误

## 入门

你可以通过使用{} 在 JSX 内构建一个元素集合

> 我们使用 Javascript 中的 map()方法来遍历 numbers 数组,将数组中的每一个元素变成 li 标签,最后我们将得到的数组赋值给 listitems

```javascript
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) => <li>{number}</li>);
```

我们把整个 listitems 插入到 ul 元素中去,然后渲染 DOM

```javascript
ReactDOM.render(<ul>{listItems}</ul>, document.getElementById("root"));
```

## 基础列表组件

- 渲染一个列表组件

```javascript
const numbers = [1, 2, 3, 4, 5];
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) => {
    <li key={number.toString()}>{number}</li>;
  });
  return <ul>{listItems}</ul>;
}
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById("root")
);
```

## key 值必须独一无二

数组元素中的 key 在兄弟节点中应该是独一无二的,当我们生成两个不同的数组的时候，我们可以使用相同的 key 值

```javascript
function Blog(props) {
  //左边第一个组件
  const sidebar = (
    <ul>
      {props.posts.map((post) => {
        <li key={post.id}>{post.title}</li>;
      })}
    </ul>
  );
  //右边一个组件
  const content = props.posts.map((posts) => {
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>;
  });
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}
const posts = [
  { id: 1, title: "Hello,world", content: "Welcome to learning" },
  { id: 2, title: "Hello,world", content: "Welcome to learning" },
];
ReactDOM.render(<Blog posts={posts} />, document.getElementById("root"));
```

## 在 JSX 中嵌入 map()

```javascript
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) => {
        <ListItem key={number.toString()} />;
      })}
    </ul>
  );
}
```

> 这样的话代码就很清晰,逻辑就很清楚了

## 总结

1. 循环的话能嵌套多个组件必须有包裹层
2. 循环的话必须有 key 值
