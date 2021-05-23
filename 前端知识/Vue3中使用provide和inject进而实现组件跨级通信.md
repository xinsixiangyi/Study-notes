## Vue3中使用provide和inject进而实现组件跨级通信.

在一般的情况下， 父组件向子组件通信我们一般采用 props 属性进行组件之间的通信， 但是如果在组件嵌套很深的情况下， 这个办法虽然可以， 但是却有点繁琐， 相比如果使用 vuex的话， 我们还需要安装这个插件， 所以Vue提供了一组方法 provide&&inject, 父组件中提供一个属性， provide, 子组件中使用一个属性进行值的接受 inject ， 这样不管组件之间层级嵌套有多深， 都可以实现组件之间的通信。
比如，通信中遇到这种情况:

- 来自官网的图:

![来自官网的图](https://img-blog.csdnimg.cn/20210302210553923.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1OTA2MjE5,size_16,color_FFFFFF,t_70)

在这里，我们需要进行组件1 和 组件5 的通信，那么这里就可以使用这一对属性了,

### 使用方法：

父组件

- js部分:

  ```js
  const app = Vue.createApp({
      data() {
        return {
          todos: ['Feed a cat']
        }
      },
      components: {
        cpn,  // 将子组件进行注册
      },
      provide() {
        return {
          todos: this.todos,
        }
      }
    })
    app.mount('#app')   //  挂载 app
  ```

- html 部分

  ```html
  <div id="app">
      <cpn></cpn>
    </div>
  ```

  

子组件

- js部分

  ```js
   const cpn = {
      template: '#tmp',
      inject: {
        todos: {
          from: 'todos',
          default: ""
        }
      },
    }
  ```

- html 部分

- ```html
  <template id="tmp">
      <div>
        {{todos.join(' ')}}
        <br>
        长度: {{todos.length}}
      </div>
    </template>
  ```

### 注意事项：

官网的案例是使用 ==app.component()== 进行全局的注册，而且二个都是这样的，在实际操作上， 发现这样我们是获取不到==inject==的值的， 二个组件必须构成父组件和子组件的形式， 也就是说一个组件必须注册到另一个组件之下， 即写入==compontens: {}== 中.
关于 provide 属性， 当我们使用对象的形式来描述的话，可能会出现问题， 抛出一些类似的err， 所以一般情况下，我们要给 provide 使用一个函数的方式进行放回.
关于 inject 属性， 官网给出的是使用数组的形式进行接受 inject:[''user],但是实际操作上，我们可以扩展使用对象的方法， 这就和 props 的普通写法和对象写法一样的作用 .

```js
const cpn = {
    template: '#tmp',
    inject: {
      todos: {
        from: 'todos',  // from 表示接受 provide 传入的值 
        default: ""  // 默认值 如果是对象或者函数  则使用函数返回默认值
      }
    },
  }

```

