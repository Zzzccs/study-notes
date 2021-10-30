# React学习笔记

## react初体验

```jsx
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="container"></div>

    <!-- 加载 React。-->
    <!-- 注意: 部署时，将 "development.js" 替换为 "production.min.js"。-->
    <script
      src="https://unpkg.com/react@16/umd/react.development.js"
      crossorigin
    ></script>
    <script
      src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"
      crossorigin
    ></script>
    <script src="./babel.min.js"></script>
    <!-- 加载我们的 React 组件。-->
    <script type="text/babel">  // 在浏览器中将babel解析成可执行的代码
      const VDOM = <h1 id="test">React</h1>;  // 注意这里是不需要加引号的，jsx特有的写法
      ReactDOM.render(VDOM, document.getElementById("container"));  // 将VDOM渲染到指定位置
    </script>
  </body>
</html>

```

**引入react文件后，有了React和ReactDOM 2个全局变量，可以通过React创建VDOM，ReactDOM 渲染真实DOM**

`const VDOM = <h1 id="test">React</h1>`  ==`React.createElement('h1',{id: 'test'},'React')`  



## 虚拟DOM优点

1. 虚拟DOM其实就是用对象模拟DOM节点，操作虚拟DOM比操作真实DOM性能开销小很多，因为每次操作真实DOM都会触发重绘回流，虚拟DOM可以将多次操作合并为一次，减少重绘回流次数

2. 虚拟DOM的体积比真实DOM体积小很多

3. 虚拟DOM最终会渲染成真实DOM呈现在页面上

   ![image-20210926213411235](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210926213411235.png)

   

## JSX

JSX：js XML，类似XML的js扩展语法，本质是React.createElement()的语法糖，特点是简化创建虚拟DOM。



XML早期用于存储数据，缺点：标签所占体积比所需存储的数据体积还大，浪费空间

目前 微信公众号服务器和微信开发者服务器消息传递使用的就是XML

```xml
<student>
	<name>Zzzccs</name>
	<age>18</age>
</student>
```

如今主流使用JSON存储数据，体积较小，更通用，强力API：JSON.stringfy，JSON.parse

```json
{"name": "Zzzccs", "age": 18}
```



### JSX语法规则

1. 定义虚拟DOM时，不需要加引号
2. 标签中想使用js表达式需要用{}包起来 `<h1 id = {myId}>{message}</h1>`， 注意与vue不同的是，标签属性用变量时，两边没有引号，并且react是一对括号
3. 虚拟DOM里标签的`class`属性必须写成`className` ，`<h1 className = "class1">{message}</h1>`，为了与ES6的class类区分开来
4. 虚拟DOM写内联样式style时，需要用两对括号包起来 `<h1 style = {{color: '20px'}}>{message}</h1>`，原因是：最外层括号表示里面将会写js表达式，内层括号表示style的值都是以key:value形式存储的对象
5. 只能有一个根标签
6. 标签必须闭合
7. React组件名首字母必须大写，若是小写，react会将其认为是html标签



## JSX遍历数组元素

想要在ul中遍历data数组的元素，每个元素作为一个li标签

```jsx
const data = ["Vue", "Angular", "React"];
<ul>
	{data.map((item, index) => {
		return <li key={index}>{item}</li>;  //注意此处返回的是将数组元素处理成XML标签的结果
	})}
</ul>
```



## 组件

简单组件和复杂组件区别就是有无state

### 函数式组件（定义简单组件）

注意点：

1. 函数式组件就是用函数定义的组件，返回值是XML标签形式
2. 函数名第一个字母必须要大写（JSX语法规则第7条）
3. 调用ReactDOM.render渲染时，`type`里写的是组件标签（必须闭合）

```jsx
function MyComponent1() {
	return (
    	<h1>
            Func Component1
            {MyComponent2()}
        </h1>
    );
}
function MyComponent2() {
	return (
    	<div>
            <h2>Func Component2</h2>
            <input />
        </div>
    );
}
ReactDOM.render(<MyComponent1 />, document.getElementById("container"));
```

render函数执行时发现虚拟DOM是组件，并且他是一个函数式组件，所以他会去执行这个函数并将返回值更新到页面上





### 类式组件（定义复杂组件）

#### 类式组件定义：

1. 必须继承React.Component
2. 必须有render函数，render函数必须有返回值

```jsx
class MyComponent extends React.Component {
        render() {
          return (
            <div className="div1">
              <h1>我是h1</h1>
              <span>span1</span>
            </div>
          );
	    }
}
ReactDOM.render(<MyComponent />, document.getElementById("container"));
```

render函数执行时发现虚拟DOM是组件，并且他是一个类式组件，所以他会new一个类的实例，并调用实例的render方法，将render方法的返回值更新到页面上



### 类式组件和函数式组件内部this指向

类式组件：

![image-20210928103411145](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210928103411145.png)

函数式组件：![image-20210928103538628](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210928103538628.png)



**解释：**

**函数式组件：经过babel编译后，开启了严格模式。`ReactDOM.render`函数遇到了函数式组件，将其执行`MyCompoent()`（这种方式在严格模式下会丢失this，并不会指向window，因此this打印undefined），并将返回值呈现到页面上**

**类式组件：ReactDOM.render函数遇到了类式组件，创建一个类的实例，并调用这个类的render函数（程序员定义在原型上的render函数），因此他的this会指向类的实例，也就是组件实例，并将返回值呈现到页面上**





**我们强调的组件实例其实指的就是类式组件的实例，因为函数式组件没有this指向(严格模式下)，所以下面的几个属性都不考虑函数式组件，但函数式组件拥有props属性，可以通过函数的参数传入。**

### 组件实例的state属性

state保存的是组件实例的状态，每个实例状态都不相同，当state里的值发生改变时，会调用他的render方法重新渲染页面。

当要修改state的值时，需要通过`this.setState({key: value})`才能响应式修改state的值，通过`this.state.key = newval`的方式无法实现响应式。

```jsx
class MyComponent extends React.Component {
        state = { text: 1 };  // 将state属性挂载到实例上
        render() {
          return (
            <div className="div1">
              <h1 onClick={this.change}>
                今天天气很{this.state.text ? "炎热" : "凉爽"}
              </h1>
            </div>
          );
        }
        // 必须要用赋值语句+箭头函数的方式才可以将这个方法放到实例上
        // 赋值号可以让这个属性是在实例上而不是在原型上，如render的定义就是在原型上
        // 箭头函数的作用是为了能正确访问到this，因为这个函数是作为回调函数执行的，即他是通过change()
        // 而不是实例.change()，所以他的this绑定会错误，因此需要用箭头函数
        change = () => {
          this.setState({ text: Number(!this.state.text) });
        };
}
```



#### setState两种写法

```jsx
state = {count: 0}

// 1. 传入参数是一个对象
this.setState({
    count: 10  // ok
	count: this.state.count+1  // wrong
})

// 2. 传入参数是一个函数，函数返回值是一个对象
this.setState((state, props)=> ({
	count: state.count + props.addCount  // ok
}))

/**
 **		出于性能考虑，React 可能会把多个 setState() 调用合并成一个调用。
 **		因为 this.props 和 this.state 可能会异步更新，所以你不要依赖他们的值来更新下一个状态。
 **		
 **		两种方法区别：第一种方法在state异步更新时可能会出错，因此需要用第二种方法
 **/


```



### 组件实例的props属性

props是只读的，为了保持数据单向流

传入组件标签的attribute属性都会被转化成props，可以在组件render函数里通过`this.props.xxx`获取

```
class Person extends React.Component {
        render() {
          return (
            <ul>
              <li>姓名{this.props.person.name}</li>
              <li>年龄{this.props.person.age}</li>
              <li>性别{this.props.person.sex}</li>
            </ul>
          )
        }
      }
      
      let vdom = (
        <div>
            <Person person = {{name:'1', age: 18, sex: '1'}}/> // 手动给person属性赋值
            <br />
            <Person person = {{name:'0', age: 20, sex: '0'}}/>
        </div>
      )
```



简化方法：通过扩展运算符将p的属性全部传入props

![image-20210929110119436](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210929110119436.png)



#### prop类型限制和默认值

在类式组件里定义prop类型限制和默认值都需要在前面加上static关键字

```jsx
class Person extends React.Component {
		// 设置默认值
        static defaultProps = {
          sex: "人",
        };
        // 设置类型限制
        static propTypes = {
          age: PropTypes.number,  // 注意这里首字母是大写，跟上面一行的区别
          name: PropTypes.string.isRequired,
        };
        render() {
          console.log(this);
          return (
            <ul>
              <li>姓名{this.props.person.name}</li>
              <li>年龄{this.props.person.age}</li>
              <li>性别{this.props.person.sex}</li>
            </ul>
          );
        }
      }
```



### 组件实例的refs属性

1. 字符串形式，ref为一个字符串

   缺点：字符串形式运行效率低

```jsx
// 设置ref标识，然后可以在当前实例的refs属性里找到它，它是真实DOM，可以进行操作
<input type="text" ref="ip1" />

showData = () => {
	console.log(this.refs.ip1.value);
};
```

2. 回调函数形式

   缺点：当这个组件实例更新时(再次执行render)，这个ref回调函数会执行两次，其中第一次执行时的参数为null，第二次执行的参数是DOM节点，原因是React更新组件时会重新创建ref函数，但这个缺点并不会引起什么大问题

```jsx
// ref是一个回调函数，回调函数的参数是当前真实DOM节点
<input type="text" ref={(currentNode)=>{this.input1 = currentNode}} />

showData = () => {
	console.log(this.refs.input1.value);
};
```

3. React.createRef形式

注意：一个ref容器只能放一个DOM节点，相同的ref会保存最后的节点

```jsx
class Person extends React.Component {
        IpRef = React.createRef(); // 创建Ref容器
        click = () => {
          console.log(this.IpRef.current); // 获取真实节点
        };
        render() {
          return (
            <div className="c" ref={this.IpRef}> // 绑定到Ref容器上
              <button onClick={this.click}>输出</button>
            </div>
          );
        }
}
```



#### Ref三种方法区别：

第一种方法的DOM节点保存在this.refs.xxx，用法最简单，但效率上可能会较差

第二种方法的DOM节点保存在this.xxx，用法较麻烦

第三种方法的DOM节点保存在this.xxx.current，用法相对简单，建议用这种方法



## 事件处理

react对原生事件都做了一层封装，所以必须用onClick，onBlur的小驼峰方式监听事件

实际上，底层都用了事件委托方式进行监听



## 生命周期

旧版：

![image-20211001201756983](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211001201756983.png)

生命周期的三个阶段（旧）
	1. 初始化阶段: 由ReactDOM.render()触发---初次渲染
	
			1.constructor()
	​		2.componentWillMount()
	​		3.render()
	​		4.componentDidMount()

 2. 更新阶段: 由组件内部this.setSate()或父组件重新render触发
           1.shouldComponentUpdate()
        	   2.componentWillUpdate()
        	   3.render()
        	   4.componentDidUpdate()

 3. 卸载组件: 由ReactDOM.unmountComponentAtNode()触发
           1.componentWillUnmount()





新版:

![image-20211002101541731](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211002101541731.png)



1. 初始化阶段: 由ReactDOM.render()触发---初次渲染
	1.constructor()
	2.getDerivedStateFromProps 
	3.render()
	4.componentDidMount()
2. 更新阶段: 由组件内部this.setSate()或父组件重新render触发
      1.getDerivedStateFromProps
      2.shouldComponentUpdate()
      3.render()
      4.getSnapshotBeforeUpdate
      5.componentDidUpdate()
3. 卸载组件: 由ReactDOM.unmountComponentAtNode()触发
      1.componentWillUnmount()



### 重要的勾子

1. render：初始化渲染或更新渲染调用
2. componentDidMount：开启监听, 发送ajax请求
3. componentWillUnmount：做一些收尾工作, 如: 清理定时器



### 即将废弃的勾子

1. componentWillMount
2. componentWillReceiveProps
3. componentWillUpdate

现在使用会出现警告，下一个大版本需要加上`UNSAFE_`前缀才能使用，以后可能会被彻底废弃，不建议使用。



## 状态提升

**这个概念的由来是因为react保持严格的单向数据流，不像vue可以通过$emit将状态返回。**

**当不同组件需要通信时，就需要将某些状态提升到他们的最近公共父组件，存放到父组件的state里，并将修改状态的事件处理函数作为prop传入子组件，当子组件需要修改状态时，通过this.props.xxx调用父组件的事件处理函数，从而达到修改父组件数据的目的，并且这个状态传入多个子组件时，就会引起其他子组件的重新渲染。因此达到了兄弟组件通信的目的**

例子可看官方文档



## react的"eventbus"

和vue类似的，react也有事件总线EventBus的跨组件通信方式

借助`PubSubJS`这个插件库，可以让不同组件进行通信

```
PubSub.publish('名称', argument)			//发布消息
PubSub.subscrib('名称', (msg,argument) => {} )		//订阅消息
PubSub.unsubscrib('名称')			//取消订阅
```



## react里的"slot"

**这个概念听起来比较模糊，其实就相当于vue的slot插槽，即在自定义组件里写入内容，也可以实现插槽的功能**

***原理其实就是jsx中的标签元素其实都是虚拟dom，即用object模拟的节点，因此可以把标签(虚拟dom/object值)通过props传递给其他组件***

### 基本使用

当我们在Search的父组件App组件里，给Search传入一些内容时，这些内容会被放到Search组件的props.children里，我们可以在Search里通过{this.props.children}显示出来。当然，你不想使用也可以

![image-20211005163357052](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211005163357052.png)

下面是通过react devtools查看到的Search组件里的内容，可以看到传入的内容就在children里

![image-20211005163456565](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211005163456565.png)

Search组件中通过{this.props.children}获取并呈现到页面上

![image-20211005163548650](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211005163548650.png)



### 默认插槽

我们可以在子组件中通过判断this.props.children是不是undefined，实现默认插槽功能

实现方法特别简单，只需要在上面的例子的基础上，在Search组件里加个判断条件即可。

![image-20211005164137823](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211005164137823.png)

呈现结果：

![image-20211005164208400](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211005164208400.png)

devtools的结果：

![image-20211005164339031](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211005164339031.png)



### 具名插槽

具名插槽其实也很好实现，上面的例子我们都是将内容写在组件的text节点里![image-20211005164748901](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211005164748901.png)，因此没法实现具名插槽的功能。

**解决方法：我们可以把内容分别放在props的某个属性里，即不要写在组件的text节点里**

App组件：父组件通过两个属性传递给Search组件

![image-20211005165324029](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211005165324029.png)

Search组件：通过this.props.xxx分别取出

![image-20211005165356009](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211005165356009.png)

呈现效果：

![image-20211005165141374](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211005165141374.png)





## Fragments

考虑某种场景，我们需要让一个组件返回多个元素时，就需要用到Fragments。

看下面例子就能明白

```jsx
// 我们想要的效果
/*
	<table>
      <tr>
          <td>Hello</td>
          <td>World</td>
      </tr>
    </table>
*/

class Table extends React.Component {
  render() {
    return (
      <table>
        <tr>
          <Columns />
        </tr>
      </table>
    );
  }
}
class Columns extends React.Component {
    render() {
        return (
        	<React.Fragment> //代替div，他并不会被渲染到页面上
                <td>Hello</td>
                <td>World</td>
            </React.Fragment>
            
            // 更方便的写法：
            <>               //代替div，他并不会被渲染到页面上
                <td>Hello</td>
                <td>World</td>
            </>
        )
    }
}


/*
class Columns extends React.Component {
  render() {
    return (
      <div>  // 因为在jsx中每个组件必须要有一个根标签，所以在<td>外层必须要有个根标签，但是这种做法
        <td>Hello</td> // 破坏了我们想要的效果，所以就可以用React.Fragment代替div
        <td>World</td>
      </div>
    );
  }
}

最后得到的代码： 这明显不是我们想要的效果 
<table>
  <tr>
    <div>  
      <td>Hello</td>
      <td>World</td>
    </div>
  </tr>
</table> 
*/


```

**上述两种方法代替div作为根标签，不会被渲染到页面上。考虑到有种情况需要给根标签加上key时，就必须使用React.Fragment，空标签没办法加上key属性**





# Router

vue和react开发的项目都是spa项目，也有两种不同的路由模式，hash和history，本质上都是一样的路由控制

安装react-router-dom插件实现SPA，里面有两种路由模式，分别是`Browser Router` 和`Hash Router`

react路由有一个缺点：在有多个路由信息时，即使已经匹配上了某个路由，但是还是会继续往下匹配，直到路由全部匹配过一次，这样子效率会非常差。

react-router-dom库里的`Switch`组件可以解决这种问题，用法是Switch组件包裹在路由信息外侧

```
<NavLink className="link" to="/add" >Todolist</NavLink>
<NavLink className="link" to="/hello">Hello</NavLink>
 
<Switch>
	<Route path="/add" component={Todolist}></Route>
	<Route path="/hello" component={Hello}></Route>
</Switch>
```



路由跳转标签：`<Link></Link>`和`<Navlink></Navlink>` 区别：

1. `Navlink`可以设置高亮效果，当点击某个`Navlink`标签时，他会加上一个`active`的类，我们可以修改这个类以达到我们想要的高亮效果
2. Link标签不会加上`active`的类，没办法实现高亮



## 路由基本使用

![image-20211007160140580](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211007160140580.png)

使用router库的标签时，需要在外部包一层`Browser Router` 或者`Hash Router`作为父标签，但是每使用一次router库的标签就包一层父标签存在的问题：

1. 每个`Browser Router` 或者`Hash Router`就相当于一个唯一的路由器，两个Router的状态都是独立的
2. 每次都需要加父标签，代码太冗长

解决方法：在index.js里对App根组件进行包裹，这样子里面的路由就会共享同个路由器的状态，而且可以减少代码量



## 路由组件与一般组件

		1.写法不同：
					一般组件：<Demo/>
					路由组件：<Route path="/demo" component={Demo}/>
		2.存放位置不同：
					一般组件：components
					路由组件：pages
		3.接收到的props不同：
					一般组件：写组件标签时传递了什么，就能收到什么
					路由组件：接收到三个固定的属性
										history:
													go: ƒ go(n)
													goBack: ƒ goBack()
													goForward: ƒ goForward()
													push: ƒ push(path, state)
													replace: ƒ replace(path, state)
										location:
													pathname: "/about"
													search: ""
													state: undefined
										match:
													params: {}
													path: "/about"
													url: "/about"


## 解决多级路径刷新页面样式丢失的问题

			1.public/index.html 中 引入样式时不写 ./ 写 / （常用）
			2.public/index.html 中 引入样式时不写 ./ 写 %PUBLIC_URL% （常用）
			3.使用HashRouter
![image-20211008103730476](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211008103730476.png)



## 路由的严格匹配与模糊匹配

**默认是模糊匹配**，如图所示，跳转的链接(to属性)包括了匹配链接(path属性)，匹配成功

![image-20211008104257122](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211008104257122.png)

反过来就匹配不上，to属性必须包括path属性

![image-20211008104419920](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211008104419920.png)

			1.默认使用的是模糊匹配（简单记：【输入的路径】必须包含要【匹配的路径】，且顺序要一致）
			2.开启严格匹配：<Route exact={true} path="/about" component={About}/>
				 简便写法：<Route exact path="/about" component={About}/>
			3.严格匹配不要随便开启，需要再开，有些时候开启会导致无法继续匹配二级路由



## 重定向

从react-router-dom引入Redirect后，在路由信息里最下面使用。

当之前的路由信息都没匹配上时就使用重定向，注意的是Redirect标签里的属性是to不是path。

![image-20211008110345344](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211008110345344.png)



## 嵌套路由

```
1.注册子路由时要写上父路由的path值
2.路由的匹配是按照注册路由的顺序进行的
3.父路由不能开启exact严格模式，否则无法进行匹配
```



## 向路由组件传递参数

			1.params参数 (需要声明路由)
						路由链接(携带参数)：<Link to='/demo/test/tom/18'}>详情</Link>
						注册路由(声明接收)：<Route path="/demo/test/:name/:age" component={Test}/>
						接收参数：this.props.match.params
			2.search参数 (需要用到querystring库进行解析数据)
						路由链接(携带参数)：<Link to='/demo/test?name=tom&age=18'}>详情</Link>
						注册路由(无需声明，正常注册即可)：<Route path="/demo/test" component={Test}/>
						接收参数：this.props.location.search
						备注：获取到的search是urlencoded编码字符串，需要借助querystring解析
			3.state参数 (当传递的参数想要隐藏，不暴露给用户时使用)
						路由链接(携带参数)：<Link to={{pathname:'/demo/test',state:{name:'tom',age:18}}}>详情</Link>
						注册路由(无需声明，正常注册即可)：<Route path="/demo/test" component={Test}/>
						接收参数：this.props.location.state
						备注：刷新也可以保留住参数



## 路由开启replace模式

默认是push模式，即每次跳转都会留下记录，方便前进后退

给跳转标签增加replace属性即可开启

![image-20211008161514916](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211008161514916.png)



## 编程式路由导航

当某些特定场景没办法通过跳转标签实现时，就需要用到编程式的路由导航，如：等待几秒再跳转，点击图片跳转等等

所有标签跳转能实现的功能，编程式的路由导航也能实现，并且更加灵活。

				借助this.prosp.history对象上的API对操作路由跳转、前进、后退
						-this.prosp.history.push()
						-this.prosp.history.replace()
						-this.prosp.history.goBack()
						-this.prosp.history.goForward()
						-this.prosp.history.go()



## BrowserRouter与HashRouter的区别

		1.底层原理不一样：
					BrowserRouter使用的是H5的history API，不兼容IE9及以下版本。
					HashRouter使用的是URL的哈希值。
		2.path表现形式不一样
					BrowserRouter的路径中没有#,例如：localhost:3000/demo/test
					HashRouter的路径包含#,例如：localhost:3000/#/demo/test
		3.刷新后对路由state参数的影响
					(1).BrowserRouter没有任何影响，因为state保存在history对象中。
					(2).HashRouter刷新后会导致路由state参数的丢失！！！
		4.备注：HashRouter可以用于解决一些路径错误相关的问题。



## withRouter

一般组件中如果想要操作路由信息，就需要引入withRouter，让一般组件的props中也拥有路由组件的内容。

引入的withRouter需要传入一个组件作为参数，返回值为处理后的组件

![image-20211008190111442](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211008190111442.png)





# Redux



## 1.求和案例_redux精简版

		(1).去除Count组件自身的状态
		(2).src下建立:
						-redux
							-store.js
							-count_reducer.js
	
		(3).store.js：
					1).引入redux中的createStore函数，创建一个store
					2).createStore调用时要传入一个为其服务的reducer
					3).记得暴露store对象
	
		(4).count_reducer.js：
					1).reducer的本质是一个函数，接收：preState,action，返回加工后的状态
					2).reducer有两个作用：初始化状态，加工状态
					3).reducer被第一次调用时，是store自动触发的，
									传递的preState是undefined,
									传递的action是:{type:'@@REDUX/INIT_a.2.b.4}
	
		(5).在index.js中监测store中状态的改变，一旦发生改变重新渲染<App/>
				备注：redux只负责管理状态，至于状态的改变驱动着页面的展示，要靠我们自己写。


## 2.求和案例_redux完整版

		新增文件：
			1.count_action.js 专门用于创建action对象
			2.constant.js 放置容易写错的type值



## 3.求和案例_redux异步action版

		 (1).明确：延迟的动作不想交给组件自身，想交给action
		 (2).何时需要异步action：想要对状态进行操作，但是具体的数据靠异步任务返回。
		 (3).具体编码：
		 			1).yarn add redux-thunk，并配置在store中
		 			2).创建action的函数不再返回一般对象，而是一个函数，该函数中写异步任务。
		 			3).异步任务有结果后，分发一个同步的action去真正操作数据。
		 (4).备注：异步action不是必须要写的，完全可以自己等待异步任务的结果了再去分发同步action。





## 4.求和案例_react-redux基本使用

			(1).明确两个概念：
						1).UI组件:不能使用任何redux的api，只负责页面的呈现、交互等。
						2).容器组件：负责和redux通信，将结果交给UI组件。
			(2).如何创建一个容器组件————靠react-redux 的 connect函数
							connect(mapStateToProps,mapDispatchToProps)(UI组件)
								-mapStateToProps:映射状态，返回值是一个对象
								-mapDispatchToProps:映射操作状态的方法，返回值是一个对象
			(3).备注1：容器组件中的store是靠props传进去的，而不是在容器组件中直接引入
			(4).备注2：mapDispatchToProps，也可以是一个对象


## 5.求和案例_react-redux优化

			(1).容器组件和UI组件整合一个文件
			(2).无需自己给容器组件传递store，给<App/>包裹一个<Provider store={store}>即可。
			(3).使用了react-redux后也不用再自己检测redux中状态的改变了，容器组件可以自动完成这个工作。
			(4).mapDispatchToProps也可以简单的写成一个对象
			(5).一个组件要和redux“打交道”要经过哪几步？
							(1).定义好UI组件---不暴露
							(2).引入connect生成一个容器组件，并暴露，写法如下：
									connect(
										state => ({key:value}), //映射状态
										{key:xxxxxAction} //映射操作状态的方法
									)(UI组件)
							(4).在UI组件中通过this.props.xxxxxxx读取和操作状态



## 6.求和案例_react-redux数据共享版

			(1).定义一个Pserson组件，和Count组件通过redux共享数据。
			(2).为Person组件编写：reducer、action，配置constant常量。
			(3).重点：Person的reducer和Count的Reducer要使用combineReducers进行合并，
					合并后的总状态是一个对象！！！
			(4).交给store的是总reducer，最后注意在组件中取出状态的时候，记得“取到位”。

## 7.求和案例_react-redux开发者工具的使用

			(1).yarn add redux-devtools-extension
			(2).store中进行配置
					import {composeWithDevTools} from 'redux-devtools-extension'
					const store = createStore(allReducer,composeWithDevTools(applyMiddleware(thunk)))

## 8.求和案例_react-redux最终版

			(1).所有变量名字要规范，尽量触发对象的简写形式。
			(2).reducers文件夹中，编写index.js专门用于汇总并暴露所有的reducer