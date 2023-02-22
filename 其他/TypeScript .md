### 一、TypeScript 是什么



TypeScript 是一种由微软开发的自由和开源的编程语言。它是 JavaScript 的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。



TypeScript 提供最新的和不断发展的 JavaScript 特性，包括那些来自 2015 年的 ECMAScript 和未来的提案中的特性，比如异步功能和 Decorators，以帮助建立健壮的组件。下图显示了 TypeScript 与 ES5、ES2015 和 ES2016 之间的关系：



![img](https://cdn.nlark.com/yuque/0/2022/png/371088/1671697911000-0d0641d2-2eb7-406a-b313-22b71040c90a.png)



#### 为什么需要TypeScript?



简单来说就是因为JavaScript是弱类型, 很多错误只有在运行时才会被发现
而TypeScript提供了一套静态检测机制, 可以帮助我们在编译时就发现错误



#### 1.1 TypeScript 与 JavaScript 的区别

| TypeScript                                     | JavaScript                                 |
| ---------------------------------------------- | ------------------------------------------ |
| JavaScript 的超集用于解决大型项目的代码复杂性  | 一种脚本语言，用于创建动态网页。           |
| 可以在编译期间发现并纠正错误                   | 作为一种解释型语言，只能在运行时发现错误   |
| 强类型，支持静态和动态类型                     | 弱类型，没有静态类型选项                   |
| 最终被编译成 JavaScript 代码，使浏览器可以理解 | 可以直接在浏览器中使用                     |
| 支持模块、泛型和接口                           | 不支持模块，泛型或接口                     |
| 社区的支持仍在增长，而且还不是很大             | 大量的社区支持以及大量文档和解决问题的支持 |



#### 1.2 获取 TypeScript



命令行的 TypeScript 编译器可以使用 Node.js 包来安装。



**1.安装 TypeScript**



```plain
$ npm install -g typescript  // 全局安装 ts
```



不记得自己是否已经安装过 typescript 的，可以使用以下命令来验证：



```plain
tsc -v
```



如果出现版本，则说明已经安装成功



```plain
Version 4.9.4
```



**2.编译 TypeScript 文件**



生成 tsconfig.json 配置文件



```plain
tsc --init
```



执行命令后我们就可以看到生成了一个 tsconfig.json 文件，里面有一些配置信息
在我们`helloworld.ts`文件中,随便写点什么



```plain
const s:string = "彼时彼刻，恰如此时此刻";
console.log(s);
```



控制台执行 `tsc helloworld.ts` 命令，目录下生成了一个同名的 helloworld.js 文件，代码如下



```plain
var s = "彼时彼刻，恰如此时此刻";
console.log(s);
```



通过tsc命令，发现我们的typescript代码被转换成了熟悉的js代码



我们接着执行



```plain
node helloworld.js
```



即可看到输出结果



当然，对于刚入门 TypeScript 的小伙伴，也可以不用安装 `typescript`，而是直接使用线上的 TypeScript Playground 来学习新的语法或新特性。



TypeScript Playground：https://www.typescriptlang.org/play/



备注：



1. 定义任何东西的时候要注明类型
2. 调用任何东西的时候要检查类型



#### 1.3.安装ts-node



那么通过我们上面的一通操作，我们知道了运行tsc命令就可以编译生成一个js文件，但是如果每次改动我们都要手动去执行编译，然后再通过 node命令才能查看运行结果岂不是太麻烦了。



而 ts-node 正是来解决这个问题的



```plain
npm i -g ts-node // 全局安装ts-node
```



有了这个插件，我们就可以直接运行.ts文件了



```plain
ts-node index.ts
```



### 二、基本类型



-  类型：  

| 类型    | 例子              | 描述                           |
| ------- | ----------------- | ------------------------------ |
| number  | 1, -33, 2.5       | 任意数字                       |
| string  | 'hi', "hi", `hi`  | 任意字符串                     |
| boolean | true、false       | 布尔值true或false              |
| 字面量  | 其本身            | 限制变量的值就是该字面量的值   |
| any     | *                 | 任意类型                       |
| unknown | *                 | 类型安全的any                  |
| void    | 空值（undefined） | 没有值（或undefined）          |
| never   | 没有值            | 不能是任何值                   |
| object  | {name:'孙悟空'}   | 任意的JS对象                   |
| array   | [1,2,3]           | 任意JS数组                     |
| tuple   | [4,5]             | 元素，TS新增类型，固定长度数组 |
| enum    | enum{A, B}        | 枚举，TS中新增类型             |



#### 2.1 Number 类型



```plain
let count: number = 10;
// ES5：var count = 10;
```



#### 2.2 String 类型



```plain
let str: string = "hi";
// ES5：var str = 'hi';
```



#### 2.3 Boolean 类型



```plain
let isDone: boolean = false;
// ES5：var isDone = false;
```



#### 2.4 字面量 类型



也可以使用字面量去指定变量的类型，通过字面量可以确定变量的取值范围



```plain
let color: 'red' | 'blue' | 'black';
let num: 1 | 2 | 3 | 4 | 5;
```



#### 2.5 any 类型



在 TypeScript 中，任何类型都可以被归为 any 类型。这让 any 类型成为了类型系统的顶级类型.



如果是一个普通类型，在赋值过程中改变类型是不被允许的：



```plain
let a: string = 'seven';
a = 7;
// TS2322: Type 'number' is not assignable to type 'string'.
```



但如果是 `any` 类型，则允许被赋值为任意类型。



```plain
let a: any = 666;
a = "Semlinker";
a = false;
a = 66
a = undefined
a = null
a = []
a = {}
```



在any上访问任何属性都是允许的,也允许调用任何方法.



```plain
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);
anyThing.setName('Jerry');
anyThing.setName('Jerry').sayHello();
anyThing.myName.setFirstName('Cat');
```



**变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型**：



```plain
let something;
something = 'seven';
something = 7;
```



在许多场景下，这太宽松了。使用 `any` 类型，可以很容易地编写类型正确但在运行时有问题的代码。如果我们使用 `any` 类型，就无法使用 TypeScript 提供的大量的保护机制。请记住，`any 是魔鬼！`尽量不要用any。



为了解决 `any` 带来的问题，TypeScript 3.0 引入了 `unknown` 类型。



#### 2.6 unknown 类型



`unknown`与`any`一样，所有类型都可以分配给`unknown`:



```plain
let notSure: unknown = 4;
notSure = "maybe a string instead"; // OK
notSure = false; // OK
```



```
unknown`与`any`的最大区别是： 任何类型的值可以赋值给`any`，同时`any`类型的值也可以赋值给任何类型。`unknown` 任何类型的值都可以赋值给它，但它只能赋值给`unknown`和`any
```



```plain
let notSure1: unknown = 4;
let uncertain1: any = notSure1; // OK

let notSure2: any = 4;
let uncertain2: unknown = notSure2; // OK

let notSure3: unknown = 4;
let uncertain3: number = notSure3; // Error

let notSure4: any = 4;
let uncertain4: number = notSure4; // OK

let e: unknown;
let s:string;
// unknown 实际上就是一个类型安全的any
// unknown类型的变量，不能直接赋值给其他变量
if(typeof e === "string"){
    s = e;
}
```



#### 2.7 void  类型



void 类型像是与 any 类型相反，它表示没有任何类型。当一个函数没有返回值时，你通常会见到其返回值类型是 void



声明一个 void 类型的变量没有什么作用，因为它的值只能为 `undefined` 或 `null`（在`strictNullChecks`未指定为true时）, 一般也只有在函数没有返回值时去声明



```plain
// 返回undefined就相当于没返回值
function fn3():void{
    return undefined
}

function fn2():void{
    return
}

// void 用来表示空值，以函数为例，就表示没有返回值的函数
function fn(): void{
}
```



#### 2.8 Never 类型



`never` 类型表示的是那些永不存在的值的类型。 例如，`never` 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型。



```plain
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}
```



思考:never 和 void 的区别 void 可以被赋值为 null（在`strictNullChecks`未指定为true时） 和 undefined 的类型。 never 则是一个不包含值的类型。 拥有 void 返回值类型的函数能正常运行。拥有 never 返回值类型的函数无法正常返回，无法终止，或会抛出异常。



#### 2.9  Null 和 Undefined 类型



TypeScript 里，`undefined` 和 `null` 两者有各自的类型分别为 `undefined` 和 `null`。



```plain
let u: undefined = undefined;
let n: null = null;
```



默认情况下 `null` 和 `undefined` 是所有类型的子类型。 就是说你可以把 `null` 和 `undefined` 赋值给 `number` 类型的变量。**然而，如果你指定了`--strictNullChecks` 标记，`undefined` 只能赋值给 `void` 和它自己的类型。
`null` 只能赋值给它自己的类型**



#### 2.10 Array 类型



```plain
let list: number[] = [1, 2, 3];
// ES5：var list = [1,2,3];

let list1: Array<number> = [1, 2, 3]; // Array<number>泛型语法
// ES5：var list = [1,2,3];
```



#### 2.11 元组 Tuple 类型



元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同
元组最重要的特性是可以限制`数组元素的个数和类型`，它特别适合用来实现多值返回。



```plain
let x: [string, number]; 
// 类型必须匹配且个数必须为2

x = ['hello', 10]; // OK 
x = ['hello', 10,10]; // Error 
x = [10, 'hello']; // Error
```



#### 2.12 object  类型



- object object 类型用于表示所有的非原始类型，即我们不能把 number、string、boolean、symbol等 原始类型赋值给 object。在`strictNullChecks:true`下，null 和 undefined 类型也不能赋给 object。



```plain
let object: object;
object = 1; // 报错
object = "a"; // 报错
object = true; // 报错
object = null; // 报错
object = undefined; // 报错
object = {}; // 编译正确
```



- Object



大 Object 代表所有拥有 toString、hasOwnProperty 方法的类型 所以所有原始类型、非原始类型都可以赋给 Object(严格模式下 null 和 undefined 不可以)



```plain
let object: Object;
object = 1; // 编译正确
object = "a"; // 编译正确
object = true; // 编译正确
object = null; // 报错
object = undefined; // 报错
object = {}; // ok
```



- {}



{} 空对象类型和大 Object 一样 也是表示原始类型和非原始类型的集合



```plain
let simpleCase: {};
simpleCase = 1; // ok
simpleCase = "a"; // ok
simpleCase = true; // ok
simpleCase = null; // error
simpleCase = undefined; // error
simpleCase = {}; // ok
```



#### 2.13 Enum 类型



使用枚举我们可以定义一些带名字的常量。 使用枚举可以清晰地表达意图或创建一组有区别的用例。 TypeScript 支持数字的和基于字符串的枚举。



```plain
enum Gender{
    Male,
    Female
}

let i: {name: string, gender: Gender};
i = {
    name: '孙悟空',
    gender: Gender.Male 
}
console.log(i)
```



### 三、类型推论



我们把 TypeScript 这种基于赋值表达式推断类型的能力称之为`类型推断`。



在 TypeScript 中，具有初始化值的变量、有默认值的函数参数、函数返回的类型都可以根据上下文推断出来。比如我们能根据 return 语句推断函数返回的类型，如下代码所示：



```plain
{
  /** 根据参数的类型，推断出返回值的类型也是 number */
  function add1(a: number, b: number) {
    return a + b;
  }
  const x1= add1(1, 1); // 推断出 x1 的类型也是 number
  
  /** 推断参数 b 的类型是数字或者 undefined，返回值的类型也是数字 */
  function add2(a: number, b = 1) {
    return a + b;
  }
}
```



指编程语言中能够自动推导出值的类型的能力 它是一些强静态类型语言中出现的特性 定义时未赋值就会推论成 `any` 类型 如果定义的时候就赋值就能利用到类型推论



```plain
let flag; //推断为any
let count = 123; //为number类型
let hello = "hello"; //为string类型
```



### 四、 联合类型



联合类型（Union Types）表示取值可以为多种类型中的一种 未赋值时联合类型上只能访问两个类型共有的属性和方法



```plain
let name1: string | number;
console.log(name1.toString());
name1 = 1;
console.log(name1.toFixed(2));
name1 = "hello";
console.log(name1.length);

// 定义联合类型数组
let arr1:(number | string)[];
// 表示定义了一个名称叫做arr的数组, 
// 这个数组中将来既可以存储数值类型的数据, 也可以存储字符串类型的数据 
arr1 = [1, 'b', 2, 'c'];
```



### 五、 类型断言



有时候你会遇到这样的情况，你会比 TypeScript 更了解某个值的详细信息。通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。其实就是你需要手动告诉 ts 就按照你断言的那个类型通过编译（这一招很关键 有时候可以帮助你解决很多编译报错）



类型断言有两种形式：



```plain
// 尖括号 语法
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

// as 语法
let someValue1: any = "this is a string";
let strLength1: number = (someValue as string).length;
```



以上两种方式虽然没有任何区别，但是尖括号格式会与 react 中 JSX 产生语法冲突，因此我们更推荐使用 as 语法。



**非空断言** 在上下文中当类型检查器无法断定类型时 一个新的后缀表达式操作符 ! 可以用于断言操作对象是非 `null` 和非 `undefined` 类型



```plain
let flag: null | undefined | string;
// 非 null 和非 undefined 类型
flag!.toString(); // ok
flag.toString(); // error
```



### 六、字面量类型



在 TypeScript 中，字面量不仅可以表示值，还可以表示类型，即所谓的字面量类型。 目前，TypeScript 支持 3 种字面量类型：字符串字面量类型、数字字面量类型、布尔字面量类型，对应的字符串字面量、数字字面量、布尔字面量分别拥有与其值一样的字面量类型，具体示例如下：



```plain
let flag1: "hello" = "hello";
let flag2: 1 = 1;
let flag3: true = true;
```



### 七、 类型别名



类型别名用来给一个类型起个新名字



```plain
type flag = string | number;
function hello(value: flag) {}

hello(1)
hello('s')
hello(true)  // 报错
```



### 八、 交叉类型



交叉类型是将多个类型合并为一个类型。通过 & 运算符可以将现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性



```plain
type Flag1 = { x: number };
type Flag2 = Flag1 & { y: string };

let flag3: Flag2 = {
  x: 1,
  y: "hello",
  henb,
};
```



### 十、函数



#### 1. 函数声明



```plain
function add(x: number, y: number): number {
  return x + y;
}


function hello(name: string): void {
  console.log("hello", name);
}
```



#### 2. 函数表达式



```plain
const add = function(x: number, y: number): number {
  return x + y;
}
```



#### 3. 接口定义函数



```plain
interface Add {
  (x: number, y: number): number;
}
```



#### 4. 可选参数



```plain
function add(x: number, y?: number): number {
  return y ? x + y : x;
}
```



#### 5. 默认参数



```plain
function add(x: number, y: number = 0): number {
  return x + y;
}
```



#### 6. 剩余参数



```plain
function sum(...numbers: number[]) {
  return numbers.reduce((val, item) => (val += item), 0);
}
console.log(sum(1, 2, 3));
```



#### 7. 函数重载



函数重载或方法重载是使用相同名称和不同参数数量或类型创建多个方法的一种能力。



```plain
function add(x: number, y: number): number;
function add(x: string, y: string): string;
function add(x: any, y: any): any {
  return x + y;
}


add(1,1)
add('a','b')
add(true,[])
```



上面示例中，我们给同一个函数提供多个函数类型定义，从而实现函数的重载



需要注意的是:



函数重载真正执行的是同名函数最后定义的函数体 在最后一个函数体定义之前全都属于函数类型定义 不能写具体的函数实现方法 只能定义类型



### 十一、类



##### 1. 类的定义



在 TypeScript 中，我们可以通过 `Class` 关键字来定义一个类



```plain
class Person {
  name: string;
  constructor(_name: string) {
    this.name = _name;
  }
  getName(): void {
    console.log(this.name);
  }
}
let p1 = new Person("hello");
p1.getName();
```



##### 2. 存取器



在 TypeScript 中，我们可以通过存取器来改变一个类中属性的读取和赋值行为



```plain
class User {
  myname: string;
  constructor(myname: string) {
    this.myname = myname;
  }
  get name() {
    return this.myname;
  }
  set name(value) {
    this.myname = value;
  }
}

let user = new User("hello");
user.name = "world";
console.log(user.name);
```



##### 3. readonly 只读属性



只读属性必须在声明时或**构造函数**里被初始化。



```plain
class Animal {
  public readonly name: string;
  constructor(name: string) {
    this.name = name;
  }
  changeName(name: string) {
    this.name = name; //这个ts是报错的
  }
}

let a = new Animal("hello");
```



##### 4. 继承



子类继承父类后子类的实例就拥有了父类中的属性和方法，可以增强代码的可复用性



将子类公用的方法抽象出来放在父类中，自己的特殊逻辑放在子类中重写父类的逻辑



super 可以调用父类上的方法和属性



在 TypeScript 中，我们可以通过 extends 关键字来实现继承



```plain
class Person {
  name: string; //定义实例的属性，默认省略public修饰符
  age: number;
  constructor(name: string, age: number) {
    //构造函数
    this.name = name;
    this.age = age;
  }
  getName(): string {
    return this.name;
  }
  setName(name: string): void {
    this.name = name;
  }
}
class Student extends Person {
  no: number;
  constructor(name: string, age: number, no: number) {
    super(name, age);
    this.no = no;
  }
  getNo(): number {
    return this.no;
  }
}
let s1 = new Student("hello", 10, 1);
console.log(s1);
```



##### 5. 类里面的修饰符



**public** 类里面 子类 其它任何地方外边都可以访问
**protected** 类里面 子类 都可以访问,其它任何地方不能访问
**private** 类里面可以访问，子类和其它任何地方都不可以访问



```plain
class Parent {
  public name: string;
  protected age: number;
  private car: number;
  constructor(name: string, age: number, car: number) {
    //构造函数
    this.name = name;
    this.age = age;
    this.car = car;
  }
  getName(): string {
    return this.name;
  }
  setName(name: string): void {
    this.name = name;
  }
}
class Child extends Parent {
  constructor(name: string, age: number, car: number) {
    super(name, age, car);
  }
  desc() {
    console.log(`${this.name} ${this.age} ${this.car}`); //car访问不到 会报错
  }
}

let child = new Child("hello", 10, 1000);
console.log(child.name);
console.log(child.age); //age访问不到 会报错
console.log(child.car); //car访问不到 会报错
```



##### 6. 静态属性 静态方法



类的静态属性和方法是直接定义在类本身上面的 所以也只能通过直接调用类的方法和属性来访问



```plain
class Parent {
  static mainName = "Parent";
  static getmainName() {
  // 注意静态方法里面的this指向的是类本身 而不是类的实例对象 
  // 所以静态方法里面只能访问类的静态属性和方法
    console.log(this); 
    return this.mainName;
  }
  public name: string;
  constructor(name: string) {
    //构造函数
    this.name = name;
  }
}
console.log(Parent.mainName);
console.log(Parent.getmainName());
```



##### 7. 抽象类和抽象方法



抽象类，无法被实例化，只能被继承并且无法创建抽象类的实例 子类可以对抽象类进行不同的实现



抽象方法只能出现在抽象类中并且抽象方法不能在抽象类中被具体实现，只能在抽象类的子类中实现（必须要实现）



使用场景： 我们一般用抽象类和抽象方法抽离出事物的共性 以后所有继承的子类必须按照规范去实现自己的具体逻辑 这样可以增加代码的可维护性和复用性



使用 `abstract` 关键字来定义抽象类和抽象方法



```plain
abstract class Animal {
// 定义一个抽象方法
// 抽象方法使用 abstract开头，没有方法体
// 抽象方法只能定义在抽象类中，子类必须对抽象方法进行重写
  abstract speak(): void;
}
class Cat extends Animal {
// 抽象方法子类中实现
  speak() {
    console.log("喵喵喵");
  }
}
let animal = new Animal(); //直接报错 无法创建抽象类的实例
let cat = new Cat();
cat.speak();
```



思考 1:重写(override)和重载(overload)的区别



**重写**是指子类重写继承自父类中的方法 **重载**是指为同一个函数提供多个类型定义



```plain
class Animal {
  speak(word: string): string {
    return "动物:" + word;
  }
}
class Cat extends Animal {
  speak(word: string): string {
    return "猫:" + word;
  }
}
let cat = new Cat();
console.log(cat.speak("hello"));
// 上面是重写
//--------------------------------------------
// 下面是重载
function double(val: number): number;
function double(val: string): string;
function double(val: any): any {
  if (typeof val == "number") {
    return val * 2;
  }
  return val + val;
}

let r = double(1);
console.log(r);
```



思考 2:什么是**多态**



在父类中定义一个方法，在子类中有多个实现，在程序运行的时候，根据不同的对象执行不同的操作，实现运行时的绑定。



```plain
abstract class Animal {
  // 声明抽象的方法，让子类去实现
  abstract sleep(): void;
}
class Dog extends Animal {
  sleep() {
    console.log("dog sleep");
  }
}
let dog = new Dog();
class Cat extends Animal {
  sleep() {
    console.log("cat sleep");
  }
}
let cat = new Cat();
let animals: Animal[] = [dog, cat];
animals.forEach((i) => {
  i.sleep();
});
```



### 十二、接口



接口的作用类似于抽象类，不同点在于接口中的所有方法和属性都是没有实值的，换句话说接口中的所有方法都是抽象方法。接口主要负责定义一个类的结构，接口可以去限制一个对象的接口，接口只定义对象的结构，而不考虑实际值, 对象只有包含接口中定义的所有属性和方法时才能匹配接口。同时，可以让一个类去实现接口，实现接口(就是使类满足接口的要求)。



#### 1. 对象的形状



```plain
  // 接口用来定义一个类结构，用来定义一个类中应该包含哪些属性和方法
 // 同时接口也可以当成类型声明去使用
interface Speakable {
  speak(): void;
  readonly lng: string; //readonly表示只读属性 后续不可以更改
  name?: string; //？表示可选属性
}

let speakman: Speakable = {
  //   speak() {}, //少属性会报错
  name: "hello",
  lng: "en",
  age: 111, //多属性也会报错
};

// 正确案例
interface Arrobj{
    name:string,
    age:number
}
let arr:Arrobj[]=[{name:'jimmy',age:22}]
```



#### 2. 行为的抽象



接口可以把一些类中共有的属性和方法抽象出来,可以用来约束实现此接口的类



一个类可以实现多个接口，一个接口也可以被多个类实现



我们用 `implements`关键字来代表 实现



```plain
//接口可以在面向对象编程中表示为行为的抽象
interface Speakable {
  speak(): void;
}
interface Eatable {
  eat(): void;
}
//一个类可以实现多个接口
class Person implements Speakable, Eatable {
  speak() {
    console.log("Person说话");
  }
  // eat() {} //需要实现的接口包含eat方法 不实现会报错
}
```



#### 3. 定义任意属性



如果我们在定义接口的时候无法预先知道有哪些属性的时候,可以使用 `[propName:string]:any`,propName 名字是任意的



```plain
interface Person {
  id: number;
  name: string;
  [propName: string]: any;
}

let p1 = {
  id: 1,
  name: "hello",
  age: 10,
};
```



这个接口表示 必须要有 id 和 name 这两个字段 然后还可以新加其余的未知字段



#### 4. 接口的继承



我们除了类可以继承 接口也可以继承 同样的使用 `extends`关键字



```plain
interface Speakable {
  speak(): void;
}
interface SpeakChinese extends Speakable {
  speakChinese(): void;
}
class Person implements SpeakChinese {
  speak() {
    console.log("Person");
  }
  speakChinese() {
    console.log("speakChinese");
  }
}
```

------

思考：接口和类型别名的区别



实际上，在大多数的情况下使用接口类型和类型别名的效果等价，但是在某些特定的场景下这两者还是存在很大区别。



1.基础数据类型 与接口不同，类型别名还可以用于其他类型，如基本类型（原始值）、联合类型、元组



```plain
// primitive
type Name = string;

// union
type PartialPoint = PartialPointX | PartialPointY;

// tuple
type Data = [number, string];
```



2.重复定义



接口可以定义多次 会被自动合并为单个接口 类型别名不可以重复定义



```plain
type myType = {
    name: string,
    age: number
};

// 重复标识符“myType”
type myType = {
    name: string,
    age: number
}; 

interface Point {
  x: number;
}
interface Point {
  y: number;
}
const point: Point = { x: 1, y: 2 };
```



3.扩展 接口可以扩展类型别名，同理，类型别名也可以扩展接口。但是两者实现扩展的方式不同



接口的扩展就是继承，通过 extends 来实现。类型别名的扩展就是交叉类型，通过 & 来实现。



```plain
// 接口扩展接口
interface PointX {
  x: number;
}

interface Point extends PointX {
  y: number;
}
// -------------------------------------
// 类型别名扩展类型别名
type PointY = {
  x: number;
};

type Point1 = PointY & {
  y: number;
};

// -------------------------------------
// 接口扩展类型别名
type PointZ = {
  x: number;
};
interface Point2 extends PointZ {
  y: number;
}
// -------------------------------------
// 类型别名扩展接口
interface PointG {
  x: number;
}
type Point3 = PointG & {
  y: number;
};
```



4.实现 这里有一个特殊情况 类无法实现定义了联合类型的类型别名



```plain
type PartialPoint = { x: number } | { y: number };

//  类只能实现对象类型或对象类型与静态已知成员的交集
class SomePartialPoint implements PartialPoint {
  // Error
  x = 1;
  y = 2;
}
```



#### 5. 接口和抽象类的区别



##### 1. 相同点



1. 抽象类和接口都不能被实例化。
2. 继承抽象类和实现接口都要对其中的抽象方法进行全部实现。



##### 2. 不同点



1. 接口和抽象类的概念不一样。接口是对动作的抽象，抽象类是对根源的抽象。
2. 接口中只能包含抽象方法；而抽象类中可以包含普通方法。
3. 接口中不能定义静态方法；而抽象类可以定义静态方法。
4. 接口中只能定义静态常量，不能定义普通属性；而抽象类既 可以定义普通属性，也可以定义静态常量属性。
5. 接口不包含构造器；而抽象类中可以包含构造器。抽象类中的构造器并不是用于创建对象，而是让子类调用这些构造器来完成属于抽象类的初始化工作。
6. 接口中不包含初始化块；而抽象类可以包含初始化块。
7. 一个类最多有一个直接父类，包括抽象类；但是一个类可以直接实现多个接口。
8. 接口只可以继承一个或多个其它接口；抽象类可以继承一个类和实现多个接口。
9. 接口方法默认修饰符是public，可以使用其它修饰符。而抽象方法可以有public、protected和private这些修饰符。
10. 接口完全是抽象的，它根本不存在方法的实现；而抽象类可以有默认的方法实现；
11. 接口强调特定功能的实现；而抽象类强调所属关系。



### 十三、泛型



泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性



为了更好的了解泛型的作用 我们可以看下面的一个例子



```plain
function createArray(length: number, value: any): any[] {
  let result = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}

console.log(createArray(3, "x")); // ['x', 'x', 'x']
```



上述这段代码用来生成一个长度为 length 值为 value 的数组 但是我们其实可以发现一个问题 不管我们传入什么类型的 value 返回值的数组永远是 any 类型 如果我们想要的效果是 我们预先不知道会传入什么类型 但是我们希望不管我们传入什么类型 我们的返回的数组的指里面的类型应该和参数保持一致 那么这时候 泛型就登场了



使用**泛型**改造



```plain
function createArray<T>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}

console.log(createArray<string>(3, "x")); // ['x', 'x', 'x']
```



我们可以使用<>的写法 然后再面传入一个变量 T 用来表示后续函数需要用到的类型 当我们真正去调用函数的时候再传入 T 的类型就可以解决很多预先无法确定类型相关的问题



#### 1. 多个类型参数



如果我们需要有多个未知的类型占位 那么我们可以定义任何的字母来表示不同的类型参数



```plain
function swap<T, U>(tuple: [T, U]): [U, T] {
  return [tuple[1], tuple[0]];
}

console.log(swap([7, "seven"])); // ['seven', 7]
```



#### 2. 泛型约束



在函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性或方法：



```plain
function loggingIdentity<T>(arg: T): T {
  console.log(arg.length);
  return arg;
}

// Property 'length' does not exist on type 'T'.
```



上例中，泛型 T 不一定包含属性 length，所以编译的时候报错了。



这时，我们可以对泛型进行约束，只允许这个函数传入那些包含 length 属性的变量。这就是**泛型约束**



```plain
interface Lengthwise {
  length: number;
}

// T extends Lengthwise 表示泛型T必须时Lengthwise实现类（子类）
function loggingIdentity<T extends Lengthwise>(arg: T): number {
  return arg.length;
}

loggingIdentity('3')
loggingIdentity(3)  // 报错
```



注意：我们在泛型里面使用`extends`关键字代表的是泛型约束 需要和类的继承区分开



#### 3. 泛型接口



定义接口的时候也可以指定泛型



```plain
interface Cart<T> {
  list: T[];
}
let cart: Cart<{ name: string; price: number }> = {
  list: [{ name: "hello", price: 10 }],
};
console.log(cart.list[0].name, cart.list[0].price);
```



我们定义了接口传入的类型 T 之后返回的对象数组里面 T 就是当时传入的参数类型



#### 4. 泛型类



```plain
class Test<T>{
    name: T;
    constructor(name: T) {
        this.name = name;
    }
}

const test = new Test<string>('猴子');
console.log(test)
```



#### 5. 泛型类型别名



```plain
type Cart<T> = { list: T[] } | T[];
let c1: Cart<string> = { list: ["1"] };
let c2: Cart<number> = [1];
```



#### 6. 泛型参数的默认类型



我们可以为泛型中的类型参数指定默认类型。当使用泛型时没有在代码中直接指定类型参数，从实际值参数中也无法推测出时，这个默认类型就会起作用



```plain
function createArray<T = string>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}
console.log(createArray(2,'d'))
```



#### 7. 泛型变量



对刚接触 TypeScript 泛型的小伙伴来说，看到 T 和 E，还有 K 和 V 这些泛型变量时，估计会一脸懵逼。其实这些大写字母并没有什么本质的区别，只不过是一个约定好的规范而已。也就是说使用大写字母 A-Z 定义的类型变量都属于泛型，把 T 换成 A，也是一样的。下面我们介绍一下一些常见泛型变量代表的意思：



- T（Type）：表示一个 TypeScript 类型
- K（Key）：表示对象中的键类型
- V（Value）：表示对象中的值类型
- E（Element）：表示元素类型



### 十四、编译



#### 1. 编译文件



1. 自动编译文件



编译文件时，使用 -w 指令后，TS编译器会自动监视文件的变化，并在文件发生变化时对文件进行重新编译。



```plain
 tsc index.ts -w
```



1.  自动编译整个项目 

- - 如果直接使用tsc指令，则可以自动将当前项目下的所有ts文件编译为js文件。
  - 但是能直接使用tsc命令的前提时，要先在项目根目录下创建一个ts的配置文件 tsconfig.json
  - tsconfig.json是一个JSON文件，添加配置文件后，只需只需 tsc 命令即可完成对整个项目的编译



#### 2. tsconfig.json 的作用



- 用于标识 TypeScript 项目的根路径；
- 用于配置 TypeScript 编译器；
- 用于指定编译的文件。



#### 3. tsconfig.json 重要字段



- files - 设置要编译的文件的名称；
- include - 设置需要进行编译的文件，支持路径模式匹配；
- exclude - 设置无需进行编译的文件，支持路径模式匹配；
- compilerOptions - 设置与编译流程相关的选项。



```plain
{
  /*
  tsconfig.json是ts编译器的配置文件，ts编译器可以根据它的信息来对代码进行编译
    "include" 用来指定哪些ts文件需要被编译
      路径：** 表示任意目录
            * 表示任意文件
    "exclude" 不需要被编译的文件目录
        默认值：["node_modules", "bower_components", "jspm_packages"]

*/
  "include": [
    "./src/**/*"
  ],
  "exclude": [node_modules],
  "files": [
    "core.ts",
    "tsc.ts"
  ],
  /*
    compilerOptions 编译器的选项
  */
  "compilerOptions": {}
}
```



#### 4. compilerOptions 选项



compilerOptions 支持很多选项，常见的有 `baseUrl`、 `target`、`baseUrl`、 `moduleResolution` 和 `lib` 等。



compilerOptions 每个选项的详细说明如下：



```plain
{
  "compilerOptions": {

    /* 基本选项 */
    "target": "es5",                       // 指定ts被编译成es的版本本: 'ES3' (default), 'ES5', 'ES6'/'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'
    "module": "commonjs",                  // 指定使用模块: 'commonjs', 'amd', 'system', 'umd' or 'es2015'
    "lib": [],                             // 指定要包含在编译中的库文件
    "allowJs": true,                       // 是否对js文件进行编译，默认是false
    "checkJs": true,                       // 是否检查js代码是否符合语法规范，默认是false
    "jsx": "preserve",                     // 指定 jsx 代码的生成: 'preserve', 'react-native', or 'react'
    "declaration": true,                   // 生成相应的 '.d.ts' 文件
    "sourceMap": true,                     // 生成相应的 '.map' 文件
    "outFile": "./",                       // 将输出文件合并为一个文件
    "outDir": "./",                        // 指定输出目录
    "rootDir": "./",                       // 用来控制输出目录结构 --outDir.
    "removeComments": true,                // 删除编译后的所有的注释
    "noEmit": true,                        // 不生成编译后的文件
    "importHelpers": true,                 // 从 tslib 导入辅助工具函数
    "isolatedModules": true,               // 将每个文件做为单独的模块 （与 'ts.transpileModule' 类似）.

    /* 严格的类型检查选项 */
    "strict": true,                        // 启用所有严格类型检查选项
    "noImplicitAny": true,                 // 在表达式和声明上有隐含的 any类型时报错
    "strictNullChecks": true,              // 启用严格的 null 检查
    "noImplicitThis": true,                // 当 this 表达式值为 any 类型的时候，生成一个错误
    "alwaysStrict": true,                  // 以严格模式检查每个模块，并在每个文件里加入 'use strict'

    /* 额外的检查 */
    "noUnusedLocals": true,                // 有未使用的变量时，抛出错误
    "noUnusedParameters": true,            // 有未使用的参数时，抛出错误
    "noImplicitReturns": true,             // 并不是所有函数里的代码都有返回值时，抛出错误
    "noFallthroughCasesInSwitch": true,    // 报告 switch 语句的 fallthrough 错误。（即，不允许 switch 的 case 语句贯穿）

    /* 模块解析选项 */
    "moduleResolution": "node",            // 选择模块解析策略： 'node' (Node.js) or 'classic' (TypeScript pre-1.6)
    "baseUrl": "./",                       // 用于解析非相对模块名称的基目录
    "paths": {},                           // 模块名到基于 baseUrl 的路径映射的列表
    "rootDirs": [],                        // 根文件夹列表，其组合内容表示项目运行时的结构内容
    "typeRoots": [],                       // 包含类型声明的文件列表
    "types": [],                           // 需要包含的类型声明文件名列表
    "allowSyntheticDefaultImports": true,  // 允许从没有设置默认导出的模块中默认导入。

    /* Source Map Options */
    "sourceRoot": "./",                    // 指定调试器应该找到 TypeScript 文件而不是源文件的位置
    "mapRoot": "./",                       // 指定调试器应该找到映射文件而不是生成文件的位置
    "inlineSourceMap": true,               // 生成单个 soucemaps 文件，而不是将 sourcemaps 生成不同的文件
    "inlineSources": true,                 // 将代码与 sourcemaps 生成到一个文件中，要求同时设置了 --inlineSourceMap 或 --sourceMap 属性

    /* 其他选项 */
    "experimentalDecorators": true,        // 启用装饰器
    "emitDecoratorMetadata": true          // 为装饰器提供元数据的支持
  }
}
```