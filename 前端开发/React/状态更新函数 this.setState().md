# 关于setState的两种写法

```jsx
this.setState(stateChangeObject, [callback]);    // 第一个传入的参数为状态改变对象，为对象式。
this.setState(stateChangeFunction, [callback]);  // 第一个传入的参数为状态更新函数，为函数式。
```

### 示例

```jsx
import React, { Component } from "react";

export default class index extends Component {
    state = { count: 0 };

    countAddByFunc1 = () => {
        // 通过传入状态改变对象更新state，为对象式setState
        this.setState({ count: this.state.count + 1 }, () => {
            console.log(this.state.count);
        });
    };

    countAddByFunc2 = () => {
        // 通过传入更新函数，其返回值为状态改变对象，为函数式setState
        this.setState(
            // updater函数可以接受state和props两个参数，该函数返回的是一个状态改变对象。
            (state, props) => ({ count: state.count + 1 }),
            () => {
                console.log(this.state.count);
            }
        );
    };

    render() {
        return (
            <div>
                <p>当前求和为：{this.state.count}</p>
                <button onClick={this.countAddByFunc1}>countAddByFunc1</button>
                &nbsp;
                <button onClick={this.countAddByFunc2}>countAddByFunc2</button>
            </div>
        );
    }
}
```