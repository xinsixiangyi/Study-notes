# 什么是Vuex？

Vuex是Vue.js应用程序的**状态管理模式+库**。它充当应用程序中所有组件的集中存储，其规则确保只能以可预测的方式更改状态。

## [＃](https://next.vuex.vuejs.org/#what-is-a-state-management-pattern)什么是“状态管理模式”？

让我们从一个简单的Vue计数器应用开始：

```js
const Counter = {
  // state
  data () {
    return {
      count: 0
    }
  },
  // view
  template: `
    <div>{{ count }}</div>
  `,
  // actions
  methods: {
    increment () {
      this.count++
    }
  }
}

createApp(Counter).mount('#app')
```

这是一个包含以下部分的自包含应用程序：

- state(**状态**)，真理，推动我们的应用程序的来源;

- view(**视图**)，所述的声明性映射**状态**;
- actions（**动作**），是**视图**中用户输入的可能改变**状态**响应的方式
- 

这是“单向数据流”概念的简单表示：

![image-20201123162249490](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201123162249490.png)