### 阅读前须知
1⃣️ 本文不适合没有React Router基础的读者
2⃣️ 本文只针对Router**第五版**的内容做阐述，很多东西和Router**第六版**不一样❗️
3⃣️ 文章内容整理自尚硅谷React视频，这个视频是全网最好的React视频，没有之一
4⃣️ 可根据目录阅读文章
![](https://img-blog.csdnimg.cn/ea5979d15f0f46f0ac482e0573c24615.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ2h1YW5ZYW5nIENoZW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
### 路由的基本使用

1. 明确好页面中的导航区，展示区
2. 导航区的`<a>`标签改为`<Link to="/xxx">...</Link>`标签
3. 展示区写Route标签进行路径的匹配：`<Route path="/xxx" component={Demo}/>`
4. 在`index.js`里的`<APP/>`的最外侧包裹一个`<BrowserRouter>`或`<HashRouter>`

### 路由组件和一般组件
1⃣️ 写法不同
一般组件： `<Demo/>`
路由组件：`<Route path="/demo" component={Demo}/>`
2⃣️ 存放位置不同
一般组件：存放在components文件夹中📁
路由组件：存放在pages文件夹中📁
3⃣️接收到的props不同
一般组件：写组件标签时，传递了什么就收到了什么
路由组件：接收到三个固定的属性

```
history:
	go(n)
	goBack()
	goForward()
	push(path,state)
	replace(path,state)
location:
	pathname
	search
	state
match
	params
	path
	url
```
### NavLink的使用（取代Link）
`<NavLink>`和`<Link>`区别在点击Link标签后，`<NavLink>`会实现路由链接的高亮，如图。其原理是在选中项中的class里加入active类。如果在编码过程中按钮高亮效果并没有写在active类里，则可以在标签中加入`activeClassName="..." `这个属性值，指定高亮效果的类
`
<NavLink activeClassName="高亮效果类名" to="/demo">Demo</NavLink>
`

![在这里插入图片描述](https://img-blog.csdnimg.cn/b77b5249d8f64d83bdac6772205cdad6.png)
### Switch标识的使用（V5版本使用）
通常情况下，path和component是一一对应的关系，`<Switch></Switch>`标签中的`<Route/>`在匹配到第一个组件后，就不会继续向下匹配了，使用`<Switch></Switch>`能提高路由匹配效率
⚠️ 注意：注册的路由有一个以上才用这个标签包裹


### 解决多级路径刷新页面样式丢失的问题（V5版本）
1. `public/index.html` 中引入样式时不写 `./` 写成 `/`（常用）
2.  `public/index.html` 中引入样式时不写 `./` 写成`%PUBLIC_URL%/`（常用）
3. 使用`HashRouter`，不要用BrowserRouter

### 路由的严格匹配与模糊匹配
1. React默认使用的是模糊匹配
2. 开启严格匹配：`<Route exact path="/demo" component={Demo} />`
3. 严格匹配能不开启就不要开启

### Redirect标签的使用
1. 一般写在所有路由注册的最下方，当所有路由都无法匹配时，跳转到Redirect指定的路由
`<Redirect to="/demo"></Redirect>`

### 嵌套路由
1. 嵌套子路由时要写上父路由的Path值
2. 路由的匹配是按照注册路由的顺序进行的

  
### 向路由组件传递参数
⭕️ 1⃣️ params参数
1. 路由链接（用「`/value/value`」方式携带参数）：
```
<Link to={`/demo/${Obj.id}/${Obj.title}`}>...</Link>
```
2. 注册路由（声明接收）
用`/:自定义参数名`接收路由链接携带的参数
`<Route path="demo/:id/:title" component={Demo} />`
3. Demo组件里接收参数
`const {id,title} = this.props.match.params`

⭕️ 2⃣️ search参数
1. 路由链接（用「`/?key1=...&key2=...`」方式携带参数）：
```
<Link to={`/demo/?id=${Obj.id}&title=${Obj.title}`}>...</Link>
```
2. search参数无需声明接收,正常注册路由即可
`<Route path="demo/" component={Demo} />`
3. Demo组件里接收参数
1⃣️ 通过 `npm i -save-dev query-string`安装依赖
2⃣️ `import QueryString from 'query-string'`
3⃣️ 接收search参数
    - const {search} = this.props.location
   -  const {id,title} = QueryString.parse(search.slice(1))

⭕️ 3⃣️ state参数
1. 路由链接（用「`to={{pathname:'组件路径',state:{key1:value1,key2:value2}}}`」方式携带参数）：
```
<Link to={{pathname:'/home/message/...',state:{id:Obj.id,title:Obj.title}}}>...</Link>
```
2.  state参数无需声明接收,正常注册路由即可
`<Route path="/home/message/..." component={Demo} />`
3. Demo组件里接收参数
`const {id,title} = this.props.location.state`
⚠️ 备注：`BrowserRouter`刷新也可以保留住参数，但如果使用`HashRouter`，刷新后会导致路由state参数的丢失❗️


### 编程式路由导航
借助`this.props.history`对象上的API对操作路由跳转、前进、后退

- `this.props.history.push(path,[state])`
- `this.props.history.replace(path,[state])`
- `this.props.history.goBack()`
- `this.props.history.goForward()`
- `this.props.history.go(n)`

[编程式路由导航英文参考文档](https://www.pluralsight.com/guides/using-react-with-the-history-api)

### 在一般组件里使用路由组件的API
代码模版：
```javascript
import React, { Component } from "react";
// 引入一个封装一般组件成为路由组件的方法
import { withRouter } from "react-router-dom";

class Header extends Component {
  // 路由组件API中提供的跳转方法
  forward = () => {
    this.props.history.goForward();
  };
  back = () => {
    this.props.history.goBack();
  };

  render() {
    return (
      <div className="page-header">
        {/* 页面跳转按钮 */}
        <button onClick={this.forward}>前进</button>
        <button onClick={this.back}>后退</button>
      </div>
    );
  }
}

// 向外暴露经过withRouter()封装的组件，让一般组件具备路由组件所特有的API
export default withRouter(Header);
```
- withRouter()是一个方法，不是一个组件，他可以加工一般组件，使其具有路由组件所特有的API
- withRouter()返回值是一个新组件


