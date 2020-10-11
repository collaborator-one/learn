react项目总结

1.子组件不能跳转报错Cannot read property ‘push’ of undefined，这是没有把history传给子组件，
<BottomBar  history={this.props.history}/>

需要这样把history传给子组件才能进行路由跳转

https://blog.csdn.net/weixin_37865166/article/details/89477489

