### Pinia的基础使用

## 1.基本概念

状态管理，存储数据的地方，差不多就是Vuex的升级版

官方解释：Pinia 是 Vue 的存储库，它允许您跨组件/页面共享状态。

## 2.为何要使用

优点：

一、2和3都支持

二、Pinia只有state、getter、action，减少编写工作量

三、Pinia的action同时支持同步和异步

四、TS支持性好

五、体积小

六、支持插件扩展功能

七、支持服务端渲染

## 3.基本使用

### 3.1 安装&挂载

npm install pinia

```javascript
// main.js

import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import { createPinia } from "pinia";

const pinia = createPinia();

createApp(App).use(pinia).mount('#app')
```

### 3.2 创建store

与Vuex类似，创建store目录。这里其实就是创建一个数据仓库（公共组件差不多，不过只存放数据）的感觉，后面我们将数据存放在这个store里，然后每个组件都可以访问和修改

在项目src目录下创建一个store，新建一个user.js，用来存放与user相关的store

```javascript
// /store/user.js

// 选项API
import { defineStore } from 'pinia'

// 第一个参数是应用程序中 store 的唯一 id
export const useUsersStore = defineStore('users', {
  // 其它配置项
})
```

defineStore函数用来创建store，该函数接受两个参数

- name：一个字符串，必传项，该store的唯一id。

- options：一个对象，store的配置项，比如配置store内的数据，修改数据的方法等等。

### 3.3 使用

在实际的组件中使用

```javascript
// /src/App.vue

// setup 的形式
<script setup>
import { useUsersStore } from "../src/store/user";
const store = useUsersStore();
console.log(store);
</script>
```

### 3.4 state

#### 3.4.1 设置state

state：是在defineStore方法的第二个参数options配置项种添加的，state用于存放我们的数据

```javascript
// user.js

export  const useUsersStore = defineStore('users', {
	state: () => {
		return {
			name: "灭尽龙",
			lifePoint: 1000000,
		}
	}
})
```

3.3节中加入了打印，可以在控制台中查看；

可以看到多了几个我们自己编写的属性。即state

我们也可以将user.js中的useUsersStore导入到其他组件中使用=>这样就实现了，多个组件同时使用了store中的数据

#### 3.4.2 修改state

调用的时候，直接赋值即可。在页面上增加一个按钮，点击改变值

```javascript
// App.vue
<template>
	<div>名称：{{ name }}</div>
	<div>血量：{{ lifePoint }}</div>
	<button @click="changeName">更改姓名</button>
    <router-view></router-view>
</template>

<script setup>
import { useUsersStore } from "../src/store/user";
import { ref } from "vue";

const store = useUsersStore();
const name = ref(store.name);
const lifePoint = ref(store.life);
const changeName = () => {
	store.name = "张三";
	console.log(store.name);
}

console.log(store);
</script>

// 上面这种逐一赋值的方法太过繁琐，可以使用解构的形式
// const name = ref(store.name);
// const lifePoint = ref(store.lifePoint);

const { name, lifePoint } = store;  // 这一条等于上面这两条
```

实际运行后，会发现，值确实改变了，但是网页上的值没有变化。（**说明此时的state不是响应式的**）原因：直接怼store的数据进行解构，会破坏数据的响应，所以提供了一个storeToRefs函数，用来进行解构

使用storeToRefs函数，将state的数据变为响应式。

```javascript
const { name, lifePoint } = storeToRefs(store); // 将解构的表达式中加入这个方法，加入以后，点击按钮改变值，页面上也改变
```

在另一个组件中同样引入我们创建的store，然后在其中一个组件中改变这个值，可以看到改变一个值，两个组件都改变了

#### 3.4.3 $reset() 重置状态

重置状态，修改了state的数据，想要将它还原。应用场景，比如用户填写了一部分表单，突然想重置为最初的状态。

使用store的$reset()方法。

```javascript
// App.vue

<button @click="reset">重置store</button>
// 重置store
const reset = () => {
  store.$reset();
};
```

#### 3.4.4 $patch() 批量修改

前面修改state时，使用的是单独取值修改；例：store.name = “xxx”；如果每次都这样一个属性一个属性的改要写很多繁琐的代码；

使用$patch()方法

```javascript
// App.vue

<button @click="patchStore">批量修改数据</button>
// 批量修改数据
const patchStore = () => {
  store.$patch({
    name: "冰咒龙",
    lifePoint: 600000
  });
};
```

该方法可以传递一个回调。单独修改state中的某一个数据

```javascript
store.$patch((state) => {
	state.name = "冰咒龙"
})
```

#### 3.4.5 $state() 替换整个state数据

使用$state()方法，可以将整个state数据进行替换

```javascript
store.$state = { name: "歼世灭尽龙", lifePoint:"70000" }
```

#### 3.4.6 $subscribe() 订阅状态

可以通过这个方法，查看store的状态及其变换，类似Vuex的subscribe方法。与常规的watch() 相比，使用这个方法的优点在于只会在patches之后触发一次

```javascript
// 以上面的patch 批量修改处为例子

store.$subscribe((mutation, state) => {
	// import { MutationType } from 'pinia'
	mutation.type // 'direct' | 'patch object' | 'patch function'
	console.log(mutation.type); // 'patch function'
	// 与 cartStore.$id 相同
	mutation.storeId 
	console.log(mutation.storeId);// 'users'
	// 仅适用于 mutation.type === 'patch object'
	mutation.payload // 补丁对象传递给 to cartStore.$patch()
	console.log(mutation.payload);
	// 每当它发生变化时，将整个状态持久化到本地存储
	localStorage.setItem('users', JSON.stringify(state))
})
```

### 3.5 getters

getters是一个对象，提供了很多的方法。它类似于Vue的计算属性，所以它会被缓存。

作用：返回一个新的结果，处理state。

#### 3.5.1 添加getter

```javascript
// 以上面的state为例，继续完善
// user.js

export  const useUsersStore = defineStore('users', {
	state: () => {
		return {
			name: "灭尽龙",
			lifePoint: 1000000,
		}
	},
    getters: {
        getAddLife: (state) => {
            return state.lifePoint + 10000;
        }
    }
})
```

添加了要给getters，在里面写了一个方法，将lifePoint加1W

#### 3.5.2 使用getter

```javascript
// 在组件引入我们创建的store后，直接调用我们添加的getAddLife方法；
// App.vue
<template>
  <p>新血量：{{ store.getAddLife }}</p>
</template>

<script setup>
import { useUsersStore } from "../src/store/user";
const store = useUsersStore();
</script>
```

#### 3.5.3 内部调用getter的方法

在getters里，有一个方法，需要调用它自己的一个方法，直接使用this，看如下代码示例中的getNameAndLife方法

```javascript
// user.js

export  const useUsersStore = defineStore('users', {
	state: () => {
		return {
			name: "灭尽龙",
			lifePoint: 1000000,
		}
	},
    getters: {
        getAddLife: (state) => {
            return state.lifePoint + 10000;
        },
        getNameAndLife: function(state) {
            return state.name + this.getAddLife;
        }
    }
})
```

这里不用箭头函数，是因为箭头函数没有this指向，可能会出现this指向的问题。

#### 3.5.4 getter传参

getters作用类似于computed 技术属性，计算属性是不能够传参的。所以这里我们可以做一个闭包，返回一个函数，在这个函数里带上我们的参数；

```javascript
// user.js

export  const useUsersStore = defineStore('users', {
	state: () => {
		return {
			name: "灭尽龙",
			lifePoint: 1000000,
		}
	},
    getters: {
        getAddLife: (state) => {
            return state.lifePoint + 10000;
        },
        getNameAndLife: function(state) {
            return state.name + this.getAddLife;
        },
        getNameAndNum: (state) => {
            return (num) => state.name + num;
        }
    }
})
```

**注意**：执行此操作时，**getter不再缓存**，它们只是被我们调用的函数。我们可以在它返回函数时进行一个缓存的操作；

### 3.6 Actions

Actions类似于Vue中的method，state与getters是基于数据层面的，并没有具体的业务逻辑代码，state，getters差不多就类似于data，computed。

当有业务逻辑代码时，我们要用到actions。它是一个对象，存储各种方法，并且这些方法可以是**同步的，**也可以是**异步的**。

#### 3.6.1 添加Actions

```javascript
// 以之前的user.js为例 添加Actions

import { defineStore } from "pinia";

// 第一个参数是应用程序种store的唯一id
export  const useUsersStore = defineStore('users', {
	state: () => {
		return {
			name: "灭尽龙",
			lifePoint: 1000000,
		}
	},
	getters: {
        getAddLife: (state) => {
            return state.lifePoint + 10000;
        },
		getNameAndLife: function(state) {
            return state.name + this.getAddLife;
        },
		getNameAndNum: (state) => {
            return (num) => state.name + num;
        },
    },
    actions: { // 添加的actions。
        savename(name) {
            this name = name;
        }
    }
})
```

#### 3.6.2 使用Actions

在App.vue中添加实例调用即可

```javascript
// App.vue
const saveName = () => {
    store.saveName("金狮子")
}
```

添加一个按钮之类的触发一下；

#### 3.6.4 异步示例

```javascript
// 在actions对象中添加
async registerUser(login, password) {
	try {
		this.userData = await api.post({ login, password })
		showTooltip(`Welcome back ${this.userData.name}!`)
	} catch (error) {
		showTooltip(error)
		// 让表单组件显示错误
		return error
	}
},
```

#### 3.6.5 $onAction()订阅Actions

可以使用$onAction()方法订阅Action及其结果。传递一个回调，会在action之前执行，使用after处理Promise并允许在action完成后执行函数。

用这种差不多的方式，onError允许在处理中抛出错误。可以在运行时跟踪错误很有用。类似于Vue文档中的这个提示

https://vuejs.org/guide/best-practices/production-deployment.html#tracking-runtime-errors

```javascript
const unsubscribe = someStore.$onAction(
  ({
    name, // action 的名字
    store, // store 实例
    args, // 调用这个 action 的参数
    after, // 在这个 action 执行完毕之后，执行这个函数
    onError, // 在这个 action 抛出异常的时候，执行这个函数
  }) => {
    // 记录开始的时间变量
    const startTime = Date.now()
    // 这将在 `store` 上的操作执行之前触发
    console.log(`Start "${name}" with params [${args.join(', ')}].`)

    // 如果 action 成功并且完全运行后，after 将触发。
    // 它将等待任何返回的 promise
    after((result) => {
      console.log(
        `Finished "${name}" after ${
          Date.now() - startTime
        }ms.\nResult: ${result}.`
      )
    })

    // 如果 action 抛出或返回 Promise.reject ，onError 将触发
    onError((error) => {
      console.warn(
        `Failed "${name}" after ${Date.now() - startTime}ms.\nError: ${error}.`
      )
    })
  }
)

// 手动移除订阅
unsubscribe()
```

在组件中，写在setup内，当组件被卸载时，它们将被自动删除，如果要在删除后保留他们，传递第二个参数为true

```javascript
// 在组件中
<script setup>
import { useUsersStore } from "../src/store/user";
const store = useUsersStore();
// 此订阅将在组件卸载后保留
someStore.$onAction(callback, true) 

// ...
</script>
```

### 3.7 访问其他store的操作

直接在那个js中将另一个store导入，直接拿来使用即可，以下以action为例子，其他的类似。

```javascript
import { useAuthStore } from './auth-store' // 另一个store

export const useSettingsStore = defineStore('settings', { // 当前的store
  state: () => ({
    // ...
  }),
  actions: {
    async fetchUserPreferences(preferences) {
      const auth = useAuthStore() // 拿到另一个store的实例
      if (auth.isAuthenticated) {
        this.preferences = await fetchPreferences()
      } else {
        throw new Error('User must be authenticated')
      }
    },
  },
})
```

插件等更多详细使用方式可见官网https://pinia.web3doc.top/core-concepts/plugins.html