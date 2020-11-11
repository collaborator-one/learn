# react项目总结

1.子组件不能跳转报错Cannot read property ‘push’ of undefined，这是没有把history传给子组件，
<BottomBar  history={this.props.history}/>

需要这样把history传给子组件才能进行路由跳转

https://blog.csdn.net/weixin_37865166/article/details/89477489

如果想要给react组件的传递参数定义是必选的或者是类型，需要引入

```	
import PropTypes from 'prop-types'
```

这个是react的一个方法

<img src="https://img2018.cnblogs.com/i-beta/1470672/201911/1470672-20191120114320922-1234406628.png" alt="img" style="zoom:50%;" />

```j s
//设置默认值用法
//设置默认值--->存在默认值情况下必填参数可以不传值，没有默认值必须传值
HeaderComponent.defaultProps = {
    title: '默认值'
};
HeaderComponent是组件的类组件的类名
//设置值的类型和属性的用法
HeaderComponent.PropTypes = {
	title: PropTypes.func.isrequired
}
```

react非常提倡使用受控组件不提倡非受控组件

![image-20201027144435257](/Users/yangyang/Library/Application Support/typora-user-images/image-20201027144435257.png)

如图，下面选中的是受控组件，上面用ref写的就是非受控组件



# react学习总结

1.react的虚拟dom其实就是js对象模拟dom关系

图是新旧dom结构

<img src="/Users/yangyang/Library/Application Support/typora-user-images/image-20201021154341294.png" alt="image-20201021154341294" style="zoom:50%;" />

2.diff算法

tree diff. 新旧两个dom树，逐层对比的过程。

component diff。在进行tree diff逐层对比的过程中可能有各种各样的组件，需要进行组件对比，如果组件变了，就需要移除旧组件，创建新组件

element diff。在进行组件diff的时候，两个类型相同的组件，需要进行元素对比

<img src="/Users/yangyang/Library/Application Support/typora-user-images/image-20201021155102422.png" alt="image-20201021155102422" style="zoom:50%;" />

**webpack创建自己的项目**

1、运行npm init -y 初始化项目

2、项目根目录创建src，dist目录

3、src中建index.html和index.js入口文件。webpack 4以后的版本有个重要的特点就是约定大于配置，约定，默认的大包入口路径是src/index.js。4版本以后新增了一个mode选项，是一个必选项。值有development和production两个选项，一个开发模式，一个生产压缩模式

4、使用npm安装webpack，npm i webpack -D

5、安装webpack-cli， npm i webpack-cli -D

6、webpack-dev-server的使用

7、html-webpack-plugin插件：将index.html放到内存中，自动生成index页面的插件

<img src="/Users/yangyang/Library/Application Support/typora-user-images/image-20201021170327420.png" alt="image-20201021170327420" style="zoom:50%;" />









## redux(http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)



redux包含几个部分

​	1、store：保存数据的地方，可以把它看成一个容器。整个应用只能有一个 Store。

​	2、state：一个包含所有数据的对象。通过store.getState（）拿到

```
state = store.getState()

```

​	3、action：定义动作的地方

​	4、reducer：是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。调用动作处理数据放到state里面的。通过store.dispatch（）来实现

​	5、通过store.subscribe监听修改的数据渲染到页面上

```javascript
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
);

unsubscribe();
```



redux通过两个方法跟props绑定

mapStateToProps将stage的值赋值给props

```js
mapStatetoProps= (state)=>{
	return {
    value: state.num
  }
}
```

```js
mapDispatchToProps=()=>{
	return {
    add: ()=>{dispatch(addAction)}
  }
}
```

然后通过connect这个上述两个方法连接到组件上就可以了



