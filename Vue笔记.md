# Vue实例管理

#### 双向数据绑定：

#### 大胡子语法：**{{ 变量 }}**：{{ message }}

这种语法只能用在标签的content里，不能添加到标签的属性里，比如<xxx xx={{message}}>是错误的，会直接把“{{message}}”作为字符串数据

![image-20201214194019282](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201214194019282.png)

![image-20201214194031789](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201214194031789.png)



Vue：

编程范式：声明式编程

**let app = new Vue(）在括号內传入参数：对象类型{数据，绑定对象等}**

let app = new Vue({

​    el: "#app",  ***注意逗号***     **el用于挂载需要管理的元素element**

​    data: {           **data定义数据项**

​     message: "Zzzccs",

​	 变量:"value"

​    },

   });

原生JS：

编程范式：命令式编程

1.创建div元素，设置id属性

2.定义变量message

3.将message添加到div元素的内容中显示

4.修改message的数据

5.将修改后的数据再次替换到div元素



# Vue指令

## 插值操作

#### v-for用法

**v-for使用时要加上v-bind：key=“xxx”，可以提高数组的插入效率，这样做实际上是把数组里的元素（value）和键值（key）一一对应，形成键值对，在内存中实际成为了链表，所以可以提高插入效率。**

数组的key最好用item（但是数组里元素相同时会报错），对象的key最好用index做到一一对应，即一个数组元素只有一个key与其对应。

![image-20210221170343827](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210221170343827.png)



1. v-for可以**遍历数组或者对象**的值：v-for="item in message"
2. **遍历对象**的key，value：v-for="（**value，key**） in message"。value在前，key在后
3. **遍历对象**的key，value和index：v-for="（**value，key，inedx**） in message"

v-for循环输出in之后的变量数组元素，item作为临时变量替换每个message[0],message[1],message[2]~~message[massage.length-1]

当message数组里的元素发生变化时，页面上的内容也会随着变化（响应式）

```javascript
<li v-for="item in message">{{item }}</li>

```

#### v-on用法

v-on用于监听某个元素的事件

![image-20201214193951457](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201214193951457.png)



**在methods中定义方法，这些方法只能在这个VUE对象里使用，在当前的vue对象內使用 this 关键字可以获取到该对象的一些属性和值**

 格式： 函数名: function(){}

语法糖 @click：相当于v-on:click

![image-20201214193659886](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201214193659886.png)

#### v-once用法

加上v-once的标签的值只能改变一次

![image-20201220154134275](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201220154134275.png)

#### v-html用法

解析html标签并显示在页面上

![image-20201221202423179](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201221202423179.png)

//<h2 v-html="url">这里加什么都没用</h2>

注意在加了v-html指令之后，这个标签內加任何元素都显示不出来，只会显示v-html='xxx'里的内容

#### v-text用法

1. v-text接受的是string类型的数据，不解析html标签而直接输出
2. 与v-html不同的是，v-text是接受什么就输出什么

![image-20201221212946235](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201221212946235.png)

## 动态绑定操作

#### v-bind用法

动态绑定属性值argument

语法糖 v-bind:argument="optional" === :argument="optional"

省略v-bind直接用“ ：”代替

![image-20210127213026173](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210127213026173.png) 	

##### v-bind绑定class有2种语法：

对象语法



数组语法





## 计算属性

**针对Vue实例对象的data数据进行操作**

本质就是一个属性，直接用变量形式调用

![image-20210202144051999](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210202144051999.png)

computed属性跟methods

1.定义方式大同小异，以函数形式定义

2.**计算属性调用时不需要加括号**，**即把其当做一个变量来使用，而methods调用时要以函数的形式调用，需要加括号。**

3.计算属性有缓存，多次调用时指挥执行一次，效率高；methods没有缓存，调用几次执行几次



#### v-show和v-if当条件为false的时候的区别

![image-20210219161725767](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210219161725767.png)

## v-model的使用

用于绑定表单元素，实现数据和表单内容的双向绑定。

![image-20210227164010053](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210227164010053.png)

![image-20210227164037594](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210227164037594.png)

通过修改input里的内容（即value属性）可以直接修改message的值，也可以通过修改message的值改变input里的value属性，是响应式的。

实现原理：

1:v-bind给input动态绑定value属性为message，可以实现 **message变化->输入框内容变化**

2:v-on给input添加输入框内容监听，可以实现 **输入框内容变化->message变化**

![image-20210227165702967](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210227165702967.png)

![image-20210227165710579](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210227165710579.png)

# 组件

## 组件的全局注册和局部注册



### 组件使用v-model

自定义事件也可以用于创建支持 `v-model` 的自定义输入组件。记住：

```
<input v-model="searchText">
```

等价于：

```
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```

当用在组件上时，`v-model` 则会这样：

```
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```

为了让它正常工作，这个组件内的 `<input>` 必须：

- 将其 `value` attribute 绑定到一个名叫 `value` 的 prop 上
- 在其 `input` 事件被触发时，将新的值通过自定义的 `input` 事件抛出

写成代码之后是这样的：

```
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```

现在 `v-model` 就应该可以在这个组件上完美地工作起来了：

```
<custom-input v-model="searchText"></custom-input>
```

# 前端模块化

## 为什么要使用模块化:

**1.实际开发时多人开发的代码合并时变量可能冲突，导致很多问题出现，因此需要引入模块化这一方式来避免变量的冲突**

**2.导入js文件时的顺序会影响到代码的执行，所以这种直接导入的方式不适合实际开发**

![image-20210307084301695](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210307084301695.png)



# 扩展：

## ES6可变参数“...”接受不定数量的参数，存放在数组里

下面的sum函数把参数存放到num数组里，可以用num[i]方式存取参数

![image-20210221172020044](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210221172020044.png)



实现多个单选框只选其一，例如选择性别。方法：给单选框绑定相同的name值

![image-20210227171214663](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210227171214663.png)



# Vue生命周期

**beforeCreate（创建前）:** 在数据观测和初始化事件还未开始,data、watcher、methods都还不存在，但是$route已存在，可以根据路由信息进行重定向等操作。

**created(创建后)：**在实例创建之后被调用，该阶段可以访问data，使用watcher、events、methods，也就是说 数据观测(data observer) 和event/watcher 事件配置 已完成。但是此时dom还没有被挂载。该阶段允许执行http请求操作。

**beforeMount （挂载前）：**将HTML解析生成AST节点，再根据AST节点动态生成渲染函数。相关render函数首次被调用(划重点)。

**mounted (挂载后)：**在挂载完成之后被调用，执行render函数生成虚拟dom，创建真实dom替换虚拟dom，并挂载到实例。可以操作dom，比如事件监听

**beforeUpdate：**vm.data更新之后，虚拟dom重新渲染之前被调用。在这个钩子可以修改vm.data更新之后，虚拟dom重新渲染之前被调用。在这个钩子可以修改*vm*.*data*更新之后，虚拟*dom*重新渲染之前被调用。在这个钩子可以修改vm.data，并不会触发附加的冲渲染过程。

**updated：**虚拟dom重新渲染后调用，若再次修改$vm.data，会再次触发beforeUpdate、updated，进入死循环。

**beforeDestroy：**实例被销毁前调用，也就是说在这个阶段还是可以调用实例的。

**destroyed：**实例被销毁后调用，所有的事件监听器已被移除，子实例被销毁**结论**
一般在，**created，mounted** 中都可以发送数据请求，但是，大部分时候，会在**created发送请求**。
Created的使用场景：如果页面首次渲染的就来自后端数据。因为，此时data已经挂载到vue实例了。
在 created（如果希望首次选的数据来自于后端，就在此处发请求）（只发了异步请求，渲染是在后端响应之后才进行的）、beforeMount、mounted（在mounted中发请求会进行二次渲染） 这三个钩子函数中进行调用。
因为在这三个钩子函数中，data 已经创建，可以将服务端端返回的数据进行赋值。但是最常用的是在 created 钩子函数中调用异步请求，因为在 created 钩子函数中调用异步请求



# Vue路由守卫

![image-20210625093742252](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210625093742252.png)

## 路由守卫分类 

**【1】全局守卫：**是指路由实例上直接操作的钩子函数，特点是所有路由配置的组件都会触发，直白点就是触发路由就会触发这些钩子函数

- beforeEach（to，from， next）
- beforeResolve（to，from， next）
- afterEach（to，from）

**【2】路由守卫：** 是指在单个路由配置的时候也可以设置的钩子函数

- beforeEnter（to，from， next）

**【3】组件守卫：**是指在组件内执行的钩子函数，类似于组件内的生命周期，相当于为配置路由的组件添加的生命周期钩子函数。

- beforeRouteEnter（to，from， next）
- beforeRouteUpdate（to，from， next）
- beforeRouteLeave（to，from， next）


