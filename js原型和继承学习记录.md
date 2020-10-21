js原型和继承学习记录https://www.bilibili.com/video/BV17J411y7XZ?p=19

可以创建没有父亲的对象，通过object.creat(父亲，对象内容)创建对象，如果吧父亲设置成null，就创建了一个纯字典对象，没有父级

__proto__是函数对象的父级，实例化对象的跟构造函数共用的父级是prototype

<img src="/Users/yangyang/Library/Application Support/typora-user-images/image-20201019100715103.png" alt="image-20201019100715103" style="zoom:50%;" />

<img src="/Users/yangyang/Library/Application Support/typora-user-images/image-20201019100745092.png" alt="image-20201019100745092" style="zoom:50%;" />

构造函数的关系是，User.prototype == hd.__proto__

__proto__是new这个对象的时候使用的，prototype是实例化这个对象，给实例化对象使用的，prototype相当于给构造函数本身添加方法，供实例化对象直接调用

__proto__其实就是对象的getter和setter，而且object.__proto__ = object只能是对象方法，不能是字符串、数字一类的，如果想设置非方法对象，只能通过object.creat（null）这样，不继承object，这样。__proto__就不是setter和getter了，就可以随便赋值了



object.isPrototypeOf(a)检查a对象是否继承自object

hasOwnProperty和in可以检测对象属性

call和apply其实就是相当于对象借用，通过call和apply借用别的对象的方法。举个例子：如果我们要筛选dom结构中有属性是class的dom元素，但是元素没有过滤的方法，这个时候可以借用数组的过滤方法实现

<img src="/Users/yangyang/Library/Application Support/typora-user-images/image-20201019144532643.png" alt="image-20201019144532643" style="zoom:50%;" />

super = this.__proto__。 当前类的原型