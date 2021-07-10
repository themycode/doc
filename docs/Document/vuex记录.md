---
title: Vuex的使用
tags: vue
notebook: wiki
---

- [Vuex 是对复杂应用进行状态管理的](#vuex-是对复杂应用进行状态管理的)
- [state 用来保存共享数据，如](#state-用来保存共享数据如)
- [mutations 用于保存修改共享数据的方法](#mutations-用于保存修改共享数据的方法)
- [使用store中的方法,用commit来调用mutations中的两个方法](#使用store中的方法用commit来调用mutations中的两个方法)
- [computed 计算属性](#computed-计算属性)
- [vuex 几个核心概念](#vuex-几个核心概念)

### Vuex 是对复杂应用进行状态管理的

vuex 是用来保存共享数据用的
他有几大属性：
state 用于保存共享数据
mutations 用于保存修改共享数据的方法
getters 用于改变state中的数据并进行缓存，如果是改变vuex中的数据用getters，如果改变vue中的数据就用computed
actions 专门用来处理异步，实际修改状态值的，依然是mutations

### state 用来保存共享数据，如

```js
state :{
  count :0,
  msg: "test"
}
```

### mutations 用于保存修改共享数据的方法

因为方法不能全部放在外面，不然出现问题很难排查。

```js
mutations:{
  //注意点：在执行mutations中定义的方法的时候，系统会自动给这些方法传递一个state参数,state中就保存了共享的数据
  mAdd(state){
    state.count = state.count +1;
  },
  mSub(state){
    state.count = state.count -1;
  }
}
```

### 使用store中的方法,用commit来调用mutations中的两个方法

```js
components: {
  "son1":{
    template: "#son1",
    methods:{
      add(){
        this.$store.commit("mAdd");
      },
      sub(){
        this.$store.commit("mSub");
      }
    }
  }
}
```

### computed 计算属性

``` JS
<template>
<div>
  // 这样就可以调用计算属性的方法
  {{formart}}
  {{formart}}
  {{formart}}
</div>
</template>
data: function() {
  return {
    msg: "哈哈哈哈哈" // 原来的属性值
  }
},
computed:{
  formart(){
    console.console.log("计算属性的方法被执行了");
    return this.msg + "test"; // 这样就修改了原来的属性
  }
}
```

```JavaScript
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

new Vuex.Store({
 state: {     //放置state的值
     count: 0,
     str:"abcd234"
    },
   getters: {   //放置getters方法
       strLen: state => state.str.length
   },
   // mutations只能是同步操作
   mutations: {   //放置mutations方法
       increment(state, payload) {
          //在这里改变state中的数据
          state.count = payload.number;
       }
   },
   // actions可以是异步操作
   actions: {      //放置actions方法
        actionName({ commit }) {
           //dosomething
          commit('mutationName')
        },
        getSong ({commit}, id) {
           api.getMusicUrlResource(id).then(res => {
             let url = res.data.data[0].url;
           })
           .catch((error) => {  // 错误处理
               console.log(error);
          });
       }
 }
});

new Vue({
  el: '#app',
  store,
  ...
});
```

通过console.log(vuex) 可以发现以下结果：

```JavaScript
{

​ Store: function Store(){},

​ mapActions: function(){}, // 对应Actions的结果集

​ mapGetters: function(){}, // 对应Getters的结果集

​ mapMutations: function(){}, // 对应Mutations的结果集

​ mapState: function(){}, // 对应State的结果集

​ install: function install(){},

​ installed: true
}
```

import进来的Vuex它实际上是一个对象，里面包含了Store这一构造函数，还有几个mapActions、mapGetters、mapMutations、mapState这几个辅助方法（后面再讲）。

除此之外，还有一个install方法。我们发现，import之后要对其进行Vue.use(Vuex);的操作。根据这两个线索，我们就明白了，Vuex本质上就是一个Vue.js的插件。不信你去对照下Vue.js的官方文档中对于插件的解释就知道

### vuex 几个核心概念

state
可以通过this.$store.state来直接获取状态，也可以利用vuex提供的mapState辅助函数将state映射到计算属性（computed）中去。

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>vueX 学习</title>
  <!-- 先引入vue -->
  <script src="js/vue.js"></script>
  <!--1. 在引入vuex -->
  <script src="js/vuex.js"></script>
</head>
<!-- 
Vuex 解决的问题：
1、如果想要在子组件中使用祖先组件中的数据，那么必须一层层的传递，非常麻烦
2、兄弟组件之间不能直接传递数据，如果兄弟组件之间要传递数据，那么就必须借助于父组件
解决方案：Vuex

什么是Vuex
vuex是vue配套的公共数据管理工具，我们可以将恭喜那个的数据保存在vuex中
方便整个程序中的任何组件都可以获取和修改vuex中保存的公共数据

注意点：
只有需要共享的数据才放到vuex上，不需要共享的数据依然放到组件自身的data上
-->

<body>
  <div id="app">
    <father></father>
  </div>
  <template id="father">
    <!-- 如果数据保存在vuex中，也就是state，需要使用this.$store.state.xxx 来取值，前面的都是固定的方法。 -->
    <div>
      <!-- 调用computed中属性变动的方法 -->
      {{formater}}
      {{formater}}
      {{formater}}
      <p>
      父组件的调用方法`{{msg}}`:{{msg}}
      vuex中的调用方法：{{this.$store.state.msg}}
      vuex中的调用方法：{{this.$store.state.msg}}
    </p>
    <p>
      <!-- getters中的数据会被调用一次，其他的数据是缓存取出来 -->
      {{this.$store.getters.formater}}
      {{this.$store.getters.formater}}
      {{this.$store.getters.formater}}
    </p>
      <p>父组件中的数据是共享的：{{this.$store.state.count}}</p>
      <button @click="add">增加</button>
      <button @click="sub">减少</button>
      <!-- vue中加冒号的属性为动态的，不加冒号为常量 -->
      <input type="text" :value="this.$store.state.count">
      <son></son>
    </div>
  </template>
  <template id="son">
    <div>
      <p>儿子组件</p>
      <button @click="add">增加</button>
      <button @click="sub">减少</button>
      <input type="text" :value="this.$store.state.count">
    </div>
  </template>
  <script>

    //2.  创建Vuex对象
    const store = new Vuex.Store({
      // 这里的state相当于组件中的data，就是专门用于保存共享数据的
      state: {
        count: 0,
        msg: "hello world"
      },
      // 用于保存修改数据的方法
      mutations: {
        // 在执行mutations中，系统会自动给这些方法传递一个state参数，state中就保存了共享的数据
        mAdd(state) {
          state.count = state.count + 1;
        },
        mSub(state) {
          state.count = state.count - 1;
        }
      },
      // 用于改变state中的数据并缓存
      getters:{
        formater(state){
          console.log("getters 方法被执行了");
          return state.msg + ":getters改变了msg数据"
        }
      }
    });

    // 初始化爷爷组件
    Vue.component("father", {
      // 父组件
      template: "#father",
      data(){
        return {
          msg: "父组件中的data参数"
        }
      },
      // 要使用store，先调取store对象
      store: store,
      methods: {
        add() {
          this.$store.commit("mAdd");
        },
        sub() {
          this.$store.commit("mSub");
        }
      },
      computed:{
        formater(){
          console.log("计算属性的方法被执行了");
          return this.msg + "===》computed计算属性改变了父组件中的data数据";
        }
      },
      
      // 儿子组件
      components: {
        "son": {
          template: "#son",
          // 调用mutations中的方法
          methods: {
            add() {
              this.$store.commit("mAdd");
            },
            sub() {
              this.$store.commit("mSub");
            }
          },

        }
      }


    });
    let vue = new Vue({
      el: "#app",
      // 这里就是MVVM中的Model，也就是vue中存储数据的地方data
      data: {

      },
      // 专门用于存储监听事件回调函数
      methods: {
        add() {
          this.$store.commit("mAdd")
        },
        sub() {
          this.$store.commit("mSub")
        }
      },
      // 专门用于定义计算属性的
      computed:{

      },
      // 专门用于定义局部组件的
      components:{

      }
    })
  </script>
</body>

</html>
```
