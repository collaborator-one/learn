首先diff算法的改进，从以前的逐层对比，深度对比，变成标记对比，只比较变化的数据，提高性能

然后解藕业务逻辑，通过setup将不同的业务逻辑节藕出去

setup的执行时机是在beforecreated之前，所以在setup函数中不能使用data中的数据和methods里面的方法，setup中的this是underfined，而且只能是同步的不能是异步的

ref和reactive是vue对简单数据和复杂数据改变是渲染ui的方法

```js
let state = ref(0)//number, string, boolean
let state1 = reactive([])//json, arr,
```

reactive是基于proxy实现的

reactive只能更新json/arr，如果是别的数据类型的话，比如时间对象，只能通过重新赋值的方式去修改页面的值

ref的本质还是reactive，当我们设置ref的值的时候，底层会换转成reactive，，ref（18）-> reactive（{value: 18}）,所以要更新ref的值的时候，需要用state.value来更新，模版中不需要这样，直接用{{state}}就行了。shallowref和shallowreactive也是一样的，shallowref（10）->shallow reactive({value: 10})

ref和reactive的区别就是在模版中，ref生成的数据不需要对象点属性的方式调用，但是reactive需要用对象点属性的方式调用，原理是通过看这个数据是否是ref类型的，如果是的话，会有个__v_ref=true这个属性判断的

ref和reactive这两个方法都对数据结构都是深层监听的（递归监听），也就是说一个对象有多少层就监听多少层，每一层都包装成一个proxy对象，这也造成了非常消耗性能。vue3还提供了两个只监听第一层的方法。就是shallowref和shallowreactive（非递归监听）。首先shallowreactive这个方法只监听对象的第一层，如果不修改第一层的值，修改了里面的值，页面不会渲染，因为只监听第一层的值，之后第一层的值改变，才能渲染页面的时候渲染深层数据的改变，shallowref这个监听的第一层不是obj.value这一层，而vue3监听的是obj.value这一层，所以vue3提供了一个triggerref方法修改数据渲染页面，一般情况用递归监听可以的，如果数据量特别大的时候，用非递归监听

toRaw()方法是获取ref/reactive原始数据的方法，因为ref/reactive方法每次修改都被跟踪，更新UI界面，非常消耗性能，如果有一些数据操作，不需要追踪，就可以用toRaw方法拿到原始数据，对数据进行操作，性能就提升了。因为vue监听的是ref类型的.value，也就是说ref类型数据的原始数据是在.value中，所以，toRaw(state.value)这样才能拿到ref数据的原始类型

markRaw方法

<img src="/Users/yangyang/Library/Application Support/typora-user-images/image-20201109150104695.png" alt="image-20201109150104695" style="zoom:50%;" />

这个方法就是告诉vue这个obj数据不需要追踪更新UI，就算后续变成响应式数据都不会追踪

如果有一个对象obj，想把obj其中某个属性变成响应式的

```js
let obj = {name: 'zs'}
let state = ref(obj.name)
state.value = 'int'
//这个时候，原始数据没有改变，相应数据变成int,而原始数据还是{name: 'zs'}，，，这也是变相的深拷贝
```

如果利用toRef（）对上述例子修改，

```js
let state = toRef(obj, 'name')
state.value = 'zs'
//这个时候，原始数据改变，相应数据也改变，但是不更新ui界面，
```

ref和toRef这两个方法通过上述两个例子，可以看成一个是深拷贝，一个是浅拷贝，前者是复制一份原始数据，后者是引用原始数据；前者是数据改变会更新ui界面，后者不会。后者应用场景，如果想让响应式数据跟原始数据关联起来，但是不想更新ui，就可以用toRef这个方法。注意的是toRef这个方法只能接收两个参数，所以引出了toRefs这个方法

<img src="/Users/yangyang/Library/Application Support/typora-user-images/image-20201109152004320.png" alt="image-20201109152004320" style="zoom:50%;" />

toRefs方法原理就是上面注释的两句话，可以给一个对象的多个属性变成响应式的

除了ref和reactive这两个方法，上述的别的方法都是做性能优化的

如何自定义一个ref

```js
import {customRef} from 'vue'
function myref (value){
  return customRef(track, tigger) =>{
    return {
      get() {
        track()//追踪方法
        return value
      },
      set(newValue) {
        value = newValue
        tigger()//触发ui界面改变方法
      }
    }
  }
}
```

自定义ref的使用场景，基本上是为了处理异步数据,例子如下

```js
import {customRef} from 'vue'
function myref (value){
  fetch().then(()=>{
    value = data
    trigger()
  }).catch(()=>{
    
  })
  return customRef(track, tigger) =>{
    return {
      get() {
        track()//追踪方法
        return value
      },
      set(newValue) {
        value = newValue
        tigger()//触发ui界面改变方法
      }
    }
  }
}

//在setup中调用就行了，因为setup只能同步处理，不能处理异步问题
setup() {
  let state = myref('url')
  return {
    state
  }
}
```

通过ref获取dom

readonly方法，用于创建一个只读的数据。不管数据接口怎么复杂，都是只读的，也就是递归只读

shallowReadonly是用于创建一个第一层只读的数据，不是递归只读的

isreadonly，判断是否是只读数据

const和readonly的区别就是，前者是变量保护，不能改变变量，可以改变变量属性，后者是属性保护，不能改变变量的属性

vue3实现一个响应式数据是通过proxy方法，这个只监听第一层，相当于shallowref和shallowreactive，只监听第一层

```js
let obj = {name: 'obj', age: 19}
let state = new Proxy(obj,{
  get(obj,key) {
    return obj[key]
  },
  set(obj,key,value) {
  	obj[key] = value
    return true//如果这个地方不返回true的话，就不会执行下次赋值，因为程序不知道是否赋值成功
	}
})
```







