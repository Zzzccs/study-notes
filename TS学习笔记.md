# TS



## ts 全局安装及编译

```
npm install -g typescript
```

浏览器没办法直接执行ts文件，因此需要将ts转化成js文件才能执行。

通过    **tsc  xxx.ts**  命令可以将一个ts文件编译成js文件。



## 构建第一个ts程序

```typescript
console.log('i am ts');

function greeter(person: string) {  // 使用了ts的类型注解，限制传入的参数person是一个字符串
    return "Hello, " + person;
}

let user = "Jane User";

document.body.innerHTML = greeter(user);
```

tsc编译得到的js：

```javascript
console.log('i am ts');
function greeter(person) {
    return "Hello, " + person;
}
var user = "Jane User";
document.body.innerHTML = greeter(user);
```



**若把传入的参数换成别的类型，ts会出现报错信息**

![image-20210921093334537](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210921093334537.png)

![image-20210921093642975](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210921093642975.png)

**！注意，即使ts文件存在报错，tsc编译时会出现报错，但是依然可以得到js文件**



## 使用接口interface限定参数的类型

```typescript
interface Person {
    firstName: String
    lastName: String
    fulName: String
}
class Student {
    fulName: String
    constructor(public firstName: String, public lastName: String) {
        this.firstName = `${firstName}-${lastName}`;
    }
}
let s = new Student("郑", "灿升");
function say(person: Person) {
    console.log(person);
}
say(s);
```

***注意：在构造函数的参数上使用`public`等同于创建了同名的成员变量。***

```javascript
// 上述ts转化后的js文件，可以看到传入的参数firstName, lastName使用public修饰后成为了成员变量
// 所以用 this.firstName 和 this.lastName 
var Student = /** @class */ (function () {
    function Student(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.firstName = firstName + "-" + lastName;
    }
    return Student;
}());
var s = new Student("郑", "灿升");
function say(person) {
    console.log(person);
}
say(s);

```



## ts数据类型

### string、number、boolean

ts中string、number、boolean与js差别并不大 



### 数组

数组： 有两种方式可以定义数组。 

第一种方式是使用数组泛型，Array<元素类型>：
1.	let arr: Array<number> = [1,2,'123'] // '123'为非法数据，该数组元素必须为number
第二种，可以在元素类型后面接上[]，表示由此类型元素组成的一个数组：
2.  let arr: string[] = ['1','2',123] // 123为非法数据，该数组元素必须为string



### 元组

元组类型允许表示一个***已知元素数量和类型的数组，各元素的类型不必相同。***

```typescript
// Declare a tuple type
let x: [string, number];  // 定义数组前两个元素的类型为字符串和数字，超过两个元素时会使用联合类型替代
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

```
x[3] = 'world'; // OK, 字符串可以赋值给(string | number)类型

console.log(x[5].toString()); // OK, 'string' 和 'number' 都有 toString

x[6] = true; // Error, 布尔不是(string | number)类型
```



### 任意值any

有时候，我们会想要为那些**在编程阶段还不清楚类型的变量指定一个类型**。 这些值可能来自于动态的内容，比如**来自用户输入或第三方代码库**。 **这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用`any`类型来标记这些变量：**

```
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

any的用处：

当你只知道一部分数据的类型时，`any`类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据：

```
let arr: Array<any> = [1,'2',false];
let arr2: any[] = [1,'2',false];
```



**any和Object的区别**

`Object`类型的变量只是允许你给它赋任意值 - 但是却不能够在它上面调用任意的方法，即便它真的有这些方法。

`any`类型的变量可以调用它具有的方法

![image-20210921152155585](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210921152155585.png)



### 空值

某种程度上来说，`void`类型像是与`any`类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是`void`：

```typescript
function warnUser(): void {
    alert("This is my warning message");
}
```

声明一个`void`类型的变量没有什么大用，因为你只能为它赋予`undefined`和`null`：

```typescript
let unusable: void = undefined;
```



### null和undefined

**默认情况下null和undefined是所有数据类型的子集，即null和undefined可以赋值给所有类型的变量**

**当开启了--strictNullChecks标记后，null和undefined只能赋值给void和它们各自**

也许在某处你想传入一个`string`或`null`或`undefined`，你可以使用**联合类型`string | null | undefined`。** 再次说明，稍后我们会介绍联合类型。



### Never

`never`类型表示的是那些永不存在的值的类型。 例如，`never`类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是`never`类型，当它们被永不为真的类型保护所约束时。

`never`类型是任何类型的子类型，也可以赋值给任何类型；然而，*没有*类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）。 即使`any`也不可以赋值给`never`。

下面是一些返回`never`类型的函数：

```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```





## 变量声明

**和js一样，ts也可以通过var, let, const进行变量的声明，同时这些声明变量的方式对应的规则在ts中也存在。**

1. var不存在块级作用域，并且可以重复声明。
2. let存在块级作用域，在当前块级作用域內不能存在重名的变量。let存在变量提升但是有暂时性死区，在声明前引用会出现报错。
3. const存在块级作用域，在当前块级作用域內不能存在重名的变量。const没有变量提升，因此不能在声明前使用。const定义的是一个常量，必须在声明的同时赋值，否则会报错，并且（基本数据类型）赋值之后无法改变值；（引用数据类型）可以改变内部的值，但是无法改变const声明的变量指向的地址。



## 接口

**在TypeScript里，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。**

```
interface Person {
    name: string, // 必须具备的属性
    age: number, // 必须具备的属性
    sex?: string  // 可选属性，这个属性存不存在都可
    readonly height: number // 只读属性，这个属性无法被修改
}
function say(person: Person): void {
  	person.name = '123';
  	person.height = 123;  // 这一行会出现报错，只读属性height无法被修改
  	console.log(person);
}
var person = {
    name: 'zzzccs',
    age: 22,
    sex: '男',
    height: 178
}
say(person)
```

### 

### readonly` vs `const

最简单判断该用`readonly`还是`const`的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用`const`，若做为属性则使用`readonly`。



### 额外的属性检查

对象字面量会被特殊对待而且会经过*额外属性检查*，当将它们赋值给变量或作为参数传递的时候。 如果一个对象字面量存在任何“目标类型”不包含的属性时，你会得到一个错误。

如：

```typescript
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    // ...
}

let mySquare = createSquare({ colour: "red", width: 100 });  // 注意这里的colour跟color不同，color虽然是可选属性，但是ts对字面量进行额外的检测，如果一个对象字面量存在任何“目标类型”不包含的属性时就会报错，也就是说这个字面量里的属性必须是约束接口的子集

let obj = { colour: "red", width: 100 };
let mySquare = createSquare(obj);  // 这种情况并不会出现报错
```

解决方法：

1.  使用类型断言，这种方法简单但不建议使用

   let mySquare = createSquare({ colour: "red", width: 100 } as SquareConfig); 

2.  常用方法：添加一个字符串索引签名，前提是你能够确定这个对象可能具有某些做为特殊用途使用的额外属性。 如果`SquareConfig`带有上面定义的类型的`color`和`width`属性，并且*还会*带有任意数量的其它属性，那么我们可以这样定义它：

   ```typescript
   interface SquareConfig {
       color?: string;
       width?: number;
       [propName: string]: any;  // 允许额外的属性（属性名为string类型），值类型为any
   }
   ```



### 接口描述函数

```
interface xxx {
	(p1: string, p2: number): number  //标志着这个函数最多只能有2个参数并且参数类型要符合接口，函数返回值为number
}
var f:xxx;  // 声明一个变量是符合xxx函数接口
f = function s(a:string,b:number) {  
	return 123;
}
```



### 接口描述可索引的类型

可索引类型具有一个*索引签名*，它描述了对象索引的类型，还有相应的索引返回值类型。 

```typescript
interface StringArray {
  [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];

let myStr: string = myArray[0];
```



## 类型断言

```
function fb(params: string|number):number {  //传入的参数是联合类型，string有length，number没有
    if((<string>params).length) {  // 我认为他是string类型时走这一步
      return (<string>params).length
    } else {
      return params.toString().length;
    }
}
```

