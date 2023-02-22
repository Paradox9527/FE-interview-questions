### Vue

----

### MVVM模型？

----

MVVM，是`Model-View-ViewModel`的简写，其本质是`MVC`模型的升级版。其中 `Model` 代表数据模型，`View` 代表看到的页面，`ViewModel`是`View`和`Model`之间的桥梁，数据会绑定到`ViewModel`层并自动将数据渲染到页面中，视图变化的时候会通知`ViewModel`层更新数据。以前是通过操作`DOM`来更新视图，现在是`数据驱动视图`。

### Vue的生命周期

----

每个 Vue 组件实例在创建后都会经过一系列的初始化过程，这个过程中会运行叫做生命周期钩子的函数，以便于用户在特定的阶段有机会添加自己的代码。
 Vue 的生命周期可以分为8个阶段：创建前后、挂载前后、更新前后、销毁前后，以及一些特殊场景的生命周期。Vue 3 中还新增了是3个用于调试和服务端渲染的场景。

| Vue 2中的生命周期 | Vue 3中的生命周期 | 描述                                                   |
| ----------------- | ----------------- | ------------------------------------------------------ |
| `beforeCreate`    | `beforeCreate`    | 创建前，此时`data`和 `methods`的数据都还没有初始化     |
| `created`         | `created`         | 创建后，`data`中有值，尚未挂载，可以进行一些`Ajax`请求 |
| `beforeMount`     | `beforeMount`     | 挂载前，会找到虚拟`DOM`，编译成`Render`                |
| `mounted`         | `mounted`         | 挂载后，`DOM`已创建，可用于获取访问数据和`DOM`元素     |
| `beforeUpdate`    | `beforeUpdate`    | 更新前，可用于获取更新前各种状态                       |
| `updated`         | `updated`         | 更新后，所有状态已是最新                               |
| `beforeDestroy`   | `beforeUnmount`   | 销毁前，可用于一些定时器或订阅的取消                   |
| `destroyed`       | `unmounted`       | 销毁后，可用于一些定时器或订阅的取消                   |
| `activated`       | `activated`       | `keep-alive`缓存的组件激活时                           |
| `deactivated`     | `deactivated`     | `keep-alive`缓存的组件停用时                           |
| `errorCaptured`   | `errorCaptured`   | 捕获一个来自子孙组件的错误时调用                       |
| —                 | `renderTracked`   | 调试钩子，响应式依赖被收集时调用                       |
| —                 | `renderTriggered` | 调试钩子，响应式依赖被触发时调用                       |
| —                 | `serverPrefetch`  | 组件实例在服务器上被渲染前调用                         |

**vue3的组合式 api 中，setup 中的函数执行相当于在选项 api 中的 beforeCreate 和 created 中执行**

使用的时候不导入这两个了

**父子组件的生命周期：**

- `加载渲染阶段`：父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted
- `更新阶段`：父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated
- `销毁阶段`：父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed

**针对Vue 3中的生命周期做如下补充：**

vue 3中的生命周期图示：
 ![lifecycle.16e4c08e.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b3b3db43e7c44ac39676131fd2a17c4c~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

Vue 3中新引入了组合API，在组合API中，生命周期钩子通过调用onXxx()函数显式的进行注册。这些生命周期钩子注册函数只能在`setup()`期间同步使用，因为它们依赖内部全局状态定位当前活动实例【即其setup()正在被调用的组件实例】。在没有当前活动实例的情况下调用它们将导致错误。

组合API中对应的生命周期钩子注册函数的名字就是生命周期选项的名字首字母大写并添加前缀on。二者对应关系如下：

- beforeCreate 和 created 没有对应的onXxx()函数，取而代之使用setup()函数
- berforeMount —> onBeforeMount
- mounted —> onMounted
- beforeUpdate —> onBeforeUpdate
- updated —> onUpdated
- beforeUnmount —> onBeforeUnmount
- unmounted —> onUnmounted
- activated —> onActivated
- deactivated —> onDeactivated
- errorCaptured —> onErrorCaptured
- renderTracked —> onRenderTracked
- renderTriggered —> onRenderTriggered

详细内容推荐阅读：
 [组合式 API：生命周期钩子--官方文档](https://link.juejin.cn?target=https%3A%2F%2Fcn.vuejs.org%2Fapi%2Fcomposition-api-lifecycle.html)
 [选项式 API：生命周期选项--官方文档](https://link.juejin.cn?target=https%3A%2F%2Fcn.vuejs.org%2Fapi%2Foptions-lifecycle.html)



### Vue.$nextTick

----

**在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。**

`nextTick` 是 Vue 提供的一个全局 API，由于 Vue 的异步更新策略，导致我们对数据修改后不会直接体现在 DOM 上，此时如果想要立即获取更新后的 DOM 状态，就需要借助该方法。

Vue 在更新 DOM 时是异步执行的。当数据发生变化，Vue 将开启一个异步更新队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个 `watcher` 被多次触发，只会被推入队列一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作是非常重要的。`nextTick`方法会在队列中加入一个回调函数，确保该函数在前面的 DOM 操作完成后才调用。

使用场景：

1. 如果想要在修改数据后立刻得到更新后的`DOM`结构，可以使用`Vue.nextTick()`
2. 在`created`生命周期中进行`DOM`操作



### Vue 实例挂载过程中发生了什么？

----

挂载过程指的是 `app.mount()`过程，这是一个初始化过程，整体上做了两件事情：`初始化`和`建立更新机制`。

初始化会创建组件实例、初始化组件状态、创建各种响应式数据。

建立更新机制这一步会立即执行一次组件的更新函数，这会首次执行组件渲染函数并执行`patch`将`vnode` 转换为 `dom`； 同时首次执行渲染函数会创建它内部响应式数据和组件更新函数之间的依赖关系，这使得以后数据发生变化时会执行对应的更新函数。



### Vue 的模版编译原理

----

Vue 中有个独特的编译器模块，称为`compiler`，它的主要作用是将用户编写的`template`编译为js中可执行的`render`函数。
 在Vue 中，编译器会先对`template`进行解析，这一步称为`parse`，结束之后得到一个JS对象，称之为`抽象语法树AST`；然后是对`AST`进行深加工的转换过程，这一步称为`transform`，最后将前面得到的`AST`生成JS代码，也就是`render`函数。



### Vue 的响应式原理

----

1. Vue 2 中的数据响应式会根据数据类型做不同的处理。如果是对象，则通过

   ```
   Object.defineProperty(obj,key,descriptor)
   ```

   拦截对象属性访问，当数据被访问或改变时，感知并作出反应；如果是数组，则通过覆盖数组原型的方法，扩展它的7个变更方法（push、pop、shift、unshift、splice、sort、reverse），使这些方法可以额外的做更新通知，从而做出响应。

   缺点：

   - 初始化时的递归遍历会造成性能损失；
   - 通知更新过程需要维护大量 `dep` 实例和 `watcher` 实例，额外占用内存较多；
   - 新增或删除对象属性无法拦截，需要通过 `Vue.$set` 及 `delete` 这样的 API 才能生效；
   - 对于`ES6`中新产生的`Map`、`Set`这些数据结构不支持。

2. Vue 3 中利用`ES6`的`Proxy`机制代理需要响应化的数据。可以同时支持对象和数组，动态属性增、删都可以拦截，新增数据结构均支持，对象嵌套属性运行时递归，用到时才代理，也不需要维护特别多的依赖关系，性能取得很大进步。

   Proxy与reflect搭配使用的原因，是reflect可以改变this的指向，需要将其指向改为receiver，我们访问的其实是receiver[key]的值，直接访问receiver[key]会造成无限循环递归，超出堆栈报错

```javascript
// 基础原理，类似吧之前的defineProperty方法换成了proxy
function reactive(target) {
		const handler ={
			get: function(target, key, receiver) {
				console.log(`访问了${key}属性`);
				return Reflect.get(target, key, receiver);
			},
			set: function(target, key, value, receiver) {
				console.log(`将${key}由->${target[key]}->设置为->${value}`);
				Reflect.set(target, key, value, receiver);
			}
		}
		return new Proxy(target, handler);
	}
```

详细阅读:https://juejin.cn/post/7001999813344493581#heading-9

### 虚拟DOM

----

1. 概念：
    虚拟DOM，顾名思义就是虚拟的DOM对象，它本身就是一个JS对象，只不过是通过不同的属性去描述一个视图结构。
    
2. 虚拟DOM的好处：
    (1) 性能提升
    直接操作DOM是有限制的，一个真实元素上有很多属性，如果直接对其进行操作，同时会对很多额外的属性内容进行了操作，这是没有必要的。如果将这些操作转移到JS对象上，就会简单很多。另外，操作DOM的代价是比较昂贵的，频繁的操作DOM容易引起页面的重绘和回流。如果通过抽象VNode进行中间处理，可以有效减少直接操作DOM次数，从而减少页面的重绘和回流。
    (2) 方便跨平台实现
    同一VNode节点可以渲染成不同平台上对应的内容，比如：渲染在浏览器是DOM元素节点，渲染在Native（iOS、Android）变为对应的控件。Vue 3 中允许开发者基于VNode实现自定义渲染器（renderer），以便于针对不同平台进行渲染。
    
3. 结构：
    没有统一的标准，一般包括`tag`、`props`、`children`三项。
    `tag`：必选。就是标签，也可以是组件，或者函数。
    `props`：非必选。就是这个标签上的属性和方法。
    `children`：非必选。就是这个标签的内容或者子节点。如果是文本节点就是字符串；如果有子节点就是数组。换句话说，如果判断`children`是字符串的话，就表示一定是文本节点，这个节点肯定没有子元素。
    
    ```html
    <ul id="list">
        <li class="item">哈哈</li>
        <li class="item">呵呵</li>
        <li class="item">嘿嘿</li>
    </ul>
    <script>
        let oldVDOM = { // 旧虚拟DOM
            tagName: 'ul', // 标签名
            props: { // 标签属性
                id: 'list'
            },
            children: [ // 标签子节点
                {
                    tagName: 'li', props: { class: 'item' }, children: ['哈哈']
                },
                {
                    tagName: 'li', props: { class: 'item' }, children: ['呵呵']
                },
                {
                    tagName: 'li', props: { class: 'item' }, children: ['嘿嘿']
                },
            ]
        }
    </script>
    ```
    
    



### diff 算法

----

1. 概念：
    `diff`算法是一种对比算法，通过对比旧的虚拟DOM和新的虚拟DOM，得出是哪个虚拟节点发生了改变，找出这个虚拟节点并只更新这个虚拟节点所对应的真实节点，而不用更新其他未发生改变的节点，实现精准地更新真实DOM，进而提高效率。
2. 对比方式：
    `diff`算法的整体策略是：`深度优先，同层比较`。比较只会在同层级进行, 不会跨层级比较；比较的过程中，循环从两边向中间收拢。

- 首先判断两个节点的`tag`是否相同，不同则删除该节点重新创建节点进行替换。

- tag相同时，先替换属性，然后对比子元素，分为以下几种情况：
  - 新旧节点都有子元素时，采用双指针方式进行对比。新旧头尾指针进行比较，循环向中间靠拢，根据情况调用`patchVnode`进行`patch`重复流程、调用`createElem`创建一个新节点，从哈希表寻找 `key`一致的`VNode`节点再分情况操作。
  - 新节点有子元素，旧节点没有子元素，则将子元素虚拟节点转化成真实节点插入即可。
- 新节点没有子元素，旧节点有子元素，则清空子元素，并设置为新节点的文本内容。
  - 新旧节点都没有子元素时，即都为文本节点，则直接对比文本内容，不同则更新。

详细阅读：https://zhuanlan.zhihu.com/p/369557715

https://juejin.cn/post/6994959998283907102



### Vue中key的作用？

----

`key`的作用主要是`为了更加高效的更新虚拟 DOM`。

Vue 判断两个节点是否相同时，主要是判断两者的`key`和`元素类型tag`。因此，如果不设置`key` ，它的值就是 undefined，则可能永远认为这是两个相同的节点，只能去做更新操作，将造成大量的 DOM 更新操作。



### 为什么组件中的 data 是一个函数？

----

在 new Vue() 中，可以是函数也可以是对象，因为根实例只有一个，不会产生数据污染。

在组件中，data 必须为函数，目的是为了防止多个组件实例对象之间共用一个 data，产生数据污染；而采用函数的形式，initData 时会将其作为工厂函数都会返回全新的 data 对象。



### Vue 中组件间的通信方式？

----

1. 父子组件通信：

   父向子传递数据是通过`props`，子向父是通过`$emit`触发事件；通过父链/子链也可以通信（`$parent`/`$children`）；`ref`也可以访问组件实例；`provide`/`inject`；`$attrs`/`$listeners`。

2. 兄弟组件通信：

   全局事件总线`EventBus`、`Vuex`。

3. 跨层级组件通信：

   全局事件总线`EventBus`、`Vuex`、`provide`/`inject`。



### v-show 和 v-if 的区别？

----

1. 控制手段不同。`v-show`是通过给元素添加 css 属性`display: none`，但元素仍然存在；而`v-if`控制元素显示或隐藏是将元素整个添加或删除。
2. 编译过程不同。`v-if`切换有一个局部编译/卸载的过程，切换过程中合适的销毁和重建内部的事件监听和子组件；`v-show`只是简单的基于 css 切换。
3. 编译条件不同。`v-if`是真正的条件渲染，它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建，渲染条件为假时，并不做操作，直到为真才渲染。
4. 触发生命周期不同。`v-show`由 false 变为 true 的时候不会触发组件的生命周期；`v-if`由 false 变为 true 的时候，触发组件的`beforeCreate`、`created`、`beforeMount`、`mounted`钩子，由 true 变为 false 的时候触发组件的`beforeDestory`、`destoryed`钩子。
5. 性能消耗不同。`v-if`有更高的切换消耗；`v-show`有更高的初始渲染消耗。

使用场景：
 如果需要非常频繁地切换，则使用`v-show`较好，如：手风琴菜单，tab 页签等； 如果在运行时条件很少改变，则使用`v-if`较好，如：用户登录之后，根据权限不同来显示不同的内容。



### computed 和 watch 的区别？

----

- `computed`计算属性，依赖其它属性计算值，内部任一依赖项的变化都会重新执行该函数，计算属性有缓存，多次重复使用计算属性时会从缓存中获取返回值，计算属性必须要有`return`关键词。
- `watch`侦听到某一数据的变化从而触发函数。当数据为对象类型时，对象中的属性值变化时需要使用深度侦听`deep`属性，也可在页面第一次加载时使用立即侦听`immdiate`属性。

运用场景：
 计算属性一般用在模板渲染中，某个值是依赖其它响应对象甚至是计算属性而来；而侦听属性适用于观测某个值的变化去完成一段复杂的业务逻辑。



### v-if 和 v-for 为什么不建议放在一起使用？

----

Vue 2 中，`v-for`的优先级比`v-if`高，这意味着`v-if`将分别重复运行于每一个`v-for`循环中。如果要遍历的数组很大，而真正要展示的数据很少时，将造成很大的性能浪费。

Vue 3 中，则完全相反，`v-if`的优先级高于`v-for`，所以`v-if`执行时，它调用的变量还不存在，会导致异常。

通常有两种情况导致要这样做：

- 为了过滤列表中的项目，比如：`v-for = "user in users" v-if = "user.active"`。这种情况，可以定义一个计算属性，让其返回过滤后的列表即可。
- 为了避免渲染本该被隐藏的列表，比如`v-for = "user in users"  v-if = "showUsersFlag"`。这种情况，可以将`v-if`移至容器元素上或在外面包一层`template`即可。



### $set

----

可手动添加响应式数据，解决数据变化视图未更新问题。当在项目中直接设置数组的某一项的值，或者直接设置对象的某个属性值，会发现页面并没有更新。这是因为`Object.defineProperty()`的限制，监听不到数据变化，可通过`this.$set(数组或对象，数组下标或对象的属性名，更新后的值)`解决。



### keep-alive 是什么？

----

- 作用：实现组件缓存，保持组件的状态，避免反复渲染导致的性能问题。
- 工作原理：Vue.js 内部将 DOM 节点，抽象成了一个个的 VNode 节点，`keep-alive`组件的缓存也是基于 VNode 节点的。它将满足条件的组件在 cache 对象中缓存起来，重新渲染的时候再将 VNode 节点从 cache 对象中取出并渲染。
- 可以设置以下属性：
   ① `include`：字符串或正则，只有名称匹配的组件会被缓存。
   ② `exclude`：字符串或正则，任何名称匹配的组件都不会被缓存。
   ③ `max`：数字，最多可以缓存多少组件实例。
   匹配首先检查组件的`name`选项，如果`name`选项不可用，则匹配它的局部注册名称（父组件 components选项的键值），匿名组件不能被匹配。

设置了`keep-alive`缓存的组件，会多出两个生命周期钩子：`activated`、`deactivated`。
 首次进入组件时：beforeCreate --> created --> beforeMount --> mounted --> activated --> beforeUpdate --> updated --> deactivated
 再次进入组件时：activated --> beforeUpdate --> updated --> deactivated



### mixin

----

`mixin`（混入）， 它提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。

使用场景： 不同组件中经常会用到一些相同或相似的代码，这些代码的功能相对独立。可以通过mixin 将相同或相似的代码提出来。

劣势：

1. 变量来源不明确
2. 多 mixin 可能会造成命名冲突（解决方式：Vue 3的组合API）
3. mixin 和组件坑出现多对的的关系，使项目复杂度变高。



### 插槽

----

`slot`插槽，一般在组件内部使用，封装组件时，在组件内部不确定该位置是以何种形式的元素展示时，可以通过`slot`占据这个位置，该位置的元素需要父组件以内容形式传递过来。`slot`分为：

- `默认插槽`：子组件用`<slot>`标签来确定渲染的位置，标签里面可以放`DOM`结构作为后备内容，当父组件在使用的时候，可以直接在子组件的标签内写入内容，该部分内容将插入子组件的`<slot>`标签位置。如果父组件使用的时候没有往插槽传入内容，后备内容就会显示在页面。
- `具名插槽`：子组件用`name`属性来表示插槽的名字，没有指定`name`的插槽，会有隐含的名称叫做 `default`。父组件中在使用时在默认插槽的基础上通过`v-slot`指令指定元素需要放在哪个插槽中，`v-slot`值为子组件插槽`name`属性值。使用`v-slot`指令指定元素放在哪个插槽中，必须配合`<template>`元素，且一个`<template>`元素只能对应一个预留的插槽，即不能多个`<template>` 元素都使用`v-slot`指令指定相同的插槽。`v-slot`的简写是`#`，例如`v-slot:header`可以简写为`#header`。
- 作用域插槽：子组件在<slot>标签上绑定props数据，以将子组件数据传给父组件使用。父组件获取插槽绑定 props 数据的方法：
  1. scope="接收的变量名"：`<template scope="接收的变量名">`
  2. slot-scope="接收的变量名"：`<template slot-scope="接收的变量名">`
  3. v-slot:插槽名="接收的变量名"：`<template v-slot:插槽名="接收的变量名">`



### Vue 中的修饰符有哪些？

----

在Vue 中，修饰符处理了许多 DOM 事件的细节，让我们不再需要花大量的时间去处理这些烦恼的事情，而能有更多的精力专注于程序的逻辑处理。Vue中修饰符分为以下几种：

1. 表单修饰符
    `lazy` 填完信息，光标离开标签的时候，才会将值赋予给value，也就是在`change`事件之后再进行信息同步。
    `number` 自动将用户输入值转化为数值类型，但如果这个值无法被`parseFloat`解析，则会返回原来的值。
    `trim` 自动过滤用户输入的首尾空格，而中间的空格不会被过滤。

2. 事件修饰符
    `stop` 阻止了事件冒泡，相当于调用了`event.stopPropagation`方法。

   `prevent` 阻止了事件的默认行为，相当于调用了`event.preventDefault`方法。

   `self` 只当在 `event.target` 是当前元素自身时触发处理函数。

   `once` 绑定了事件以后只能触发一次，第二次就不会触发。

   `capture` 使用事件捕获模式，即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理。

   `passive` 告诉浏览器你不想阻止事件的默认行为。

   `native` 让组件变成像`html`内置标签那样监听根元素的原生事件，否则组件上使用 `v-on` 只会监听自定义事件。

3. 鼠标按键修饰符

   `left` 左键点击。

   `right` 右键点击。

   `middle` 中键点击。

4. 键值修饰符

   键盘修饰符是用来修饰键盘事件（onkeyup，onkeydown）的，有如下：keyCode存在很多，但vue为我们提供了别名，分为以下两种：

   - 普通键（enter、tab、delete、space、esc、up...）
   - 系统修饰键（ctrl、alt、meta、shift...）



### 对 SPA 的理解？

----

1. 概念：
    `SPA（Single-page  application）`，即单页面应用，它是一种网络应用程序或网站的模型，通过动态重写当前页面来与用户交互，这种方法避免了页面之间切换时打断用户体验。在`SPA`中，所有必要的代码（HTML、JavaScript 和 CSS）都通过单个页面的加载而检索，或者根据需要（通常是响应用户操作）动态装载适当的资源并添加到页面。页面在任何时间点都不会重新加载，也不会将控制转移到其他页面。举个例子，就像一个杯子，上午装的是牛奶，中午装的是咖啡，下午装的是茶，变得始终是内容，杯子始终不变。

2. `SPA`与`MPA`的区别：
    `MPA（Muti-page application）`，即多页面应用。在`MPA`中，每个页面都是一个主页面，都是独立的，每当访问一个页面时，都需要重新加载 Html、CSS、JS 文件，公共文件则根据需求按需加载。

   |                 | SPA                       | MPA                                 |
   | --------------- | ------------------------- | ----------------------------------- |
   | 组成            | 一个主页面和多个页面片段  | 多个主页面                          |
   | url模式         | hash模式                  | history模式                         |
   | SEO搜索引擎优化 | 难实现，可使用SSR方式改善 | 容易实现                            |
   | 数据传递        | 容易                      | 通过url、cookie、localStorage等传递 |
   | 页面切换        | 速度快，用户体验良好      | 切换加载资源，速度慢，用户体验差    |
   | 维护成本        | 相对容易                  | 相对复杂                            |

3. `SPA`的优缺点：
    优点：

   - 具有桌面应用的即时性、网站的可移植性和可访问性
   - 用户体验好、快，内容的改变不需要重新加载整个页面
   - 良好的前后端分离，分工更明确

   缺点：

   - 不利于搜索引擎的抓取
   - 首次渲染速度相对较慢



### 双向绑定？

----

1. 概念：
    Vue 中双向绑定是一个指令`v-model`，可以绑定一个响应式数据到视图，同时视图的变化能改变该值。`v-model`是语法糖，默认情况下相当于`:value`和`@input`，使用`v-model`可以减少大量繁琐的事件处理代码，提高开发效率。
2. 使用：
    通常在表单项上使用`v-model`，还可以在自定义组件上使用，表示某个值的输入和输出控制。
3. 原理：
    `v-model`是一个指令，双向绑定实际上是Vue 的编译器完成的，通过输出包含`v-model`模版的组件渲染函数，实际上还是`value`属性的绑定及`input`事件监听，事件回调函数中会做相应变量的更新操作。

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
</head>
<body>
	<div id="app">
		<input v-model="username" type="text" placeholder="请输入用户名" autocomplete="off">
		<span v-bind="username"></span><br>
		<input v-model="password" type="text" placeholder="请输入密码" autocomplete="off">
		<span v-bind="password"></span>
	</div>
</body>
<script>
	// 声明构造函数Vue
	var Vue = function (options) {
		// 创建实例时传递M层的数据，数据层发生了set操作，就自动更新页面
		this.$data = options.data;

		// 需要一个指令集,因为可能存在多个指令.这里所说的指令其实就是元素的属性
		this.$bindings = {};

		// 根节点 dom元素
		this.$el = document.getElementById(options.el);

		this.init(); // 直接调用->初始化阶段 M->VM->V
		this.compile(); // 直接调用->编译阶段
	}

	// 将Vue的内部方法完善，使用原型重写
	Vue.prototype.init = function () {
		console.log("获取Mode层的数据");
		let _this = this;
		// 初始化时,劫持数据 即遍历上述的$data
		for (let key in _this.$data) {
			// 遍历数据的时候,给每一个Model属性配置指令集
			this.$bindings[key] = {
				directives: []
			}
			let value = _this.$data[key]; // _this.$data[key] 会触发get函数 因为相当于读取,就是会读取对象的get函数
			// 使用object的defineProperty,对某个对象的某个key进行劫持
			Object.defineProperty(_this.$data, key, {
				get: function() {
					return value;
				},
				set: function(newVal) {
					// set时,遍历指令集,更新DOM
					value = newVal;
					// 更新DOM
					_this.$bindings[key].directives.forEach(watcher => {
						watcher.update();
					})
				}
			})
		}
	}

	Vue.prototype.compile = function () {
		// 编译阶段,根据传过来的el(根节点元素),遍历每个子节点,挨个检查元素是否有对应的指令,有就进行绑定
		console.log("监听DOM元素");
		// console.log(this);
		let _this = this; // 监听input事件回调,丢失了vue实例的this指向,所以要在外层将指向vue实例的this保存一下
		let nodes = this.$el.children;
		// console.log(nodes);
		for (let i = 0; i < nodes.length; i++) {
			let node = nodes[i];

			if (node.hasAttribute('v-model')) {
				let dataKey = node.getAttribute('v-model');
				// console.log(dataKey); // username
				// 如果是v-model指令,就监听DOM的input事件
				node.addEventListener('input', function() {
					// 更新Model
					// console.log(this); 
					// console.log(_this);
					_this.$data[dataKey] = node.value; // 赋值
				})
				// 添加Watcher
				_this.$bindings[dataKey].directives.push(new Watcher(
					node,
					'value',
					_this,
					dataKey
				))
			} else if (node.hasAttribute('v-bind')) {
				let dataKey = node.getAttribute('v-bind');
				console.log(dataKey);
				_this.$bindings[dataKey].directives.push(new Watcher(
					node,
					'innerHTML',
					_this,
					dataKey
				))
			}
		}
	}

	// 侦听
	var Watcher = function (dom, expression, vm, dataKey) {
		this.dom = dom;
		this.expression = expression;
		this.vm = vm;
		this.dataKey = dataKey;
		this.update();
	}

	Watcher.prototype.update = function () {
		console.log("窃听到了");
		this.dom[this.expression] = this.vm.$data[this.dataKey];
	}

	// 使用构造函数创建实例
	var vm = new Vue({
		// 需要被vue控制的根元素ID，即我们的app
		el: "app",

		// Model数据层
		data: {
			username: 'jack',
			password: '123456'
		}
	})
</script>
</html>
```



### 子组件是否可以直接改变父组件的数据？

----

1. 所有的`prop`都遵循着单项绑定原则，`props`因父组件的更新而变化，自然地将新状态向下流往子组件，而不会逆向传递。这避免了子组件意外修改父组件的状态的情况，不然应用的数据流将很容易变得混乱而难以理解。
    另外，每次父组件更新后，所有的子组件中的`props`都会被更新为最新值，这就意味着不应该子组件中去修改一个`prop`，若这么做了，Vue 会在控制台上抛出警告。
2. 实际开发过程中通常有两个场景导致要修改prop：
   - `prop`被用于传入初始值，而子组件想在之后将其作为一个局部数据属性。这种情况下，最好是新定义一个局部数据属性，从`props`获取初始值即可。
   - 需要对传入的`prop`值做进一步转换。最好是基于该`prop`值定义一个计算属性。
3. 实践中，如果确实要更改父组件属性，应`emit`一个事件让父组件变更。当对象或数组作为`props`被传入时，虽然子组件无法更改`props`绑定，但仍然**可以**更改对象或数组内部的值。这是因为JS的对象和数组是按引用传递，而对于 Vue 来说，禁止这样的改动虽然可能，但是有很大的性能损耗，比较得不偿失。



### Vue Router中的常用路由模式和原理？

----

1. hash 模式：

- `location.hash`的值就是url中 # 后面的东西。它的特点在于：hash虽然出现url中，但不会被包含在HTTP请求中，对后端完全没有影响，因此改变hash不会重新加载页面。
- 可以为hash的改变添加监听事件`window.addEventListener("hashchange", funcRef, false)`，每一次改变`hash (window.location.hash)`，都会在浏览器的访问历史中增加一个记录，利用hash的以上特点，就可以实现**前端路由更新视图但不重新请求页面**的功能了。
   特点：兼容性好但是不美观

1. history 模式：

利用 HTML5 History Interface 中新增的`pushState()`和`replaceState()`方法。
 这两个方法应用于浏览器的历史记录栈，在当前已有的`back`、`forward`、`go` 的基础上，他们提供了对历史记录进行修改的功能。
 这两个方法有个共同点：当调用他们修改浏览器历史记录栈后，虽然当前url改变了，但浏览器不会刷新页面，这就为单页面应用前端路由“更新视图但不重新请求页面”提供了基础
 特点：虽然美观，但是刷新会出现 404 需要后端进行配置。



### 动态路由？

----

很多时候，我们需要将给定匹配模式的路由映射到同一个组件，这种情况就需要定义动态路由。例如，我们有一个 User组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。那么，我们可以在 vue-router 的路由路径中使用`动态路径参数（dynamic segment）`来达到这个效果：`{path: '/user/:id', compenent: User}`，其中`:id`就是动态路径参数。



### 对Vuex的理解？

----

1. 概念：
    Vuex 是 Vue 专用的状态管理库，它以全局方式集中管理应用的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
2. 解决的问题：
    Vuex 主要解决的问题是多组件之间状态共享。利用各种通信方式，虽然也能够实现状态共享，但是往往需要在多个组件之间保持状态的一致性，这种模式很容易出问题，也会使程序逻辑变得复杂。Vuex 通过把组件的共享状态抽取出来，以全局单例模式管理，这样任何组件都能用一致的方式获取和修改状态，响应式的数据也能够保证简洁的单向流动，使代码变得更具结构化且易于维护。
3. 什么时候用:
    Vuex 并非是必须的，它能够管理状态，但同时也带来更多的概念和框架。如果我们不打算开发大型单页应用或应用里没有大量全局的状态需要维护，完全没有使用Vuex的必要，一个简单的 store 模式就够了。反之，Vuex将是自然而然的选择。
4. 用法：
    Vuex 将全局状态放入`state`对象中，它本身是一颗状态树，组件中使用`store`实例的`state`访问这些状态；然后用配套的`mutation`方法修改这些状态，并且只能用`mutation`修改状态，在组件中调用`commit`方法提交`mutation`；如果应用中有异步操作或复杂逻辑组合，需要编写`action`，执行结束如果有状态修改仍需提交`mutation`，组件中通过`dispatch`派发`action`。最后是模块化，通过`modules`选项组织拆分出去的各个子模块，在访问状态（state）时需注意添加子模块的名称，如果子模块有设置`namespace`，那么提交`mutation`和派发`action`时还需要额外的命名空间前缀。



### 页面刷新后Vuex 状态丢失怎么解决？

----

Vuex 只是在内存中保存状态，刷新后就会丢失，如果要持久化就需要保存起来。

`localStorage`就很合适，提交`mutation`的时候同时存入`localStorage`，在`store`中把值取出来作为`state`的初始值即可。

也可以使用第三方插件，推荐使用`vuex-persist`插件，它是为 Vuex 持久化储存而生的一个插件，不需要你手动存取`storage`，而是直接将状态保存至 `cookie` 或者 `localStorage`中。



### 关于 Vue SSR 的理解？

----

`SSR`即`服务端渲染（Server Side Render）`，就是将 Vue 在客户端把标签渲染成 html 的工作放在服务端完成，然后再把 html 直接返回给客户端。

- 优点：
   有着更好的 SEO，并且首屏加载速度更快。
- 缺点：
   开发条件会受限制，服务器端渲染只支持 beforeCreate 和 created 两个钩子，当我们需要一些外部扩展库时需要特殊处理，服务端渲染应用程序也需要处于 Node.js 的运行环境。服务器会有更大的负载需求。



### 了解哪些 Vue 的性能优化方法？

----

- 路由懒加载。有效拆分应用大小，访问时才异步加载。
- `keep-alive`缓存页面。避免重复创建组件实例，且能保留缓存组件状态。
- `v-for`遍历避免同时使用`v-if`。实际上在 Vue 3 中已经是一个错误用法了。
- 长列表性能优化，可采用虚拟列表。
- `v-once`。不再变化的数据使用`v-once`。
- 事件销毁。组件销毁后把全局变量和定时器销毁。
- 图片懒加载：vue-lazyload插件。
- 第三方插件按需引入。
- 子组件分割。较重的状态组件适合拆分。
- 服务端渲染。



#### vue.use的作用

----

`Vue.use({install(){}},1 ,2)`

- 参数1：可以传一个函数或者对象，如果是对象，对象中需要一个install函数，如果是一个函数，相当于install函数。
- 其他参数：依次传给了install函数。

install函数默认第一个参数是Vue，后面的参数是来自于Vue.use后续参数。

Vue.use本质是无法注册全局组件或者给Vue原型添加方法，但是我们在使用路由或者vuex或者element ui,实际还是在install函数内部通过Vue.component注册了全局组件或者个Vue.prototype手动添加方法。



#### 路由守卫/路由钩子

----

1.全局守卫

对所有路由全部都生效，进行权限的处理，比如是否登录的路由权限控制

- 路由前置守卫：beforeEach
- 路由后置守卫：afterEach

2.路由独享守卫

针对某一条路由进行单向控制：beforeEnter

3.组件内守卫（针对组件）

beforeRouteEnter：只要通过路由，不管哪一条路由显示该组件就会触发，控制改页面是否展示

beforeRouterUpdate：路由组件被复用时，例如/a/1，进入a页面，在当前组件重新进入/a/2，显示a页面，a页面就会被复用，组件没有销毁，组件不会重新走，created不会执行，此时可以使用beforeRouterUpdate解决

beforeRouteLeave：表单页面的返回，提示用户当前表单未提交是否离开，离开当前组件