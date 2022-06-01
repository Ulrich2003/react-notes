# React中Redux的创建和使用（代码模版）

### 前言：
redux原理图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/0b61353d35194322816b6c53786e985f.png#pic_center)
<hr>

### Redux的创建：

1⃣️ 创建redux文件夹

**🧯警告🛠：**

<font color=red size=4>❌修正错误：之前误将redux文件夹📁创建在components文件夹📁下，目前文章已经更正。但是后面的代码模板中所出现的：
`import XXX from '../..../....'`
导入路径可能有误，按您本地的路径为准。</font>

![在这里插入图片描述](https://img-blog.csdnimg.cn/abafe160fc51460fac6d35f5525e6e8a.png)



在redux文件夹📁里面创建以下几个文件📃：

- `xxx(自定义名称)_reducer.js`
-  `xxx(自定义名称)_action.js`
-  `constants.js`
-  `store.js`

2⃣️ 填入`constants.js`

```handlebars
/* 
    该模块用于定义action对象中type类型的常量值
*/

export const INCREMENT = 'increment'
export const DECREMENT = 'decrement'
```

3⃣️ 填入`xxx(自定义名称)_reducer.js`

```javascript
/* 
    1. 该文件用于创建一个为XXX组件服务的reducer，reducer的本质是一个函数
    2. reducer函数会接收到俩个参数，分别是：之前的状态（preState），动作对象（action）
*/
import { INCREMENT,DECREMENT } from "./constants" // 引入常量
const initState = 0 // 初始化数据
export default function xxx（小驼峰自定义名称）Reducer(preStete = initState,action) {
    // 从动作（action）对象里面取出要干嘛，以及这件事的数据
    const {type,data} = action
    // 根据type决定如何加工数据
    switch(type){
        case INCREMENT:
            return preStete + data
        case DECREMENT:
            return preStete - data
        default:
            return preStete
    }
}
```
- reducer的本质是一个函数，接收：preState，action，返回加工后的状态
- reducer有两个作用：初始化状态，加工状态
- reducer被第一次调用时，是store自动触发的，传递的preState是undefined

4⃣️ 填入`xxx(自定义名称)_action.js`

```javascript
/* 
    该文件专门为Count组件生成action对象
*/
import { INCREMENT, DECREMENT } from "./constants";

export const createIncrementAction = (data) => ({ type: INCREMENT, data });

export const createDecrementAction = (data) => ({ type: DECREMENT, data });

```

5⃣️ 填充 `stroe.js` 文件：

- 1）引入redux中createStore函数，创建一个store
- 2）createStore调用时要传入一个为其服务的reducer
- 3）记得暴露store对象

```javascript
// 引入createStore方法，创建redux中最为核心的Store对象
import {createStore} from 'redux'
// 引入reducer
import myReducer from './xxx(自定义名称)_reducer'
// 暴露store
export default createStore(myReducer)
```
6⃣️ 在`index.js`中检测store中状态的改变，一旦发生改变重新渲染`<App/>`

⚠️ redux只负责状态管理，至于状态的改变驱动着页面的展示，要靠程序员自己写

```javascript
import React from "react";
import ReactDOM from "react-dom";
import store from "./redux/store";
import App from "./App";

ReactDOM.render(<App />, document.getElementById("root"));

store.subscribe(() => {
  ReactDOM.render(<App />, document.getElementById("root"));
});
```

<hr>

### Redux的使用：

在想要使用redux的.jsx组件中：

```javascript
import React, { Component } from "react";
import store from "../redux/store"; // 引入redux核心store
// 引入redux的 action 对象
import { createIncrementAction, createDecrementAction } from "../redux/count_action";

export default class count extends Component {
  increment = () => {
    // 修改redux中的数据
    store.dispatch(createIncrementAction(1));
  };
  decrement = () => {
    // 修改redux中的数据
    store.dispatch(createDecrementAction(1));
  };
  render() {
    return (
      <div>
      	{/* 获取redux中的数据 */}
        <h1>当前求和为：{store.getState()}</h1>
        <button onClick={this.increment}>➕</button>
        <button onClick={this.decrement}>➖</button>
      </div>
    );
  }
}
```

