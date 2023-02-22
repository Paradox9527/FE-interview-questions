## JavaScript

### JS中的8种数据类型及区别

----

包括值类型(基本对象类型)和引用类型(复杂对象类型)

**基本类型(值类型)：** Number(数字),String(字符串),Boolean(布尔),Symbol(符号),null(空),undefined(未定义)在内存中占据固定大小，保存在栈内存中

**引用类型(复杂数据类型)：** Object(对象)、Function(函数)。其他还有Array(数组)、Date(日期)、RegExp(正则表达式)、特殊的基本包装类型(String、Number、Boolean) 以及单体内置对象(Global、Math)等 引用类型的值是对象 保存在堆内存中，栈内存存储的是对象的变量标识符以及对象在堆内存中的存储地址。

**使用场景：**

Symbol：使用Symbol来作为对象属性名(key)  利用该特性，把一些不需要对外操作和访问的属性使用Symbol来定义

BigInt：由于在 Number 与 BigInt 之间进行转换会损失精度，因而建议仅在值可能大于253 时使用 BigInt 类型，并且不在两种类型之间进行相互转换。

传送门 ☞[# JavaScript 数据类型之 Symbol、BigInt](https://juejin.cn/post/7000754813801775111)



### null和undefined的区别

----

- null表示一个对象被定义了，值为空，typeof是一个object，原型链终点是null
- undefined表示不存在这个值，变量声明了，但是没有赋值，为undefined。



### 深拷贝和浅拷贝

----

- 浅拷贝：属性是基本类型，则拷贝的是基本类型的值；如果为引用类型，拷贝的就是内存地址；拷贝一层，深层次的的引用类型共享了内存地址。常见方法：object.assgin，扩展运算符等

```javascript
var a = { count: 1, deep: { count: 2 } };
var b = Object.assign({}, a);
// 或者
var c = {...a}
// 实现一个浅拷贝 对象的话
function shallowClone(obj) {
  const newObj = {};
  for (let prop in obj) {
    if (obj.hasOwnProperty(prop)) {
      newObj[prop] = obj[prop];
    }
  }
  return newObj
}

```

- 深拷贝：开辟一个新栈，两个对象的属性完全相同，但是对应两个不同的地址，修改一个对象的属性，不会改变另一个对象的属性。方法有JSON

```javascript
// 简单的实现
var a = { 
	key1: {
        subKey: 1
	}
};
var b = JSON.parse(JSON.stringify(a));
b.key1.subKey = 2
console.log(a)
// 输出结果： { key1: { subKey: 1 } }

		// 复杂点的
		/*
         deep clone
         @param {[type]} parent object 需要进行克隆的对象
         @return {[type]}  深克隆后的对象 
        */
        const clone = parent => {
            // 判断类型
            const isType = (obj, type) => {
                if (typeof obj !== "object") return false
                const typeString = Object.prototype.toString.call(obj)
                let flag
                switch (type) {
                    case "Array":
                        flag = typeString === "[object Array]"
                        break;
                    case "Date":
                        flag = typeString === "[object Date]"
                        break;
                    case "RegExp":
                        flag = typeString === "[object RegExp]"
                        break;
                    default:
                        flag = false;
                }
                return flag
            };

            // 处理正则
            const getRegExp = re => {
                var flags = ""
                if (re.global) flags += "g"
                if (re.ignoreCase) flags += "i"
                if (re.multiline) flags += "m"
                return flags
            };

            // 维护两个储存循环引用的数组
            const parents = []
            const children = []

            const _clone = parent => {
                if (parent === null) return null
                if (typeof parent !== "object") return parent

                let child, proto

                if (isType(parent, "Array")) {
                    // 对数组做特殊处理
                    child = []
                } else if (isType(parent, "RegExp")) {
                    // 对正则对象做特殊处理
                    child = new RegExp(parent.source, getRegExp(parent))
                    if (parent.lastIndex) child.lastIndex = parent.lastIndex
                } else if (isType(parent, "Date")) {
                    // 对Date对象做特殊处理
                    child = new Date(parent.getTime())
                } else {
                    // 处理对象原型
                    proto = Object.getPrototypeOf(parent)
                    // 利用object.create切断原型链
                    child = Object.create(proto)
                }

                // 处理循环引用
                // 返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。
                const index = parents.indexOf(parent)

                if (index != -1) {
                    // 如果父数组存在本对象，说明之前已经被引用过，直接返回此对象
                    return children[index]
                }
                parents.push(parent)
                children.push(child)

                for (let i in parent) {
                    // 递归
                    child[i] = _clone(parent[i])
                }

                return child
            }
            return _clone(parent)
        }
```



### JS中的数据类型检测方案

----

#### 1.typeof

```js
console.log(typeof 1);               // number
console.log(typeof true);            // boolean
console.log(typeof 'mc');            // string
console.log(typeof Symbol)           // function
console.log(typeof function(){});    // function
console.log(typeof console.log());   // function
console.log(typeof []);              // object 
console.log(typeof {});              // object
console.log(typeof null);            // object
console.log(typeof undefined);       // undefined
```

优点：能够快速区分基本数据类型

缺点：不能将Object、Array和Null区分，都返回object

识别Null可用全等运算符===来判断

#### 2.instanceof

根据原型链中查找当前数据的原型对象是否存在并返回布尔类型的值，、

用于判断一个引用类型是否属于某构造函数，还可以在继承关系中用来判断一个实例是否属于它的父类型。

```js
console.log(1 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false  
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
```

优点：能够区分Array、Object和Function，适合用于判断自定义的类实例对象

缺点：Number，Boolean，String基本数据类型不能判断，即不能正确判断基本数据类型

#### 3.Object.prototype.toString.call()

```js
var toString = Object.prototype.toString;
console.log(toString.call(1));      //[object Number]
console.log(toString.call(true));   //[object Boolean]
console.log(toString.call('mc'));   //[object String]
console.log(toString.call([]));     //[object Array]
console.log(toString.call({}));     //[object Object]
console.log(toString.call(function(){})); //[object Function]
console.log(toString.call(undefined));  //[object Undefined]
console.log(toString.call(null)); //[object Null]
```

优点：精准判断数据类型

缺点：写法繁琐不容易记，推荐进行封装后使用

#### 4.Array.isArray

可以判断value是否为数组

```js
Array.isArray([]); // true
Array.isArray({}); // false
Array.isArray(1); // false
Array.isArray('string'); // false
Array.isArray(true); // false
```

#### instanceof 和 typeof 的区别：
typeof在对值类型number、string、boolean 、null 、 undefined、 以及引用类型的function的反应是精准的；但是，对于对象{ } 、数组[ ] 、null 都会返回object

为了弥补这一点，instanceof 从原型的角度，来判断某引用属于哪个构造函数，从而判定它的数据类型。



### var && let && const

----

ES6之前创建变量用的是var,之后创建变量用的是let/const

**三者区别**：

1.  var定义的变量，`没有块的概念，可以跨块访问`, 不能跨函数访问。\
    let定义的变量，只能在块作用域里访问，不能跨块访问，也不能跨函数访问。\
    const用来定义常量，使用时必须初始化(即必须赋值)，只能在块作用域里访问，且不能修改。
1.  var可以`先使用，后声明`，因为存在变量提升；let必须先声明后使用。
1.  var是允许在相同作用域内`重复声明同一个变量`的，而let与const不允许这一现象。
1.  在全局上下文中，基于let声明的全局变量和全局对象GO（window）没有任何关系 ;\
    var声明的变量会和GO有映射关系；
1.  `会产生暂时性死区`：

> 暂时性死区是浏览器的bug：检测一个未被声明的变量类型时，不会报错，会返回undefined\
> 如：console.log(typeof a) //undefined\
> 而：console.log(typeof a)//未声明之前不能使用\
> let a

6.  let /const/function会把当前所在的大括号(除函数之外)作为一个全新的块级上下文，应用这个机制，在开发项目的时候，遇到循环事件绑定等类似的需求，无需再自己构建闭包来存储，只要基于let的块作用特征即可解决



### == 和 ===的区别

----

- == 比较值是否相等，类型不同时会发生类型的隐式转换，转换为相同类型再比较
- === 等同符，数据类型相同时，比较等号两边的值。相同就是ture；不同直接返回false。



### 如何遍历对象的属性？

----

- 遍历自身可枚举的属性（可枚举、非继承属性）：Object.keys()，该方法会返回一个由给定对象的自身可枚举属性组成的数组。
- 遍历自身所有的属性（可枚举、不可枚举、非继承属性）：Object.getOwnPropertyNames()，该方法会返回一个由指定对象的所有自身属性组成的数组
- 遍历可枚举的自身属性和继承属性：for ...in ...



### 如何判断两个对象是否相等？

----

1. Object.is(obj1, obj2)，判断两个对象都引用地址是否一致，true则一致，false不一致。
2. 判断两个对象内容是否一致，思路是遍历对象的所有键名和键值是否都一致。
   1. 判断两个对象是否指向同一内存。
   2. 使用`Object.getOwnPropertyNames` 获取对象所有键名数组
   3. 判断连个对象的键名数组是否相等，再遍历键名，判断键值是否都相等

```javascript
// 实现例子
function isObjValueEqual(a, b) {
  // 判断两个对象是否指向同一内存，指向同一内存返回 true
  if (a === b) return true;
  // 获取两个对象的键名数组
  let aProps = Object.getOwnPropertyNames(a);
  let bProps = Object.getOwnPropertyNames(b);
  // 判断两键名数组长度是否一致，不一致返回 false
  if (aProps.length !== bProps.length) return false;
  // 遍历对象的键值
  for (let prop in a) {
    // 判断 a 的键名，在 b 中是否存在，不存在，直接返回 false
    if (b.hasOwnProperty(prop)) {
      // 判断 a 的键值是否为对象，是对象的话需要递归；
      // 不是对象，直接判断键值是否相等，不相等则返回 false
      if (typeof a[prop] === 'object') {
        if (!isObjValueEqual(a[prop], b[prop])) return false;
      } else if (a[prop] !== b[prop]){
        return false
      }
    } else {
      return false
    }
  }
  return true;
}
```



### 强制类型转换和隐式转换有哪些

----

- 强制：

  转换为字符串：toString()、String()

  转换成数字：Number()、parseInt()、parseFloat()

  转换为布尔类型： Boolean()

- 隐式：

  拼接字符串：let str = 1 + “” 

### 作用域和作用域链

----

作用域：即函数创建的时候的那种上下文（变量作用域）。函数生效（能被访问）的区域。作用域决定了代码区块中变量和其他资源的可见性。**全局作用域、局部作用域（函数作用域）、块级作用域**

定义：简单来说作用域就是变量与函数的可访问范围，`由当前环境与上层环境的一系列变量对象组成`\

1. 全局作用域：代码在程序的任何地方都能被访问，window 对象的内置属性都拥有全局作用域。

2. 函数作用域：在固定的代码片段才能被访问，也叫函数作用域。在函数内部能够访问

3. 块级作用域：代码块就可以划分变量的作用域。

作用：作用域最大的用处就是`隔离变量`，不同作用域下同名变量不会有冲突。

**作用域链参考链接**一般情况下，变量到 创建该变量 的函数的作用域中取值。但是如果在当前作用域中没有查到，就会向上级作用域去查，直到查到全局作用域，这么一个查找过程形成的链条就叫做作用域链。

详细阅读：[JavaScript深入之词法作用域和动态作用域](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmqyqingfeng%2FBlog%2Fissues%2F3)



### 原型 && 原型链

----

**原型关系：**

- 每个 class都有显示原型 prototype
- 每个实例都有隐式原型 `__proto__`
- 实例的 `__proto__` 指向对应 class 的 prototype
- 原型对象的constructor指向构造函数本身

‌ **原型:**  在 JS 中，每当定义一个对象（函数也是对象）时，对象中都会包含一些预定义的属性。其中每个`函数对象`都有一个`prototype` 属性，这个属性指向函数的`原型对象`。

**原型链**：函数的原型链对象constructor默认指向函数本身，原型对象除了有原型属性外，为了实现继承，还有一个原型链指针`__proto__`,该指针是指向上一层的原型对象，而上一层的原型对象的结构依然类似。因此可以利用`__proto__`一直指向Object的原型对象上，而Object原型对象用Object.prototype.__ proto__ = null表示原型链顶端。如此形成了js的原型链继承。同时所有的js对象都有Object的基本防范

通俗的说就是：当一个对象在查找一个属性的时候，找不到，就会根据`__proto__`属性向它的原型去找，还是没有的话，再在它的原型的原型找，知道查到Object.prototype.`__proto__`是Null了，这样就形成了原型链

**特点:**  `JavaScript`对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。

```
f1 = new Foo() ---->_proto_ -----> Foo.prototype
function Foo()-----> prototype -----> Foo.prototype
Foo.prototype -----> constructor -----> function Foo()
```

详细阅读：[轻松理解JS 原型原型链](https://juejin.cn/post/6844903989088092174)



### JS的预解析

----

游览器是使用JS解析器来执行js代码的。分为两个过程：**预解析过程和代码执行过程**。预解析分为**变量预解析（变量提升）**和**函数预解析（函数提升）**；

- 变量提升：声明提升到当前作用域的最前面，只提升声明，不提升赋值；
- 函数提升：同上，不提示调用

**函数表达式的写法不存在函数提升**

**函数提升优先级高于变量提升，即函数提升在变量提升之上，且不会被同名变量声明时覆盖，但是会被同名变量赋值后覆盖**



### 数组Array的一些方法，以及部分区别

----

##### Array.from()和Array.of()的使用以及区别？

------

- `Array.from()`：将伪数组对象或可遍历对象转换为真数组。接受三个参数：input、map、context。

input：待转换的伪数组对象或可遍历对象；

map：类似于数组的 map 方法，用来对每个元素进行处理，将处理后的值放入返回的数组；

context：绑定map中用到的 this。

```javascript
console.log(Array.from('foo'));
// Expected output: Array ["f", "o", "o"]

console.log(Array.from([1, 2, 3], x => x + x));
// Expected output: Array [2, 4, 6]
```

- `Array.of()`：将一系列值转换成数组，会创建一个包含所有传入参数的数组，而不管参数的数量与类型，解决了`new Array()`行为不统一的问题。

- `Array.prototype.reduce()`：逐个遍历当前元素的值与上一步的计算结果相加（上一步的计算结果是当前元素之前所有元素的总和），直到没有更多的元素被相加；

```javascript
const array1 = [1, 2, 3, 4];

// 0 + 1 + 2 + 3 + 4
const initialValue = 0;
const sumWithInitial = array1.reduce(
  (accumulator, currentValue) => accumulator + currentValue,
  initialValue
);

console.log(sumWithInitial);
// Expected output: 10

```

- `Array.prototype.push()`：将一个或多个元素添加到数组的末尾，**并返回该数组的新长度**

- `Array.prototype.flat()`：按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。

```javascript
const arr1 = [0, 1, 2, [3, 4]];

console.log(arr1.flat());
// Expected output: Array [0, 1, 2, 3, 4]

const arr2 = [0, 1, 2, [[[3, 4]]]];

console.log(arr2.flat(2));
// Expected output: Array [0, 1, 2, Array [3, 4]]
```

- `Array.prototype.findIndex()`：返回数组中满足提供的测试函数的第一个元素的索引。若没有找到对应元素则返回-1

```javascript
const array1 = [5, 12, 8, 130, 44];

const isLargeNumber = (element) => element > 13;

console.log(array1.findIndex(isLargeNumber));
// Expected output: 3
```

- `Array.prototype.entries()`：返回一个新的数组迭代器对象，该对象包含数组中每个索引的键/值对

```javascript
const array1 = ['a', 'b', 'c'];

const iterator1 = array1.entries();

console.log(iterator1.next().value);
// Expected output: Array [0, "a"]

console.log(iterator1.next().value);
// Expected output: Array [1, "b"]

// 示例
const array = ["a", "b", "c"];
const arrayEntries = array.entries();

for (const element of arrayEntries) {
  console.log(element);
}

// [0, 'a']
// [1, 'b']
// [2, 'c']

```

- `Array.prototype.every()`：测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值。

```javascript
const isBelowThreshold = (currentValue) => currentValue < 40;

const array1 = [1, 30, 39, 29, 10, 13];

console.log(array1.every(isBelowThreshold));
// Expected output: true
```

- `Array.prototype.fill()`：用一个固定值填充一个数组从起始索引到终止索引内的全部元素，不包括终止索引

```javascript
const array1 = [1, 2, 3, 4];

// Fill with 0 from position 2 until position 4
console.log(array1.fill(0, 2, 4));
// Expected output: Array [1, 2, 0, 0]

// Fill with 5 from position 1
console.log(array1.fill(5, 1));
// Expected output: Array [1, 5, 5, 5]

console.log(array1.fill(6));
// Expected output: Array [6, 6, 6, 6]

```

- `Array.prototype.splice()`：删除或替换现有元素或者原地添加新的元素来修改数组，并以数组形式返回被修改的内容。此方法会改变原数组

```javascript
const months = ['Jan', 'March', 'April', 'June'];
months.splice(1, 0, 'Feb');
// Inserts at index 1
console.log(months);
// Expected output: Array ["Jan", "Feb", "March", "April", "June"]

months.splice(4, 1, 'May');
// Replaces 1 element at index 4
console.log(months);
// Expected output: Array ["Jan", "Feb", "March", "April", "May"]
```



#### String.prototype.（字符串的一些方法）

----

- String.prototype.padStart()：用另一个字符串填充当前字符串（如果需要的话，会重复多次），以便产生的字符串达到给定的长度。从字符串的左侧开始填充。

  ```javascript
  const str1 = '5';
  
  console.log(str1.padStart(2, '0'));
  // Expected output: "05"
  
  const fullNumber = '2034399002125581';
  const last4Digits = fullNumber.slice(-4);
  const maskedNumber = last4Digits.padStart(fullNumber.length, '*');
  
  console.log(maskedNumber);
  // Expected output: "************5581"
  ```

- String.raw()：模板字符串的标签函数，用来获取一个模板字符串的原始字符串。转义字符不会



#### Object. 方法

----

- Object.entries()：返回一个给定对象自身可枚举属性的键值对数组，其排列与使用for...in循环遍历该对象时返回的顺序一致（区别在于for-in循环还会枚举原型链的属性）

  ```javascript
  const object1 = {
    a: 'somestring',
    b: 42
  };
  
  for (const [key, value] of Object.entries(object1)) {
    console.log(`${key}: ${value}`);
  }
  
  // Expected output:
  // "a: somestring"
  // "b: 42"
  ```


- `Object.seal()`：封闭一个对象，阻止添加新属性并将所有现有属性标记为不可配置。当前属性的值只要原来是可写的就可以改变。

  ```javascript
  const object1 = {
    property1: 42
  };
  
  Object.seal(object1);
  object1.property1 = 33;
  console.log(object1.property1);
  // Expected output: 33
  
  delete object1.property1; // Cannot delete when sealed
  console.log(object1.property1);
  // Expected output: 33
  ```

- `Object.freeze()`：冻结一个对象。一个被冻结的对象再也不能被修改；不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性、以及不能修改已有属性的值。冻结后，该对象的原型也不能被修改。freeze()返回和传入的参数相同的对象

  ```javascript
  const obj = {
    prop: 42
  };
  
  Object.freeze(obj);
  
  obj.prop = 33;
  // Throws an error in strict mode
  
  console.log(obj.prop);
  // Expected output: 42
  
  ```

- `Object.fromEntries()`：把键值对列表转换为一个对象

  ```javascript
  const entries = new Map([
    ['foo', 'bar'],
    ['baz', 42]
  ]);
  
  const obj = Object.fromEntries(entries);
  
  console.log(obj);
  // Expected output: Object { foo: "bar", baz: 42 }
  ```




#### Math的方法

----

- `Math.floor()`：总是返回小于等于一个给定数字的最大整数

```javascript
console.log(Math.floor(5.95));
// Expected output: 5

console.log(Math.floor(5.05));
// Expected output: 5

console.log(Math.floor(5));
// Expected output: 5

console.log(Math.floor(-5.05));
// Expected output: -6
```



#### 纯函数

----

概念：输入输出数据流全是显式的。显示的意思是，函数与外界交换数据只有一个唯一渠道。参数与返回值：函数从函数外部接受的所有输入信息都通过参数传递到该函数内部；函数输出到函数外部的所有信息都通过返回值传递到该函数外部

```javascript
function sum(a, b) {
  return a + b;
}
```



### 闭包的两大作用：保存/保护

----

-   **闭包的概念**

    函数执行时形成的私有上下文，正常情况下，代码执行完会出栈后释放;但是特殊情况下，如果当前私有上下文中的某个东西被上下文以外的事物占用了，则上下文不会出栈释放，从而形成不销毁的上下文。 函数执行函数执行过程中，会形成一个全新的私有上下文，可能会被释放，可能不会被释放，不论释放与否。
    
    简要概括：函数嵌套，内层函数访问外层函数的内部变量时，导致外层函数的变量得不到释放
    
    他的作用是：
    
    （1）保护：划分一个独立的代码执行区域，在这个区域中有自己私有变量存储的空间，保护自己的私有变量不受外界干扰（操作自己的私有变量和外界没有关系）；
    
    （2）保存：如果当前上下文不被释放【只要上下文中的某个东西被外部占用即可】，则存储的这些私有变量也不会被释放，可以供其下级上下文中调取使用，相当于把一些值保存起来了；

我们把函数执行形成私有上下文，来保护和保存私有变量机制称为`闭包`。

> 闭包是指有权访问另一个函数作用域中的变量的函数--《JavaScript高级程序设计》

**稍全面的回答**： 在js中变量的作用域属于函数作用域, 在函数执行完后,作用域就会被清理,内存也会随之被回收,但是由于闭包函数是建立在函数内部的子函数, 由于其可访问上级作用域,即使上级函数执行完, 作用域也不会随之销毁, 这时的子函数(也就是闭包),便拥有了访问上级作用域中变量的权限,即使上级函数执行完后作用域内的值也不会被销毁。

-   **闭包的特性**：

    -   1、内部函数可以访问定义他们外部函数的参数和变量。(作用域链的向上查找，把外围的作用域中的变量值存储在内存中而不是在函数调用完毕后销毁)设计私有的方法和变量，避免全局变量的污染。

        1.1.闭包是密闭的容器，类似于set、map容器，存储数据的

        1.2.闭包是一个对象，存放数据的格式为 key-value 形式

    -   2、函数嵌套函数

    -   3、本质是将函数内部和外部连接起来。优点是可以读取函数内部的变量，让这些变量的值始终保存在内存中，不会在函数被调用之后自动清除

-   **闭包形成的条件**：

    1.  函数的嵌套
    1.  内部函数引用外部函数的局部变量，延长外部函数的变量生命周期

-   **闭包的用途**：

    1.  模仿块级作用域
    1.  保护外部函数的变量 能够访问函数定义时所在的词法作用域(阻止其被回收)
    1.  封装私有化变量
    1.  创建模块

-   **闭包应用场景**

    闭包的两个场景，闭包的两大作用：`保存/保护`。 在开发中, 其实我们随处可见闭包的身影, 大部分前端JavaScript 代码都是“事件驱动”的,即一个事件绑定的回调方法; 发送ajax请求成功|失败的回调;setTimeout的延时回调;或者一个函数内部返回另一个匿名函数,这些都是闭包的应用。

- **闭包的优点**：延长局部变量的生命周期

-   **闭包缺点**：会导致函数的变量一直保存在内存中，过多的闭包可能会导致内存泄漏

详细阅读：[我从来不理解JavaScript闭包，直到有人这样向我解释它](https://juejin.cn/post/6844903858636849159)



#### new操作符的实现

----

1. 创建了一个空对象。
2. 将空对象的原型，指向于构造函数的原型
3. 将空对象作为构造函数的上下文（就是改变this指向），执行构造函数（为这个新对象添加属性）
4. 判断函数的返回值，值类型？返回创建的对象；引用类型？返回这个引用类型的对象

```javascript
function myNew(context) { // 就是上面的4个步骤
	const obj = new Object();
	obj.__proto__ = context.prototype;
	const res = context.apply(obj, [...arguments].slice(1));
	return typeof res === "object" ? res : obj; 
    //  return res instanceof object ? res : obj;
}
```



### JS 中 this 的情况

----

概念：

使用函数时，自动生成的一个内部对象，只能在函数内部使用。随着函数的使用场合不同，this值会发生变化。**this指的是调用函数的那个对象**

1. 普通函数调用：通过函数名()直接调用：`this`指向`全局对象window`（注意let定义的变量不是window属性，只有window.xxx定义的才是。即let a =’aaa’; this.a是undefined）
2. 构造函数调用：函数作为构造函数，用new关键字调用时：`this`指向`新new出的对象`
3. 对象函数调用：通过对象.函数名()调用的：`this`指向`这个对象`
4. 箭头函数调用：箭头函数里面没有 this ，所以`永远是上层作用域this`（上下文）
5. apply和call调用：函数体内 this 的指向的是 call/apply 方法`第一个参数`，若为空默认是指向全局对象window。
6. 函数作为数组的一个元素，通过数组下标调用的：this指向这个数组
7. 函数作为window内置函数的回调函数调用：this指向window（如setInterval setTimeout 等）



### call/apply/bind 的区别

----

相同：

1、都是用来改变函数的this对象的指向的。

2、第一个参数都是this要指向的对象

3、都可以利用后续参数传参。

不同：

apply和call传入的参数列表形式不同。apply 接收 arguments（数组形式），call和bind接收一串参数列表
```javascript
fn.call(obj, 1, 2);
fn.apply(obj, [1, 2]);
```
bind：语法和call一模一样，区别在于立即执行还是等待执行，bind不兼容IE6~8
bind 主要就是将函数绑定到某个对象，bind()会创建一个函数，返回对应函数便于稍后调用；而apply、call则是立即调用。

总结：基于Function.prototype上的 ` apply 、 call 和 bind  `调用模式，这三个方法都可以显示的指定调用函数的 this 指向。`apply`接收参数的是数组，`call`接受参数列表，`` bind`方法通过传入一个对象，返回一个` this ` 绑定了传入对象的新函数。这个函数的  `this`指向除了使用`new `时会被改变，其他情况下都不会改变。若为空默认是指向全局对象window。

参考：☞ [call、apply、bind三者的用法和区别](https://blog.csdn.net/hexinyu_1022/article/details/82795517)

手写实现：

```javascript
	// call
	Function.prototype.myCall = function (context) {
		// 判断调用对象
		if (typeof this != "undefined") {
			throw new Error("type error");
		}
		// 首先获取参数 call接收一串参数 列表形式
		let args = [...arguments].slice(1);
		let res = null;
		
		// 判断context是否传入，如果没有，就设置为window
		context = context || window;
		// 将被调用的方法置入context的属性
		// this 即为要调用的方法
		context.fn = this;
		// 执行要被调用的方法
		res = context.fn(...args);
		// 删除手动增加的属性方法
		delete context.fn;
		// 执行结果返回
		return res;
	}

	// apply
	Function.prototype.myApply = function (context) {
		// 判断调用对象
		if (typeof this != "undefined") {
			throw new Error("type error");
		}
		let res = null;
		// 判断context是否传入，如果没有，就设置为window
		context = context || window;
		// 使用symbol 来保护属性唯一；保证不会重写用户自己原来定义在context中同名属性
		const fnSymbol = Symbol();
		context[fnSymbol] = this;
		// 执行被调用的方法
		if (arguments[1]) {
			res = context[fnSymbol](...arguments[1]);
		} else {
			res = context[fnSymbol]();
		}
		delete context[fnSymbol];
		return res
	}

	// bind
	// 1.返回一个函数，绑定this,传递预置参数
	// 2.bind返回的函数可以作为构造函数使用。故作为构造函数时应使得this失效，但是传入的参数依然有效
	Function.prototype.myBind = function (context) {
		if (typeof this != "undefined") {
			throw new Error("type error");
		}
		// 首先获取参数 call接收一串参数 列表形式
		let args = [...arguments].slice(1);
		const fn = this;
		const fNop = function() {};
		return function Fn() {
			return fn.apply(
				this instanceof Fn ? this : context,
				// 当前这个 arguments 是指 Fn 的参数
				args.concat(...arguments)
			)
		}
		// 维护原型关系
		if (this.prototype) {
			fNop.prototype = this.prototype;
		}
		fn.prototype = new fNop();
		return fn
	}

```



### 箭头函数的特性

----

1.  `箭头函数没有自己的this`，会捕获其所在的上下文的this值，作为自己的this值
1.  `箭头函数没有constructor`，是匿名函数，不能作为构造函数，不能通过new 调用；
1.  `没有new.target 属性`。在通过new运算符被初始化的函数或构造方法中，new.target返回一个指向构造方法或函数的引用。在普通的函数调用中，new.target 的值是undefined
1.  `箭头函数不绑定Arguments 对象`。取而代之用rest参数...解决。由于 箭头函数没有自己的this指针，通过 call() 或 apply() 方法调用一个函数时，只能传递参数（不能绑定this），他们的第一个参数会被忽略。（这种现象对于bind方法同样成立）
1. 箭头函数通过 call()  或   apply() 方法调用一个函数时，只传入了一个参数，对 this 并没有影响。使用bind方法时，只会返回一个预设参数的新函数，并不会改变这个新函数的this指向。
1. 箭头函数没有原型属性 Fn.prototype 值为 undefined
1. 箭头函数不能当做Generator函数,不能使用yield关键字

参考：[箭头函数与普通函数的区别](https://www.cnblogs.com/biubiuxixiya/p/8610594.html)



### JS中内存泄漏的几种情况

----

`内存泄漏`一般是指系统进程不再用到的内存，没有及时释放，造成内存资源浪费，导致程序运行速度减慢甚至系统崩溃等严重后果。

造成内存泄漏的原因有：

1. `全局变量。`在局部作用域中，函数执行完毕后，变量就没有存在的必要了，垃圾回收机制很快的做出判断并回收；但是对于全局变量，很难判断什么时候不用这些变量，无法正常回收。
    解决办法：
    ① 尽量少使用全局变量；
    ② 使用严格模式，在 js 文件头部或者函数的顶部加上`use strict`。
2. `闭包引起的内存泄露。`闭包可以读取函数内部的变量，然后让这些变量始终保存在内存中，如果在使用结束后没有将局部变量清除，就可能导致内存泄露。
    解决办法：将事件处理函数定义在外部，解除闭包。
3. `被遗忘的定时器。`定时器`setInterval`或者`setTimeout`不再需要使用时，且没有被清除，导致定时器的回调函数及其内部依赖的变量都不能被回收，就会造成内存泄漏。
    解决办法：当不需要定时器的时候，调用`clearInterval`或者`clearTimeout`手动清除。
4. `事件监听。`垃圾回收机制不好判断事件是否需要被解除，导致`callback`不能被释放，此时需要手动解除绑定。
    解决办法：及时使用`removeEventListener`移除事件监听。
5. `元素引用没有清理。`
    解决办法：移除元素后，手动设置元素的引用为`null`。
6. `console`。传递给`console.log`的对象是不能被垃圾回收，可能会存在内存泄漏。
    解决办法：清除不必要的`console`。



### EventLoop 事件循环

----

`JS`是单线程的，为了防止一个函数执行时间过长阻塞后面的代码，所以会先将同步代码压入执行栈中，依次执行，将异步代码推入异步队列，异步队列又分为宏任务队列和微任务队列，因为宏任务队列的执行时间较长，所以微任务队列要优先于宏任务队列。微任务队列的代表就是，`Promise.then`，`MutationObserver`，宏任务的话就是`setImmediate setTimeout setInterval`

JS运行的环境。一般为浏览器或者Node。 在浏览器环境中，有JS 引擎线程和渲染线程，且两个线程互斥。 Node环境中，只有JS 线程。 不同环境执行机制有差异，不同任务进入不同Event Queue队列。 当主程结束，先执行准备好微任务，然后再执行准备好的宏任务，一个轮询结束。

#### **浏览器中的事件环（Event Loop)**

事件环的运行机制是，先会执行栈中的内容，栈中的内容执行后执行微任务，微任务清空后再执行宏任务，先取出一个宏任务，再去执行微任务，然后在取宏任务清微任务这样不停的循环。

-   eventLoop 是由JS的宿主环境（浏览器）来实现的；

-   事件循环可以简单的描述为以下四个步骤:

    1.  函数入栈，当Stack中执行到异步任务的时候，就将他丢给WebAPIs,接着执行同步任务,直到Stack为空；
    1.  此期间WebAPIs完成这个事件，把回调函数放入队列中等待执行（微任务放到微任务队列，宏任务放到宏任务队列）
    1.  执行栈为空时，Event Loop把微任务队列执行清空；
    1.  微任务队列清空后，进入宏任务队列，取队列的第一项任务放入Stack(栈）中执行，执行完成后，查看微任务队列是否有任务，有的话，清空微任务队列。重复4，继续从宏任务中取任务执行，执行完成之后，继续清空微任务，如此反复循环，直至清空所有的任务。

    ![事件循环流程](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/342e581223d2471d9484fc48beb9f8e1~tplv-k3u1fbpfcp-zoom-1.image)

-   浏览器中的任务源(task):

    -   `宏任务(macrotask)`：\
        宿主环境提供的，比如浏览器\
        ajax、setTimeout、setInterval、setTmmediate(只兼容ie)、script、requestAnimationFrame、messageChannel、UI渲染、一些浏览器api
    -   `微任务(microtask)`：\
        语言本身提供的，比如promise.then\
        then、queueMicrotask(基于then)、mutationObserver(浏览器提供)、messageChannel 、mutationObersve

传送门 ☞ [# 宏任务和微任务](https://juejin.cn/post/7001881781125251086)



#### **Node 环境中的事件环（Event Loop)**

`Node`是基于V8引擎的运行在服务端的`JavaScript`运行环境，在处理高并发、I/O密集(文件操作、网络操作、数据库操作等)场景有明显的优势。虽然用到也是V8引擎，但由于服务目的和环境不同，导致了它的API与原生JS有些区别，其Event Loop还要处理一些I/O，比如新的网络连接等，所以Node的Event Loop(事件环机制)与浏览器的是不太一样。

![2020120317343116.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e362c1770f62428fbf3faabd99d2a64c~tplv-k3u1fbpfcp-zoom-1.image) 执行顺序如下：

-   `timers`: 计时器，执行setTimeout和setInterval的回调
-   `pending callbacks`: 执行延迟到下一个循环迭代的 I/O 回调
-   `idle, prepare`: 队列的移动，仅系统内部使用
-   `poll轮询`: 检索新的 I/O 事件;执行与 I/O 相关的回调。事实上除了其他几个阶段处理的事情，其他几乎所有的异步都在这个阶段处理。
-   `check`: 执行`setImmediate`回调，setImmediate在这里执行
-   `close callbacks`: 执行`close`事件的`callback`，一些关闭的回调函数，如：socket.on('close', ...)

详细阅读：[详解JavaScript中的Event Loop（事件循环）机制](https://link.juejin.cn/?target=https%3A%2F%2Fzhuanlan.zhihu.com%2Fp%2F33058983)

[微任务、宏任务与Event-Loop](https://juejin.cn/post/6844903657264136200)



### setTimeout、Promise、Async/Await 的区别

----

1.  setTimeout

    settimeout的回调函数放到宏任务队列里，等到执行栈清空以后执行。

1.  Promise

    Promise本身是**同步的立即执行函数**， 当在executor中执行resolve或者reject的时候, 此时是异步操作， 会先执行then/catch等，当主栈完成后，才会去调用resolve/reject中存放的方法执行。

    ```js
    console.log('script start')
    let promise1 = new Promise(function (resolve) {
        console.log('promise1')
        resolve()
        console.log('promise1 end')
    }).then(function () {
        console.log('promise2')
    })
    setTimeout(function(){
        console.log('settimeout')
    })
    console.log('script end')
    // 输出顺序: script start->promise1->promise1 end->script end->promise2->settimeout
    ```

1.  async/await

    async 函数返回一个 Promise 对象，当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再执行函数体内后面的语句。可以理解为，是让出了线程，跳出了 async 函数体。

    ```js
    async function async1(){
       console.log('async1 start');
        await async2();
        console.log('async1 end')
    }
    async function async2(){
        console.log('async2')
    }

    console.log('script start');
    async1();
    console.log('script end')

    // 输出顺序：script start->async1 start->async2->script end->async1 end
    ```
传送门 ☞ [# JavaScript Promise 专题](https://juejin.cn/post/6999651011304357925)

### Async/Await 如何通过同步的方式实现异步

Async/Await就是一个**自执行**的generate函数。利用generate函数的特性把异步的代码写成“同步”的形式,第一个请求的返回值作为后面一个请求的参数,其中每一个参数都是一个promise对象.



### 介绍节流防抖原理、区别以及应用

----

`节流`：事件触发后，规定时间内，事件处理函数不能再次被调用。也就是说在规定的时间内，函数只能被调用一次，且是最先被触发调用的那次。

`防抖`：多次触发事件，事件处理函数只能执行一次，并且是在触发操作结束时执行。也就是说，当一个事件被触发准备执行事件函数前，会等待一定的时间（这时间是自己去定义的，比如 1 秒），如果没有再次被触发，那么就执行，如果被触发了，那就本次作废，重新从新触发的时间开始计算，并再次等待 1 秒，直到能最终执行！

主要的区别是节流不清除定时器，就是只执行1次，防抖是要清除定时器的。两者都有闭包的应用

`使用场景`：

节流：滚动加载更多、搜索框搜的索联想功能、高频点击、表单重复提交……

防抖：搜索框搜索输入，并在输入完以后自动搜索、手机号，邮箱验证输入检测、窗口大小 resize 变化后，再重新渲染。

```js
/**
 * 节流函数 一个函数执行一次后，只有大于设定的执行周期才会执行第二次。有个需要频繁触发的函数，出于优化性能的角度，在规定时间内，只让函数触发的第一次生效，后面的不生效。
 * @param fn要被节流的函数
 * @param delay规定的时间
 */
function throttle(fn, delay) {
    //记录上一次函数触发的时间
    var lastTime = 0;
    return function(){
        //记录当前函数触发的时间
        var nowTime = Date.now();
        if(nowTime - lastTime > delay){
            //修正this指向问题
            fn.call(this);
            //同步执行结束时间
            lastTime = nowTime;
        }
    }
}

document.onscroll = throttle(function () {
    console.log('scllor事件被触发了' + Date.now());
}, 200); 

/**
 * 防抖函数  一个需要频繁触发的函数，在规定时间内，只让最后一次生效，前面的不生效
 * @param fn要被节流的函数
 * @param delay规定的时间
 */
function debounce(fn, delay) {
    //记录上一次的延时器
    var timer = null;
    return function () {
       //清除上一次的演示器
        clearTimeout(timer);
        //重新设置新的延时器
        timer = setTimeout(function(){
            //修正this指向问题
            fn.apply(this);
        }, delay); 
    }
}
document.getElementById('btn').onclick = debounce(function () {
    console.log('按钮被点击了' + Date.now());
}, 1000);
```



### Promise

----

`Promise`异步编程的一种解决方案。`Promise`是一个构造函数，接收一个函数作为参数，返回一个 Promise 实例。一个 Promise 实例有三种状态，分别是`pending`、`fulfilled`和`rejected`，分别代表了进行中、已成功和已失败。实例的状态只能由 pending 转变 fulfilled 或者 rejected 状态，并且状态一经改变，就无法再改变了。状态的改变是通过`resolve()`和`reject()`函数来实现的，可以在异步操作结束后调用这两个函数改变 Promise 实例的状态。Promise 的原型上定义了一个 `then`方法，使用这个 then 方法可以为两个状态的改变注册回调函数。这个回调函数属于微任务，会在本轮事件循环的末尾执行。

详细阅读：
 [Promise 对象](https://link.juejin.cn?target=https%3A%2F%2Fes6.ruanyifeng.com%2F%23docs%2Fpromise)

 [Promise不会？？看这里！！！史上最通俗易懂的Promise！！!](https://juejin.cn/post/6844903607968481287)

 [100 行代码实现 Promises/A+ 规范](https://link.juejin.cn?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FqdJ0Xd8zTgtetFdlJL3P1g)



#### ES10或TS3.7 可选链运算符(?.)

----

允许读取位于连接对象链深处的属性的值，而不必明确验证链中的每个引用是否有效。`?.`运算符的功能类似于`.`链式运算符，不同在于，在引用为空（nullish）(null或undefined)的情况不会报错，该表达式短路返回值是undefined。与函数调用一起使用时，如果给定的函数不存在，则返回undefined。

```javascript
const adventurer = {
  name: 'Alice',
  cat: {
    name: 'Dinah'
  }
};

const dogName = adventurer.dog?.name;
console.log(dogName);
// Expected output: undefined

console.log(adventurer.someNonExistentMethod?.());
// Expected output: undefined
```

