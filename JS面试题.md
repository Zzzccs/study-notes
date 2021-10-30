# JS面试题

## 1 函数提升和变量提升问题

**函数提升在变量提升之前**

```
// a.
function Foo () {
 getName = function () {
   console.log(1);
 }
 return this;
}
// b.
Foo.getName = function () {
 console.log(2);
}
// c.
Foo.prototype.getName = function () {
 console.log(3);
}
// d.
var getName = function () {
 console.log(4);
}
// e.
function getName () {
 console.log(5);
}

Foo.getName(); //2
getName(); //4
Foo().getName(); //1
getName(); //1
new Foo.getName(); //2 
new Foo().getName(); //3
new new Foo().getName(); //3
```

解析：

1. **Foo.getName();  // 2**

Foo为一个函数对象，对象都可以有属性，题目 b 处定义Foo的getName属性为函数，输出2




2. **getName(); // 4**

这里看d、e处，d为函数表达式，e为函数声明，两者区别在于变量提升，这两处可以等价于


```
var getName = undefined;
// e处提升到顶部
getName = function () {
 console.log(5);
}
 
...
// d. 重新赋值
getName = function () {
 console.log(4);
}
// e.
 

```

可见函数声明的 5 会被后边函数表达式的 4 覆盖。




3. **Foo().getName();  // 1**

这里要看a处，在Foo内部将全局的getName重新赋值为 console.log(1) 的函数，执行Foo()返回 this，这个this指向window，Foo().getName() 即为window.getName()，输出 1。




4. **getName();  //  1**

上述3中，全局的getName已经被重新赋值，所以这里依然输出 1。




5. **new Foo.getName();  // 2**

这里等价于 new (Foo.getName())，先执行 Foo.getName()，输出 2，然后new一个实例；




6. **new Foo().getName(); // 3**

这里等价于 (new Foo()).getName(), 先new一个Foo的实例，再执行这个实例的getName方法，但是这个实例本身没有这个方法，所以去原型链 __ proto __ __上边找，实例__ __ proto  __ === Foo.prototype，所以输出 3。




7. new new Foo().getName(); // 3

这里等价于new (new Foo().getName())，如上述6，先输出 3，然后new 一个 new Foo().getName() 的实例。

补充：

关于上述 5中 new Foo.getName()先执行 Foo.getName()，而6中 new Foo().getName() 先执行 new Foo()，是因为：

new Foo() 属于new（带参数列表）
new Foo属于new（无参数列表）
无参数列表的优先级为18，而成员访问的优先级为19，高于无参数列表。因此new Foo.getName()先执行Foo.getName()

带参数列表的优先级为19，而成员访问的优先级也为19，按照运算符规则（同一优先级，按照从左向右的执行顺序），new Foo().getName()先执行new Foo()，再对new之后的实例进行成员访问.getName()操作。
这是js运算符的优先级链接，可查看每个运算符的优先级。



## 2 函数形参问题

 **1 函数形参形式func1(a, b, c)，此时a,b,c分别和arguments[0],arguments[1],arguments[2]指向了同一个内存地址**

**2 函数形参形式func2(a, b, c=3)，此时a,b,c分别和arguments[0],arguments[1],arguments[2]没有任何关联**



```
function side(arr) {
            arr[0] = arr[2];
            arr[1] = 10
        }
function func1(a, b, c) { //此时abc就是arguments[0,1,2]
            console.log(arguments);  //1,1,1
            a = 2;
            c = 10;
            console.log(a, b, c); //2,1,10
            console.log(arguments); //2,1,10
            side(arguments);
            console.log(arguments); //10,10,10
            console.log(a, b, c); //10,10,10
            console.log(a + b + c); //30
        }
        func1(1, 1, 1);
        
        
function func2(a, b, c = 3) { //此时abc和arguments[0,1,2]是互不干扰的，arguments[0,1,2]赋了abc的初值
            console.log(arguments); //1,1,1
            a = 2;
            c = 10;
            console.log(a, b, c); //2,1,10
            console.log(arguments); //1,1,1
            side(arguments);
            console.log(arguments); //1,10,1
            console.log(a, b, c); //2,1,10
            console.log(a + b + c); //13
        }
        func2(1, 1, 1);
```



# 防抖和节流

**// 如果事件触发是高频但是有停顿时，可以选择debounce； 在事件连续不断高频触发时，只能选择throttling，因为debounce可能会导致动作只被执行一次，界面出现跳跃。**

```javascript
// 防抖:使用场景：多次点击事件、快速滑动滚动条、输入文字自动补全等等

// 可以在行为的最后一次触发时执行任务，减少任务的执行次数

function debounced(func, delay) {

  let timer = null;

  return function(...args) {

    if(timer) clearTimeout(timer);

    timer = setTimeout(()=> {

      func.apply(this,args)

    },delay)

  }

}



// 节流：使用场景：多次请求数据时

// 可以在一段周期时间內只执行一次

function throttle(func,delay){

  let valid = true //使用状态位判断此时有没有在执行

  return function(...args) {

    if(!valid){

      //休息时间 暂不接客

      return false 

    }

    // 工作时间，执行函数并且在间隔期内把状态位设为无效

    valid = false

    setTimeout(() => {

      func.apply(this,args)

      valid = true;

    }, delay)

  }

}
```



# 处理JS中0.1+0.2!=0.3

```js
function judge(a,b) {
    return Math.abs(a-b)<Number.EPSILON
}
console.log(judge(0.1+0.2,0.3)); //true
console.log(0.1+0.2==0.3); //false
```



# 函数的声明方法

**函数声明有三种方式：函数声明，函数表达式（又称函数字面量声明），函数对象的声明（使用率很低）**

**方式一：函数声明**
function  函数名(形参列表) {
//方法体 
}
 **方式二：函数表达式（又称函数字面量声明）**
       var  变量名=function 函数名(形参列表){  备注：这个函数名有点特别，它只能在它自己这个函数体内被调用。不能被外部访问，有兴趣的自己测试测试。
//方法体
}
  **方式三：函数对象的声明**
        var  方法名 =new Function("形参1","形参2","形参3", "方法体");



## **只有函数声明才会函数声明提升，其他两种不会函数声明提升。**



# 事件循环



## 宏任务和微任务：





# []和{}的面试题

==：双等号比较时会**把两边转换为Number类型再进行比较**

===: 比较时**不会转换类型，直接比较值是否相等**



对象的==比较方式:
  *   1.调用.valueOf()方法，不是基本数据类型，则进行下一步，若是基本类型则直接第三步
  *   2.调用.toString()，返回的是""，是基本数据类型，进行下一步
  *   3.把基本数据类型通过Number函数转换为Number类型，最后进行比较。

```
console.log([]==false) //true
/**
  *   1.[].valueOf()，返回"[]"
  *   2.[].toString()，返回""，空字符串
  *   Number("")=0，Number(false)==0，因此[]==false
 **/
 
console.log({}==false) //false
/**
  *   1.{}.valueOf()，返回"{}",不是基本数据类型
  *   2.{}.toString()，返回一个Obeject对象，不是基本类型
  *   3.Number({})=NaN,Number("false")=0。因为NaN!=0，所以{}!=false
 **/
  
console.log([]?true:false) //true
console.log({}?true:false) //true
```

