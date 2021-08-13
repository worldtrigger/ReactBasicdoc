# ReactHooks 之 UseEffect

> useEffect 主要的作用就是替代生命周期函数

- 它接收两个参数,第一个参数表示回调函数,第二个表示生命周期函数

- 第二个参数表示它什么时候执行,如果没有第二个参数表示只要组件渲染都执行

## 重点第二个函数

- 当 useEffect 没有第二个参数

> 组件的初始化和更新都会执行

```javascript
useEffect(() => {
  console.log("哈哈哈");
});
```

- 当 useEffect 第二个参数是空数组(仅仅执行一次)

> 初始化,和卸载的时候执行,相当于 componentDidMount,componentWillUnMount

```javascript
useEffect(() => {
  console.log("哈哈哈");
}, []);
```

- 当有一个或者多个值的数组

```javascript
useEffect(() => {
  console.log("哈哈哈");
}, [name, age]);
```

`当有一个值的时候,该值有变化就执行`

`当有多个值的时候,只要有一个不相等就执行`

## 监听函数

- 有 return 就表示卸载

```javascript
useEffect(() => {
  //监听
  window.addEventListener("resize", onResize, false);
  //监听完了就考虑卸载,卸载的时候必须是return
  return () => {
    window.removeEventListener("resize", onResize, false);
  };
}, []);
```
