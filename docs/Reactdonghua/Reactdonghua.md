# React 动画

`React动画必须依赖react-transition-group`

## 安装

```javascript
npm i react-transition-group -S
```

## 利用 CSSTransiton 作为壳子套 HTML 标签

- in 表示开关是否执行

- timeout 表示动画的执行时间

- appear 表示是否一加载就执行

- className 动画执行的名称

- unmountOnExit 动画执行完节点消失

- onEnter 表示钩子函数刚进入的时候,里面就一个参数 el

- onEnter onEntering onEntered onExit onExiting onExited 钩子函数

```javascript
import "./donghua.css";

class Donghua extends Component {
  constructor(props) {
    super(props);
    this.state = {
      flag: true,
    };
  }
  render() {
    return (
      <CSSTransition
        in={this.state.flag}
        timeout={2000}
        appear={true}
        classNames="fade"
        unmountOnExit
        onEnter={(el) => {
          el.style.color = "red";
        }}
      >
        <Fragment>
          <div>红色</div>
        </Fragment>
      </CSSTransition>
    );
  }
}
```

- 动画.css

```css
.fade-enter,
.fade-appear {
  transform: translate(0, 0);
}
.fade-appear-active {
  transform: translate(80%, 0);
  transition: all 2s linear;
}
.fade-enter-active {
  transform: translate(80%, 0);
  transition: all 2s linear;
}
.fade-enter-done {
  transform: translate(80%, 0);
}
.fade-exit {
  transform: translate(80%, 0);
}
.fade-exit-active {
  transform: translate(0, 0);
  transition: all 2s linear;
}
.fade-exit-done {
  transform: translate(0, 0);
  color: blue;
}
```

## ReactCss 动画结合

```javascript
<TransitionGroup key={index}>
  <CSSTransition
    in
    timeout={2000}
    appear={true}
    classNames="fade"
    unmountOnExit
    onEnter={(el) => {
      el.style.color = "red";
    }}
  >
    <List content={item} index={index} deletedate={this.deleteone.bind(this)} />
  </CSSTransition>
</TransitionGroup>
```
