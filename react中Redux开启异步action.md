# react中Redux开启异步action
目的：想在action中实现方法调用的延迟


⚠️：异步action不是必须要写的，完全可以在组件中写setTimeout()开启异步任务，等待异步任务结果后再去dispatch同步的action

1⃣️ 安装中间件

```handlebars
yarn add redux-thunk
```
2⃣️ 配置store

在`store.js`中录入以下配置

```javascript
// 这里需要从redux中导入applyMiddleware
import {...... ,applyMiddleware} from 'redux'
import XXXXXReducer from './XXXXX_reducer'

// 引入redux-thunk，用于支持异步action
import thunk from 'redux-thunk'

// 需要写上applyMiddleware(thunk)
export default createStore(XXXXXReducer,applyMiddleware(thunk))
```

3⃣️ 创建action函数不再返回一般对象，而是一个函数，在函数中写异步任务

```javascript
import { INCREMENT } from "./constants";
export const createIncrementAction = (data) => ({ type: INCREMENT, data });

// 异步action，就是值action的值为函数。异步action中一般都会调用同步action
export const createIncrementAsynAction = (data,time)=>{
    return (dispatch)=>{
        setTimeout(() => {
            dispatch(createIncrementAction(data))
        }, time);
    }
}
```

4⃣️ 在组件中的使用：

```javascript
export default class count extends Component {
	......
	
	incrementAsync = () => {
		store.dispatch(createIncrementAsynAction(value*1,500));
	};
	  
	......
}
```

