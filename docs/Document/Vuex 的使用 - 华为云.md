---
title: Vuex的使用(介绍)
tags: vue
notebook: wiki
---

- [我们可以用公司举个例子：公司有个仓库](#我们可以用公司举个例子公司有个仓库)
  - [Vuex使用场景](#vuex使用场景)
  - [Vuex 核心内容](#vuex-核心内容)
- [创建一个小小的Demo](#创建一个小小的demo)
- [`mapState`辅助函数](#mapstate辅助函数)
- [`mapMutations`函数传参数](#mapmutations函数传参数)
- [`Getters`计算属性过滤](#getters计算属性过滤)
  - [`mapGetters` 辅助函数](#mapgetters-辅助函数)
- [`action`异步修改状态](#action异步修改状态)
- [`Module`模块组](#module模块组)
> Vuex是一个针对Vue.js开发的状态管理模式。说简单点儿就是一个工具，可以管理（修改或设置）所有组件用到的数据，而不需要借助之前的event bus或者props在组件间传值。

## 我们可以用公司举个例子：公司有个仓库

1.State（公司的仓库）  
2.Getter（只能取出物品，包装一下，不能改变物品任何属性）  
3.Muitation（仓库管理员，只有他可以直接存储到仓库）  
4.Action（公司的物料采购员，负责从外面买东西和接货， 要往仓库存东西，告诉仓库管理员要存什么）  
非常要注意的地方：**只要刷新或者退出浏览器，仓库清空。**

### Vuex使用场景

大型单页应用程序，存在多组件共享数据的时候，需要用到

### Vuex 核心内容

![Vuex 的使用1](https://res-static.hc-cdn.cn/fms/img/47d3416697059ed8dd3e8a80f92e84dc1603766127689.png)

![Vuex 的使用2](https://res-static.hc-cdn.cn/fms/img/1fa822f7ca8e55692087795ba07498d11603766127689.png)

## 创建一个小小的Demo

```
# 安装 vuex 
npm i vuex --save
```

![Vuex 的使用3](https://res-static.hc-cdn.cn/fms/img/2b6b4c323dd0d702ac6e435e29e9e12e1603766127690.png)

在`src`里面创建vuex/store.js

> store (一个容器对象，存储Vuex中的state,mutations,actions,getters等)
> 
> state (一个保存数据的对象，对象中的数据可以供所有组件使用)

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex);

const state = {
  count: 3
};

const mutations = {
  add(state) { state.count++;
  },
  reduce(state) { state.count--;
  }
};

export default new Vuex.Store({
  state,
  mutations
})
```

在`src`里面创建components/Count.vue

```js
<script>
  import store from '@/vuex/store' export default { name: "Count", data() { return { msg: 'hello Vuex' } }, store
  }
</script>

<style>

</style>
```

修改路由

```js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import Count from '@/components/Count'

Vue.use(Router);

export default new Router({
  routes: [ { path: '/hello', name: 'HelloWorld', component: HelloWorld }, { path: '/', name: 'Count', component: Count }
  ]
})
```

## `mapState`辅助函数

使用count

```
{{count}}
```

两种方法:

1.  需要一个计算属性
    
    > 那么我们如何在 Vue 组件中展示状态呢？由于 Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在[计算属性](https://cn.vuejs.org/guide/computed.html)中返回某个状态
    
    > 每当 `store.state.count` 变化的时候，都会重新求取计算属性，并且触发更新相关联的 DOM。
    

```js
 computed: { count() { return this.$store.state.count; } }
```

1.  引入mapState

```js
import {mapState} from 'vuex'
```

计算属性

```js
  computed: mapState({ count: state => state.count}),
```

或者这样

```js
 computed: mapState(['count']),
```

都行

## `mapMutations`函数传参数

> mutations(一个对象，保存的是更改state的函数，也只有它能更改state中的值，触发方式this.$store.commit('函数名',参数))

store.js 记得导出

```js
const mutations = {
  add(state,n) { state.count+=n;
  },
  reduce(state) { state.count--;
  }
};
```

Count.vue

```
  + - 
```

第二种引入

```
 + -
```

引入`mapMutations`

```js
import {mapState, mapMutations} from 'vuex'
```

```js
methods: mapMutations(['add', 'reduce'])
```

## `Getters`计算属性过滤

> getters(一个对象，保存的是一些类似与计算属性的函数，可以通过他们得到任何依赖于state的新的数据)

store.js 记得导出

```js
const getters = {
  count: function (state) { return state.count += 100;
  }, count: state => state.count += 100
};
```

Count

```
 computed: { 
```

### `mapGetters` 辅助函数

引入

```js
  import {mapState, mapMutations,mapGetters} from 'vuex'
```

计算属性

```js
 computed: { ...mapState(['count']), 
```

## `action`异步修改状态

> actions(一个对象，保存的是触发mutations的函数，让mutations去修改state中的值)

store.js 记得导出

```js
const actions = {
  addAction(content) { content.commit('add', 10), setTimeout(() => { content.commit('reduce') }, 5000) console.log('我比reduce先执行');
  },
  reduceAction({commit}) { commit('reduce');
  }
};
```

Count

使用

```
  + - 
```

引用

```js
  import {mapState, mapMutations,mapGetters,mapActions} from 'vuex'

```

方法

```js
 methods: { ...mapMutations(['add', 'reduce']), ...mapActions(['addAction','reduceAction']) }
```

## `Module`模块组

store

```js
const moduleA={
  state,
  mutations,
  getters,
  actions
};
export default new Vuex.Store({
modules:{a:moduleA}
})
```

使用

```js
 {{$store.state.a.count}}--{{count}}
```

{{count}}不能直接使用

要用计算属性

src/vuex/store.js

```js
import Vuex from 'vuex'
import Vue from 'vue'
import axios from 'axios'
Vue.use(Vuex);

const state = {
  todos: [ {id: 1, title: 'Todo One'}, {id: 2, title: 'Todo Two'}
  ]
};

const mutations = {
  setTodos(state, todos) { state.todos = todos;
  },
  post(state,todo){ state.todos.unshift(todo);
  }
};

const getters = {
  getAllTodos(state) { return state.todos;
  }
};
const actions = {
  async getAllTodos({commit}) { const res = await axios.get('http://jsonplaceholder.typicode.com/todos'); console.log(res); commit('setTodos', res.data)
  } ,
  async addTodo({commit},todo) { const res = await axios.get('http://jsonplaceholder.typicode.com/todos',todo); console.log(res); commit('post', res.data)
  }
};


export default new Vuex.Store({state,mutations,actions,getters})

```

导入

```js
  import store from '@/vuex/store'
  import {mapGetters,mapActions} from 'vuex'
```

安装Vuex

```js
npm i vuex --save
npm i axios --save
```