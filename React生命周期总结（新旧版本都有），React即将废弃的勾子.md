# 旧版生命周期（16版本）
![在这里插入图片描述](https://img-blog.csdnimg.cn/4aa979c6739f462dbd587aa15acc6192.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ2h1YW5ZYW5nIENoZW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 初始化阶段：由ReactDOM.render()触发 -- 初次渲染
1. constructor()
2. componentWillMount()「新版本不推荐使用」
4. render()
5. componentDidMount()
#### 更新阶段：由组件内部this.setState() 或父组件重新render触发
1. shouldComponentUpdate()
2. componentWillUpdate()「新版本不推荐使用」
3. render()
4. componentDidUpdate()
#### 卸载组件阶段：由ReactDOM.unmountComponentAtNode() 触发
1. componentWillUnmount()

# 新版生命周期（17版本）
![在这里插入图片描述](https://img-blog.csdnimg.cn/949a1562d34940739f2c96e583b67a7e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ2h1YW5ZYW5nIENoZW4=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
#### 初始化阶段：由ReactDOM.render()触发 -- 初次渲染
1. constructor()
2. getDerivedStateFromProps()
3. render()
4. componentDidMount()
#### 更新阶段：由组件内部this.setState() 或父组件重新render触发
1. getDerivedStateFromProps()
2. shouldComponentUpdate()
3. render()
4. getSnapShotBeforeUpdate()
5. componentDidUpdate()
#### 卸载组件阶段：由ReactDOM.unmountComponentAtNode() 触发
1. componentWillUnmount()

# 即将废弃的勾子
1. componentWillUpdate()
2. componentWillReceiveProps()
3. componentWillMount()
记法：除了componentDidUpdate()这个组件卸载阶段的勾子外，其他带Will的勾子在新版本中都不推荐使用

⚠️ componentWillUpdate()、componentWillReceiveProps()、componentWillMount()在react新版本中需要加上UNSAFE_前缀才可以使用，否则会出现警告，而且在未来的版本中，这3个勾子可能被废弃。

> 来源：
> 尚硅谷React教程，这个课很好

