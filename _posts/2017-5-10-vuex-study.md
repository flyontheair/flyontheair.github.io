---
layout: post
title:  "Vuex学习笔记"
date:   2017-05-10 17:25:18 +0800
categories: 前端
tags: Vue Vuex
author: Max
mathjax: true
---

* content
{:toc}

#### Vuex 是什么？
>Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。Vuex 也集成到 Vue 的官方调试工具 devtools extension，提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。

#### vuex的作用
进行实时状态管理，

##### Mutations修改状态
* 它会接受 state 作为第一个参数

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {//参数state
      // 变更状态
      state.count++
    }
  }
})
```

* 提交载荷，格式

```js
mutations: {
  increment (state, payload) {//注意
    state.count += payload.amount
  }
}
store.commit('increment', {
  amount: 10
})
//对象风格提交方式
store.commit({
  type: 'increment',
  amount: 10
})
```

* mutation 必须是同步函数
所有的异步需求可以放在Actions,再提交到mutation

```js
mutations: {
  increment (state) {
    state.count++
  }
},
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
//----------------------------
// 以载荷形式分发
store.dispatch('incrementAsync', {
  amount: 10
})

// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

#### 热重载
支持开发过程中的webpack的热重载

#### 用vuex加localstorage配合，可以持久化特定状态
有个比较多的说法，是一起使用来实现购物车功能。
git上有一个项目，里面有一段这种方式实现购物车的功能。
点击查看：[项目地址][giturl]

[giturl]:https://github.com/sailengsi/sls-admin
