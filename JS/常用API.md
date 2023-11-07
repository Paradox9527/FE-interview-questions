## Number

- Number.isNaN()

确定传递的值是否为NaN，并且检查其类型是否为Number，返回一个boolean值。有一个全局的isNaN，那个只能判断是否为NaN。

- Number.isInteger()

判断给定的参数是否为整数。返回一个boolean值

- Number.isfinite()

检测传入的参数是否是一个有穷数。返回一个Boolean值。0是true，1/0是false

- Number.isSafeInteger()

判断传入的参数值是否是一个“安全整数” 2的53次方是false，2^53-1是true

- Number.prototype.toFixed()

使用定点表示法来格式化一个数值，参数是四舍五入小数点后几位介于0-20（包括）之间

12345.6789 用toFixed(1) => 12345.7

## String

- String.fromCharCode()

f返回指定的UTF-16码元序列创建的字符串。String.fromCharCode(198)

- String.raw()

模板字符串的标签函数。可以不带括号使用，但后面要跟模板字符串。任何类型的转义都会失效，保持原样输出

- String.prototype.charAt()

返回一个由给定索引处的单个UTF-16码元构成的新字符串

- String.prototype.charCodeAt()

返回一个整数，表示给定索引处的UTF-16码元，其值介于0和65535之间

- String.prototype.concat()

将字符串参数连接到调用的字符串，并返回一个新的字符串

- String.prototype.startsWith()

判断当前字符串是否以另外一个给定的子字符串开头，并根据判断结果返回true或false。第一个参数是字符串，第二个参数是位置即索引

- String.prototype.endsWith()

判断一个字符串是否以指定字符串结尾，返回true或者false。用法同上

- String.prototype.includes()

执行区分大小写的搜索，以确定是否可以在一个字符串中找到另一个字符串，并根据情况返回true或者false。用法：includes(searchString, position)

- String.prototype.indexOf()

在字符串中搜索指定子字符串，并返回其第一次出现的位置索引。它可以接受一个可选参数，指定搜索的起始位置，如果找到了，则返回的位置索引大于或等于指定的数字。没找到返回-1

- String.prototype.lastIndexOf()

搜索该字符串并返回指定子字符串最后一次出现的索引。可以接受一个可选的起始位置参数，并返回指定子字符串在小于或等于指定数字的索引中的最后一次出现的位置。没有找到值返回-1

```javascript
"canal".lastIndexOf("a"); // 返回 3
"canal".lastIndexOf("a", 2); // 返回 1
"canal".lastIndexOf("a", 0); // 返回 -1
```

- String.prototype.trim()
  
  从字符串的两端移除空白字符，并返回一个新的字符串，而不会修改原始字符串
  
  - 如何手写实现 trim
  
- String.prototype.trimStart()

从字符串的开头移除空白字符，并返回一个新的字符串，不会修改原始字符串。trimLeft方法是这个方法的别名。

- String.prototype.trimEnd()

从字符串的结尾移除空白字符，并返回一个新的字符串，而不会修改原始字符串。trimRight是该方法的别名。

- String.prototype.repeat()

构造并返回一个新的字符串，其中包含指定数量的所调用的字符串副本，这些副本连接在一起。

- String.prototype.replace()

返回一个新字符串，其中一个、多个或所有匹配的模式被替换掉了

```javascript
replace(pattern, replacement)
const p = 'The quick brown fox jumps over the lazy dog. If the dog reacted, was it really lazy?';

console.log(p.replace('dog', 'monkey'));
// Expected output: "The quick brown fox jumps over the lazy monkey. If the dog reacted, was it really lazy?"

const regex = /Dog/i;
console.log(p.replace(regex, 'ferret'));
// Expected output: "The quick brown fox jumps over the lazy ferret. If the dog reacted, was it really lazy?"

```

- String.prototype.slice()

提取字符串的一部分，并将其作为新字符串返回，而不修改原始字符串

```javascript
slice(indexStart, indexEnd)
```

- String.prototype.split()

接受一个模式，通过搜索模式将字符串分割成一个有序的子串列表，将这些子串放入一个数组，并返回该数组。

- String.prototype.substring()

返回该字符串从起始索引到结束索引（不包括）的部分，如果未提供结束索引，则返回到字符串末尾的部分。

- String.prototype.padStart()

用另一个字符串填充当前字符串（如果需要会重复填充），直到达到给定的长度。填充是从当前字符串的开头开始的。

```javascript
padStart(targetLength, padString) // 当前str填充后的长度。如果targetLength小于等于str.length,则会直接返回当前str。/ 填充当前str的字符串
```

- String.prototype.padEnd()

从末尾开始填充给定的字符串。用法同上

- String.prototype.search()

用于在string对象中执行正则表达式的搜索，寻找匹配项

- String.prototype.match()

检索与字符串与正则表达式进行匹配的结果。

- String.prototype.toLowerCase()

将字符串转换为小写

- String.prototype.toUpperCase()

将字符串转换为大写

## Array

- Array.isArray()

确定传递的值是否是一个Array

- Array.from()
  - 如何生成 `[0, 1, 2, ..., 99]` 的数组

将类数组对象创建一个新的浅拷贝的数组实例。

```javascript
let arrLike = {length:3}
const arr = Array.from(arrLike)
// 可选的
const mapFn = function() {
    .....
}
Array.from(arrayLike, mapFn, thisArg)
// thisArg 执行 mapFn 时用作 this 的值。
```

- Array.of()

通过可变数量的参数创建一个新的Array实例，而不考虑参数的数量或类型

- Array.prototype.slice()

返回一个新的数组对象，这一对象是一个由start和end决定的原数组的浅拷贝（包括start，不包括edn），其中start和end代表了数组元素的索引。原始数组不会被改变。

- Array.prototype.splice()

通过移除或者替换已存在的元素和/或添加新元素就地 改变一个数组的内容。会改变原数组

- Array.prototype.sort()

对数组的元素进行排序，并返回对相同数组的引用。会改变原数组

- Array.prototype.reverse()

反转数组中的元素，并返回同一数组的引用。数组的第一个元素会变成最后一个，数组的最后一个会变成第一个。就是翻转了。会改变原数组

- Array.prototype.indexOf

返回数组中第一次出现给定元素的下标，如果不存在则返回-1

- Array.prototype.lastIndexOf()

返回数组中给定元素最后一次出现的索引，如果不存在则返回-1.

- Array.prototype.includes()

判断一个数组是否包含一个指定的值，根据情况，如果包含则返回true，否则返回false。

- Array.prototype.push()

将指定的元素添加到数组的末尾，并返回新的数组长度。

- Array.prototype.pop()

从数组中删除最后一个元素，并返回该元素的值。会更改数组的长度

- Array.prototype.shift()

从数组中删除第一个元素，并返回该元素的值。此方法更改数组的长度

- Array.prototype.unshift()
  - 如何给数组首项添加元素

将指定元素添加到数组的开头，并返回数组的新长度

- Array.prototype.map()

创建一个新数组，这个新数组由原数组中的每个元素都调用一次提供的函数后的返回值组成

- Array.prototype.reduce()

对数组中的每个元素按序执行一个提供的reducer函数，每一次运行reducer会将先前元素的计算结果作为参数传入，最后将其结果汇总为单个返回值

第一次执行回调函数事，不存在“上一次的计算结果”。如果需要回调函数从数组索引为0的元素开始执行，则需要传递初始值。否则，数组索引为0的到元素将被用作初始值，迭代器将从第二个元素开始执行（即从索引为1而不是0的位置开始）

```javascript
const array1 = [1, 2, 3, 4];

// 0 + 1 + 2 + 3 + 4
const initialValue = 0;
const sumWithInitial = array1.reduce((accumulator, currentValue) => accumulator + currentValue, initialValue);

console.log(sumWithInitial);
// Expected output: 10

```

- Array.prototype.forEach()

对数组的每个元素执行一次给定的函数

- Array.prototype.filter()

创建给定数组一部分的浅拷贝，其包含通过所提供函数实现的测试的所有元素

- Array.prototype.every()

测试一个数组内的所有元素是否能通过指定函数的测试。它返回一个布尔值

- Array.prototype.some()

测试数组中是否至少有一个元素通过了由提供的函数实现的测试。如果在数组中找到一个元素使得提供的函数返回true，则返回true；否则false。它不会修改数组

- Array.prototype.flat()

创建一个新数组，并根据指定深度递归地将所有子数组元素拼接到新的数组中

```javascript
const arr1 = [0, 1, 2, [3, 4]];

console.log(arr1.flat());
// expected output: Array [0, 1, 2, 3, 4]

const arr2 = [0, 1, [2, [3, [4, 5]]]];

console.log(arr2.flat());
// expected output: Array [0, 1, 2, Array [3, Array [4, 5]]]

console.log(arr2.flat(2));
// expected output: Array [0, 1, 2, 3, Array [4, 5]]

console.log(arr2.flat(Infinity));
// expected output: Array [0, 1, 2, 3, 4, 5]

```



- Array.prototype.flatMap()

对数组中的每个元素应用给定的回调函数，然后将结果展开一级，返回一个新数组。它等价于在调用map()方法后再调用深度1的flat方法。但比分别调用这俩方法要高效一点

- Array.prototype.toString()

返回一个字符串，表示指定的数组及其元素

## Object

- Object.create()

静态方法以一个现有对象作为原型，创建一个新对象

- Object.assign()

将一个或多个源对象中所有可枚举的自有属性复制到目标对象，并返回修改后的目标对象。

- Object.defineProperties()

直接在一个对象上定义新的属性或修改现有属性，并返回该对象

- Object.defineProperty()

直接在一个对象上定义一个新属性，或修改其现有属性，并返回此对象

- Object.keys()

返回一个由给定对象自身的可枚举的字符串键属性名组成的数组

- Object.values()

返回一个给定对象的自有可枚举字符串键属性值组成的数组。

- Object.entries()

返回一个数组，包含给定对象自有的可枚举字符串键属性的键值对

```javascript
const object1 = {
  a: 'somestring',
  b: 42,
};

for (const [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}

// Expected output:
// "a: somestring"
// "b: 42"

```

- Object.fromEntries()

静态方法将键值对列表转换为一个对象。

```javascript
const entries = new Map([
  ['foo', 'bar'],
  ['baz', 42],
]);

const obj = Object.fromEntries(entries);

console.log(obj);
// Expected output: Object { foo: "bar", baz: 42 }
```

- Object.is()

确定两个值是否为相同值。可以用于判断-0和+0，两个NaN