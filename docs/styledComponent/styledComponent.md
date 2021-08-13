# 样式冲突 Styled-Components

> styled-components 它的作用就是怕里面的个个 css 之间相互重叠,覆盖,犯冲突,所以引入了这个组件,因为他只对本组件管用

- 这个组件仅仅负责样式 不负责逻辑

## 安装

```javascript
npm install --save styled-components
```

## 基础语法

- 用组件的形式编写 css,必须引入 styled

```javascript
import styled from "styled-components";

export const HomeWrap = styled.div`
  width: 1200px;
  height: 150px;
  margin: 0 auto;
  background: blue;
  text-align: center;
  color: yellow;
`;

export const HomeHeader = styled.div`
  width: 800px;
  height: 75px;
  margin: 0 auto;
  background: red;
`;
```

- 组件里面使用的时候

```javascript
import React, { Component } from "react";
import { HomeHeader, HomeWrap } from "./jscss/Header";
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "首页",
    };
  }
  render() {
    return (
      <div>
        <HomeWrap>
          <HomeHeader>{this.state.message}</HomeHeader>
        </HomeWrap>
      </div>
    );
  }
}

export default App;
```

## 全局样式

- styled-components 提供了 createGlobalStyle 可以设置全局样式

- 例如下面就是简单版本的全局引入,里面有 iconfont 请注意路径匹对

- 代码

```javascript
import { createGlobalStyle } from "styled-components";
const GrobalStyle = createGlobalStyle`
    html, body, div, span {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
    }
    /* HTML5 display-role reset for older browsers */
    article, aside, details, figcaption, figure,
    footer, header, hgroup, menu, nav, section {
    display: block;
    }
    body {
    line-height: 1;
    background:red;
    }
    ol, ul {
    list-style: none;
    }
    blockquote, q {
    quotes: none;
    }
    blockquote:before, blockquote:after,
    q:before, q:after {
    content: '';
    content: none;
    }
    table {
    border-collapse: collapse;
    border-spacing: 0;
    }
  @font-face {font-family: "iconfont";
  src: url('../iconfont/iconfont.eot?t=1563781351915'); /* IE9 */
}

.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.icon-tupian:before {
  content: "\e643";
}

.icon-fanhui:before {
  content: "\e624";
}

.icon-jiantouxia:before {
  content: "\e62d";
}

.icon-sousuo:before {
  content: "\e632";
}
`;
export default GrobalStyle;
```

- 页面上引入的时候千万注意他也是组件。但是不能包含其他组件。必须优先引入,在 app.js 里面 Global 必须放在外面

```javascript
import Global from "./pages/commonstyle";

class App extends Component {
  render() {
    return (
      <Fragment>
        <Global />
        <PageUi
          inputValue={this.state.inputValue}
          handchange={this.handchange}
          handClick={this.handClick}
          deleteItem={this.deleteItem}
          list={this.state.list}
        />
        <Index />
      </Fragment>
    );
  }
}

export default App;
```

## 背景图片引入

- ${} 里面放的变量

```javascript
import styled from "styled-components";
import Middleimg from "../../../images/2019-07-20_200948.png";
export const MiddleWraper = styled.div`
  width: 880px;
  height: 100px;
  float: left;
  background: url(${Middleimg});
`;
```

## 组件传值的方式也可以(尽量少用)

- 组件传值只能是地址不能是相对路径

### 父组件

```javascript
import React, { Component, Fragment } from "react";
import Header from "./components/Header.js";
class Index extends Component {
  constructor(props) {
    super(props);
    this.state = {
      imgurl:
        "http://fuss10.elemecdn.com/c/cd/c12745ed8a5171e13b427dbc39401jpeg.jpeg?imageView2/1/w/114/h/114",
    };
  }
  render() {
    return (
      <Fragment>
        <Header imgurl={this.state.imgurl} />
      </Fragment>
    );
  }
}

export default Index;
```

### 子组件

- 把值放到属性里面

```javascript
<RightWraper imgurl={this.props.imgurl} />
```

- css 里面变量变成

```javascript
export const RightWraper = styled.div`
  width: 100px;
  height: 100px;
  float: left;
  background-image: url(${(props) => props.imgurl});
  background-size: contain;
`;
```

## 还可以加标签属性

```javascript
export const NavSearch = styled.input.attrs({
  placeholder: "搜索框架",
  type: "text",
})`
  width: 200px;
  height: 50px;
`;
```

## 继承

- 比如一个按钮 button 只想改变颜色 不想改变大小

- styled(xxxx)

```javascript
export const HomeHeader = styled.div`
  width: 800px;
  height: 75px;
  margin: 0 auto;
  background: red;
`;

export const HomeHeaderson = styled(HomeHeader)`
  width: 300px;
  height: 100px;
  background: black;
  float: left;
`;
```

## 总结

- 利用 styled-components 这样可以避免 css 混淆

- styled-components 法则
