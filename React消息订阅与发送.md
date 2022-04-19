# React消息订阅与发送

需要用到的库：

https://github.com/mroderick/PubSubJS

### 发送方

```react
export default class Search extends Component {
  xxxx = () => {
    // 发送网络请求
    axios.get(`.../users?q=${value}`).then(
      (response) => {
        // 消息的发送
        PubSub.publish("订阅的主题", response.data.xxx);
      },
      (error) => {
        console.log("出错咯");
      }
    );
  };
}
```



### 订阅方：需要在组件加载到页面后立刻订阅消息

```react
export default class List extends Component {
    state = {users:[]}
    componentDidMount(){
        this.token = PubSub.subscribe('订阅的主题',(msg,stateObj)=>{
            this.setState({users:stateObj})
        })
    }
    componentWillUnmount(){
      	// 组件卸载前关闭订阅
        PubSub.unsubscribe(this.token)
    }
}
```

