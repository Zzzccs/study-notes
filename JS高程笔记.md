# script：

## defer属性

给script标签加defer属性可以让脚本延迟执行。

HTML5中规定：**script元素的defer属性只对外部脚本有效，支持HTML5的浏览器会默认忽略行内脚本的defer属性**。

HTML5 规范要求：**脚本应该按照它们出现的顺序执行，因此第一个推迟的脚本会在第二个推迟的脚本之前执行**



## async属性

添加 async 属性的目的是告诉浏览器，不必等脚本下载和执行完后再加载页面。

## async和defer

**从改变脚本处理方式上看，async属性和defer属性类似。并且async属性也只适用于外部脚本，都会告诉浏览器立即开始下载。不过，与 defer 不同的是，标记为 async 的脚本并不保证能按照它们出现的次序执行**

#### 

# 外部脚本的好处

- 可维护性。JavaScript 代码如果分散到很多 HTML 页面，会导致维护困难。而用一个目录保存所有JavaScript 文件，则更容易维护，这样开发者就可以独立于使用它们的 HTML 页面来编辑代码。
- **缓存。浏览器会根据特定的设置缓存所有外部链接的 JavaScript 文件，这意味着如果两个页面都用到同一个文件，则该文件只需下载一次。这最终意味着页面加载更快。**
- 适应未来。通过把 JavaScript 放到外部文件中，就不必考虑用 XHTML 或前面提到的注释黑科技。包含外部 JavaScript 文件的语法在 HTML 和 XHTML 中是一样的。

# 语言基础

## 7种数据类型

JS有**6种简单数据类型（原始类型）**：Undefined、Null、Boolean、String、Number、Symbol（ES6新增）。

**1种复杂数据类型：**Object

## var关键字

var message；定义了一个名为message的变量，可以用来保存任何类型的值。

var message=“hi”，这时候初始化message的值为“hi”，为string类型，但是之后可以修改message为别的类型，执行message=100；typeof message输出number

![image-20210406161311142](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210406161311142.png)

## var作用域

使用 var 操作符定义的变量会成为包含它的函数的局部变量。比如，**使用 var在一个函数内部定义一个变量，就意味着该变量将在函数退出时被销毁**：

![image-20210406161941542](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210406161941542.png)

**不过，在函数内定义变量时省略 var 操作符，可以创建一个全局变量,在函数外也可以使用：**

![image-20210406162042039](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210406162042039.png)



## var变量提升

**变量提升就是把所有变量声明都拉到函数作用域的顶部**

此外，反复多次使用 var 声明同一个变量也没有问题

**var关键字声明的变量会自动提升到函数作用域顶部：**

![image-20210406162739953](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210406162739953.png)

## let关键字

let 跟 var 的作用差不多，但有着非常重要的区别。最明显的区别是，**let 声明的范围是块作用域，而 var 声明的范围是函数作用域。**

块作用域是函数作用域的子集，因此适用于 var 的作用域限制同样也适用于 let。

![image-20210406163346707](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210406163346707.png)![image-20210406163353955](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210406163353955.png)

let 也不允许同一个块作用域中出现冗余声明。这样会导致报错：

![image-20210406163517486](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210406163517486.png)

### 1.暂时性死区

**在解析代码时，JavaScript 引擎也会注意出现在块后面的 let 声明，*只不过在此之前不能以任何方式来引用未声明的变量*。*在 let 声明之前的执行瞬间被称为“暂时性死区”*（temporal dead zone），在此阶段引用任何后面才声明的变量都会抛出 ReferenceError。**

**let 与 var 的另一个重要的区别，就是 let 声明的变量不存在变量提升**

![image-20210406164514239](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210406164514239.png)



> 在解析代码时，JavaScript 引擎也会注意出现在块后面的 let 声明，*只不过在此之前不能以任何方式来引用未声明的变量*。

![image-20210406165601596](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210406165601596.png)

上面的图可以解释这段话：**第二个log会报错是因为JS引擎注意到了if块中有let name，因此在声明之前无法使用。若是JS引擎无法注意到let name，则第二个log会输出123（但结果并不是这样，说明JS引擎能够注意到let和var定义的变量）**

### 2.全局声明

**与 var 关键字不同，使用 let 在全局作用域中声明的变量不会成为 window 对象的属性（var 声明的变量则会）。**

**不过，let 声明仍然是在全局作用域中发生的，相应变量会在页面的生命周期内存续。因此，为了**
**避免 SyntaxError，必须确保页面不会重复声明同一个变量。**

![image-20210406170327572](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210406170327572.png)

### 3.条件声明

**在使用 var 声明变量时，由于声明会被提升，JavaScript 引擎会自动将多余的声明在作用域顶部合并为一个声明。因为 let 的作用域是块，所以不可能检查前面是否已经使用 let 声明过同名变量，同时也就不可能在没有声明的情况下声明它。**

![image-20210406171606362](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210406171606362.png)

## const关键字

const 的行为与 let 基本相同，唯一一个重要的区别是用它**声明变量时必须同时初始化变量**，且**尝试修改 const 声明的变量会导致运行时错误**。

**const 声明的限制只适用于它指向的变量的引用。换句话说，*如果 const 变量引用的是一个对象，那么修改这个对象内部的属性并不违反 const 的限制。***

虽然 const 变量跟 let 变量很相似，但是不能用 const 来声明迭代变量（因为迭代变量会自增）



## typeof操作符

typeof是一个操作符而不是函数。

***：“typeof null” 返回的是：“object”。因为特殊值 null 被认为是一个对空对象的引用**

*：**严格来讲，函数在 ECMAScript 中被认为是对象，并不代表一种数据类型。可是，函数也有自己特殊的属性。为此，就有必要通过 typeof 操作符来区分函数和其他对象。即用typeof可以返回function“”**

***：对一个未初始化的变量进行typeof会返回“undefined”，而对一个未声明的变量进行typeof也会返回“undefined”**



## var,let,const区别

**ES6规定，var 命令和 function 命令声明的全局变量，依旧是顶层对象的属性，但 let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。**

```javascript
var a = 12;
function f(){};

console.log(window.a); // 12
console.log(window.f); // f(){}

//let，const在全局环境下定义的变量不能用window来获取，而需要直接输出
let aa = 1;
const bb = 2;

console.log(window.aa); // undefined
console.log(window.bb); // undefined
console.log(aa); // 1
console.log(bb); // 2
```







## null类型

Null 类型同样只有一个值，即特殊值 null。**逻辑上讲，null 值表示一个空对象指针，这也是给typeof 传一个 null 会返回"object"的原因**

**undefined 值是由 null 值派生而来的，因此 ECMA-262 将它们定义为表面上相等：**
**console.log(null == undefined); // true**



## Boolean类型

**true 不等于 1，false 不等于 0**

虽然布尔值只有两个，但所有其他 ECMAScript 类型的值都有相应布尔值的等价形式。要将一个其他类型的值转换为布尔值，可以调用特定的 Boolean()转型函数：

let message = "Hello world!"; 

let messageAsBoolean = Boolean(message);  //messageAsBoolean===true

![image-20210413174936951](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210413174936951.png)



## Number类型

### 1.浮点值

要定义浮点值，**数值中必须包含小数点，而且小数点后面必须至少有一个数字**。虽然小数点前面不是必须有整数，但推荐加上。



**因为存储浮点值使用的内存空间是存储整数值的两倍，所以 ECMAScript 总是想方设法把值转换为整数。在小数点后面没有数字的情况下，数值就会变成整数。类似地，如果数值本身就是整数，只是小数点后面跟着 0（如 1.0），那它也会被转换为整数**

let floatNum1 = 1. ; // 小数点后面没有数字，当成整数 1 处理

let floatNum2 = 10.0; // 小数点后面是零，当成整数 10 处理



对于非常大或非常小的数值，浮点值可以用科学记数法来表示。ECMAScript 中科学记数法的格式要求是一个数值（整数或浮点数）后跟一个大写或小写的字母 e，再加上一个要乘的 10 的多少次幂。

**在IEEE754数值中，0.1+0.2=0.300 000 000 000 000 04。**



### 2.值的范围

ECMAScript 可以表示的最小数值保存在 Number.MIN_VALUE 中，这个值在多数浏览器中是 5e-324；可以表示的最大数值保存在

Number.MAX_VALUE 中，这个值在多数浏览器中1.797 693 134 862 315 7e+308。**如果某个计算得到的数值结果超出了 JavaScript 可以表示的范围，那么这个数值会被自动转换为一个特殊的 Infinity（无穷）值。任何无法表示的负数以-Infinity（负无穷大）表示，任何无法表示的正数以 Infinity（正无穷大）表示。**

如果计算返回正 Infinity 或负 Infinity，则该值将不能再进

一步用于任何计算。这是因为Infinity 没有可用于计算的数值表示形式。**要确定一个值是不是有限大（即介于 JavaScript 能表示的最小值和最大值之间），可以使用 isFinite()函数**



### 3.NaN

“Not a Number”用于表示本来要返回数值的操作失败了（而不是抛出错误）。

比如，用 0 除任意数值在其他语言中通常都会导致错误，从而中止代码执行。但在 ECMAScript 中，0、+0 或0 相除会返回 NaN

#### **NaN独特的属性：**

1.首先，**任何涉及 NaN 的操作始终返回 NaN**（如 NaN/10），在连续多步计算时这可能是个问题。

2.其次，**NaN 不等于包括 NaN 在内的任何值**。例如，下面的比较操作会返回 false：
console.log(NaN == NaN); // false



#### **isNaN()函数**：

该函数接收一个参数，可以是任意数据类型，然后判断这个参数**是否“不是数值”**。把一个值传给 isNaN()后，该函数会尝试把它转换为数值。某些数值的值可以直接转换成数值，如字符串"10"或布尔值。**任何不能转换为数值的值都会导致这个函数返回true**（带英文字母的都返回true）。举例如下：

console.log(isNaN(NaN)); // true 

console.log(isNaN(10)); // false，10 是数值

console.log(isNaN("10")); // false，可以转换为数值 10 

console.log(isNaN("blue")); // true，不可以转换为数值

console.log(isNaN(true)); // false，可以转换为数值 1 



### 4.数值转换

**有 3 个函数可以将非数值转换为数值：Number()、parseInt()和 parseFloat()。**

Number()是转型函数，可用于任何数据类型。后两个函数主要用于将字符串转换为数值。对于同样的参数，这 3 个函数执行的操作也不同。

- **Number()函数基于如下规则执行转换。**

   布尔值，true 转换为 1，false 转换为 0。 

   数值，直接返回。
   null，返回 0。 

   undefined，返回 NaN。 

   字符串，应用以下规则。
   如果字符串包含数值字符，包括数值字符前面带加、减号的情况，则转换为一个十进制数值。
  因此，Number("1")返回 1，Number("123")返回 123，Number("011")返回 11（忽略前面的零）。
   如果字符串包含有效的浮点值格式如"1.1"，则会转换为相应的浮点值（同样，忽略前面的零）。
   如果字符串包含有效的十六进制格式如"0xf"，则会转换为与该十六进制值对应的十进制整数值。
   如果是空字符串（不包含字符），则返回 0。 

   **如果字符串包含除上述情况之外的其他字符，则返回 NaN。**

    对象，调用 valueOf()方法，并按照上述规则转换返回的值。如果转换结果是 NaN，则调用toString()方法，再按照转换字符串的规则转换。

  

- **parseInt()**

  **parseInt函数注重于字符串中是否包含数值模式**

  parseInt函数会从第一个非空格字符开始转换，若**第一个字符**不是**（数值，+，-）**，则立即返回NaN。空字符串也会返回NaN

  若遇到字母则会返回已转换好的数值，若没有已转换好的数值则返回NaN。

  （“+123H”返回123，“+H123”返回NaN）

  

  **parseInt函数有第二个参数，可以选择转换为多少进制**

  parseInt("af") ===NaN

  parseInt("af",16 )===175



## String类型

**转义字符在字符串中作为单个字被符解释**

“ \u03a3”虽然由6个字符组成，但解释之后只占一个字符长度

![image-20210419165453403](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210419165453403.png)

### 字符串的特点

**ECMAScript 中的字符串是不可变的**（immutable），意思是一旦创建，它们的值就不能变了。要修改某个变量中的字符串值，必须先销毁原始的字符串，然后将包含新值的另一个字符串保存到该变量，如下所示：
let lang = "Java";   lang = lang + "Script"; 
这里，变量 lang 一开始包含字符串"Java"。紧接着，lang 被重新定义为包含"Java"和"Script"的组合，也就是"JavaScript"。**整个过程首先会分配一个足够容纳 10 个字符的空间，然后填充上"Java"和"Script"。最后销毁原始的字符串"Java"和字符串"Script"，因为这两个字符串都没有用了。所有处理都是在后台发生的**。



### 转换为字符串

几乎所有值都有toString()方法(null和undefined没有)，这个方法可以返回当前值的字符串等价物。



### 模板字面量

``：可以用来定义html模板。

ECMAScript 6 新增了使用模板字面量定义字符串的能力。与使用单引号或双引号不同，模板字面量保留换行字符，可以跨行定义字符串。

由于模板字面量会保持反引号内部的空格，因此在使用时要格外注意。格式正确的模板字符串看起来可能会缩进不当：

**字符串插值**

1. 模板字面量最常用的一个特性是支持字符串插值，也就是可以在一个连续定义中插入一个或多个值。技术上讲，模板字面量不是字符串，而是一种特殊的 JavaScript 句法表达式，只不过求值后得到的是字符串。模板字面量在定义时立即求值并转换为字符串实例，任何插入的变量也会从它们最接近的作用域中取值。

   let str =`Zzzccs+${value}``

2. **所有插入的值都会使用 toString()强制转型为字符串，而且任何 JavaScript 表达式都可以用于插值。将表达式转换为字符串时会调用 toString()：**

   let foo = { toString: () => 'World' }; 
   console.log(`Hello, ${ foo }!`); // Hello, World!



## Symbol类型

Symbol 是原始数据类型。

**符号是原始值，且符号实例是唯一、不可变的**。
**符号的用途是确保对象属性使用唯一标识符，不会发生属性冲突的危险。**

**Symbol()函数不能与 new 关键字一起作为构造函数使用。**

### 1. 符号的基本用法

**符号需要使用 Symbol()函数初始化。因为符号本身是原始类型，所以 typeof 操作符对符号返回**
**symbol。**

调用 Symbol()函数时，也可以传入一个字符串参数作为对符号的描述（description），将来可以通
过这个字符串来调试代码。**但是，这个字符串参数与符号定义或标识完全无关：**

let genericSymbol = Symbol(); 
let otherGenericSymbol = Symbol(); 
let fooSymbol = Symbol('foo'); 
let otherFooSymbol = Symbol('foo'); 
**console.log(genericSymbol == otherGenericSymbol); // false** 
**console.log(fooSymbol == otherFooSymbol); // false**



### 2.使用符号作为属性

let s1 = Symbol('foo');
let o = { 
[s1]: 'foo val' 
}; 
// 这样也可以：o[s1] = 'foo val'; 

**//注意：o.s1 ='foo bar' 不可用，因为以.的方式定义属性会直接把s1当做属性名，而不会作为一个变量**

console.log(o); 
// {Symbol(foo): foo val}

**Object.getOwnPropertyNames()返回对象实例的常规属性数组，Object.getOwnPropertySymbols()返回对象实例的符号属性数组。这两个方法的返回值彼此互斥**。

let s1 = Symbol('foo'), 
 s2 = Symbol('bar'); 
let o = { 
[s1]: 'foo val', 
 [s2]: 'bar val', 
 baz: 'baz val', 
 qux: 'qux val' 
}; 
**console.log(Object.getOwnPropertySymbols(o));** 
**// [Symbol(foo), Symbol(bar)]** 

**console.log(Object.getOwnPropertyNames(o));** 
**// ["baz", "qux"]**

Object.getOwnPropertyDescriptors()会返回同时包含常规和符号属性描述符的对象。Reflect.ownKeys()会返回两种类型的键



### **3.Symbol.hasInstance**

这个符号作为一个属性表示“一个方法，该方法决定一个构造器对象是否认可一个对象是它的实例。由 instanceof 操作符使用”。instanceof 操作符可以用来确定一个对象实例的原型链上是否有原型。

instanceof 常规用法：

```
function Foo() {} 
let f = new Foo(); 
console.log(f instanceof Foo); // true 
class Bar {} 
let b = new Bar(); 
console.log(b instanceof Bar); // true
```

 ES6 中，instanceof 操作符会使用 Symbol.hasInstance 函数来确定关系。以 Symbol. hasInstance 为键的函数会执行同样的操作，只是操作数对调了一下 ：

foo instanceof Foo在语言内部，实际调用的是:**Foo\[ Symbol.hasInstance] (foo)**

```
function Foo() {} 
let f = new Foo(); 
console.log(Foo[Symbol.hasInstance](f)); // true 
// 采用Foo[Symbol.hasInstanc]是因为这个属性有.分割，不适合用Symbol.xxx的形式调用
class Bar {} 
let b = new Bar(); 
console.log(Bar[Symbol.hasInstance](b)); // true 
```

这个属性定义在 Function 的原型上，因此默认在所有函数和类上都可以调用。由于 instanceof
操作符会在原型链上寻找这个属性定义，就跟在原型链上寻找其他属性一样



## Object类型

每个 Object 实例都有如下属性和方法。
 constructor：用于创建当前对象的函数。在前面的例子中，这个属性的值就是 Object() 函数。
 hasOwnProperty(propertyName)：用于判断当前对象实例（不是原型）上是否存在给定的属性。要检查的属性名必须是字符串（如 o.hasOwnProperty("name")）或符号。
 isPrototypeOf(object)：用于判断当前对象是否为另一个对象的原型。（第 8 章将详细介绍原型。）
 propertyIsEnumerable(propertyName)：用于判断给定的属性是否可以使用（本章稍后讨论的）for-in 语句枚举。与 hasOwnProperty()一样，属性名必须是字符串。
 toLocaleString()：返回对象的字符串表示，该字符串反映对象所在的本地化执行环境。
 toString()：返回对象的字符串表示。
 valueOf()：返回对象对应的字符串、数值或布尔值表示。通常与 toString()的返回值相同。



## 原始值包装类型

原始值的引用类型:Boolean、Number 和 String

```
let s1 = "some text"; //原始值字面量
let s2 = s1.substring(2); //原始值调用String对象的方法原始值

在这里，s1 是一个包含字符串的变量，它是一个原始值。第二行紧接着在 s1 上调用了 substring()
方法，并把结果保存在 s2 中。我们知道，原始值本身不是对象，因此逻辑上不应该有方法。而实际上
这个例子又确实按照预期运行了。这是因为后台进行了很多处理，从而实现了上述操作。具体来说，当
第二行访问 s1 时，是以读模式访问的，也就是要从内存中读取变量保存的值。在以读模式访问字符串
值的任何时候，后台都会执行以下 3 步：
(1) 创建一个 String 类型的实例；
(2) 调用实例上的特定方法；
(3) 销毁实例。

可以把这 3 步想象成执行了如下 3 行 ECMAScript 代码：
let s1 = new String("some text"); 
let s2 = s1.substring(2); 
s1 = null;
```



**特别注意，这3种原始值字面量添加属性时的问题**

**String,Number,Boolean同理**

```
let s1 = "some text"

/**
  * 原因是：访问 s1 时，是以读模式访问的,后台都会执行以下 3 步:
  *	(1) 创建一个 String 类型的实例；
  *	(2) 调用实例上的特定方法；
  *	(3) 销毁实例。
  **/
s1.zzzccs='666' //给字面量添加属性
//相当于执行完上面这行代码时，这个拥有zzzccs属性的对象已经被销毁了


//此时执行下面这行代码时，会重新创建一个String类型的实例，而这个实例不具有zzzccs属性
console.log(s1.zzzccs)  //undefined
```



## 数组搜索和位置方法

ECMAScript 提供两类搜索数组的方法：**按严格相等搜索和按断言函数搜索。**

1. 严格相等

ECMAScript 提供了 3 个严格相等的搜索方法：**indexOf()、lastIndexOf()和 includes()。**

**indexOf()和 lastIndexOf()都返回要查找的元素在数组中的位置，如果没找到则返回-1。
includes()返回布尔值。**

**在比较第一个参数跟数组每一项时，会使用全等（===）比较，也就是说两项必须严格相等**

```
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1]; 
//第一个参数是查找的值，第二个参数是查找的起始位置
alert(numbers.indexOf(4)); // 3 
alert(numbers.lastIndexOf(4)); // 5 
alert(numbers.includes(4)); // true 
alert(numbers.indexOf(4, 4)); // 5 
alert(numbers.lastIndexOf(4, 4)); // 3 
alert(numbers.includes(4, 7)); // false 


let person = { name: "Nicholas" }; 
let people = [{ name: "Nicholas" }]; 
let morePeople = [person]; 

//在people数组里查找person，虽然people和person
alert(people.indexOf(person)); // -1 

alert(morePeople.indexOf(person)); // 0 

alert(people.includes(person)); // false 

alert(morePeople.includes(person)); // true
```





# flex布局

## 原理

给父元素添加display：flex，对子元素进行控制



## 常见父项元素

![image-20210530170611876](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210530170611876.png)





# ES6 class特点

## ES5和ES6继承的区别

**注意，父类的静态方法，也会被子类继承。**

extends 时，子类在构造函数里必须在开头调用super()，这是因为ES6的继承，子类的this必须先经过父类的构造函数构造完之后再将自身的属性进行构造。

ES5 的继承，**实质是先创造子类的实例对象`this`，然后再将父类的方法添加到`this`上面（`Parent.apply(this)`）。**

ES6 的继承机制完全不同，**实质是先将父类实例对象的属性和方法，加到`this`上面（所以必须先调用`super`方法），然后再用子类的构造函数修改`this`。**

**如果子类没有定义`constructor`方法，这个方法会被默认添加，代码如下。也就是说，不管有没有显式定义，任何一个子类都有`constructor`方法。**



## super 关键字

`super`这个关键字，**既可以当作函数使用，也可以当作对象使用**。在这两种情况下，它的用法完全不同。

  **不管super作为函数还是对象的形式，他调用的属性的this都是指向子类**



#### **第一种情况，`super`作为函数调用时，代表父类的构造函数。作为函数时，`super()`只能用在子类的构造函数之中，用在其他地方就会报错。**

​		**注意，`super`虽然代表了父类`A`的构造函数，但是返回的是子类`B`的实例，即`super`内部的`this`指的是`B`的		实例，因此`super()`在这里相当于`A.prototype.constructor.call(this)`。**



#### **第二种情况，`super`作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。**

​		**super.p()`，就是将`super`当作一个对象使用。这时，`super`在普通方法之中，指向`A.prototype，		所以	`super.p()`就相当于A.prototype.p()。**

由于`super`指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过`super`调用的。

```
class A {
  constructor() {
    this.p = 2;
  }
  say() {
  	console.log("A.say")
  }
}
A.prototype.name = "A.name"

class B extends A {
  get m() {
    return super.p;
  }
  get name() {
    return super.name;
  }
  Bsay() {
  	return super.say();
  }
}

let b = new B();
b.m // undefined  p是父类A实例的属性，super.p就引用不到它。
b.name // "A.name"  name和say都在A的prototype上，所以可以访问到
b.Bsay() // "A.say"
```

**注意，不管super作为函数还是对象的形式，他调用的属性的this都是指向子类**

```
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  m() {
    super.print();  // super.print.call(this)。实际是这样执行的
  }
}

let b = new B();
b.m() // 2  
```





如果`super`作为对象，用在静态方法之中，这时`super`将指向父类，而不是父类的原型对象。

```
class Parent {
  static myMethod(msg) {
    console.log('static', msg);
  }

  myMethod(msg) {
    console.log('instance', msg);
  }
}

class Child extends Parent {
  static myMethod(msg) {
    super.myMethod(msg);
  }

  myMethod(msg) {
    super.myMethod(msg);
  }
}

Child.myMethod(1); // static 1

var child = new Child();
child.myMethod(2); // instance 2
```



在子类的静态方法中通过`super`调用父类的方法时，方法内部的`this`指向当前的子类，而不是子类的实例。

```
class A {
  constructor() {
    this.x = 1;
  }
  static print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  static m() {
    super.print();
  }
}

B.x = 3;
B.m() // 3
```

