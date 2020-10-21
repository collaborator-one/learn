# react项目总结

1.子组件不能跳转报错Cannot read property ‘push’ of undefined，这是没有把history传给子组件，
<BottomBar  history={this.props.history}/>

需要这样把history传给子组件才能进行路由跳转

https://blog.csdn.net/weixin_37865166/article/details/89477489





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









