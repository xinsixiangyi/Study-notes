## vue cli项目文件结构：

![image-20201120102440204](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120102440204.png)

具体说明：

```bash
node_modules:用于存放我们项目的各种依赖，比如axios等等，没有moudles文件，项目就没法运行，可以使用 npm install进行项目依赖的安装
public:用于存放静态文
public/index.html:是一个模板文件，作用是生成项目的入口文件，webpack打包的js,css也会自动注入到该页面中。我们浏览器访问项目的时候就会默认打开生成好的index.html
src:我们存放各种vue文件的地方
src/assets:用于存放各种静态文件，如图片
src/compnents：用于存放我们的公共组件，如 header、footer等
src/App.vue:主vue模块 引入其他模块，app.vue是项目的主组件，所有页面都是在app.vue下切换的
src/main.js:入口文件，主要作用是初始化vue实例，同时可以在此文件中引用某些组件库或者全局挂在一些变量
package.json:模块基本信息项目开发所需要模块，版本，项目名称
package-lock.json:是在 npm install时候生成一份文件，用以记录当前状态下实际安装的各个npm package的具体来源和版本号
babel.config.js:是一个工具链，主要用于在当前和较旧的浏览器或环境中将ECMAScript 2015+代码转换为JavaScript的向后兼容版本
.gitignore:git上传需要忽略的文件格式
vue.config.js:保存vue配置的文件，可以用于设置代理,打包配置等
```

### 一. dist

dist文件夹在新建项目中一开始并不会存在。只有当你执行过一次构建命令（build）后，才会创建。通常它的内部目录结构为：

```javascript
vue-demo
├── dist  //项目构建后的输出目录
│   └── css
│   └── img
│   └── js
│   └── index.html  // 项目主入口文件
│   └── ...  // 其他公共资源
```

**这就是我们之前很熟悉的原生开发阶段的目录结构。也是浏览器能直接识别的文件类型**。而我们现在使用的Vue.js等框架开发的项目，并不能为浏览器所识别，所以就需要编译打包这一步操作，来转换成实际生产环境（浏览器）所需的文件。

### 二. node_modules

`node_modules`文件夹中存放的是各种项目依赖文件，其中包括很多基础依赖和自己安装的依赖。

```javascript
vue-demo
├── node_modules
│   └── ...
123
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020101510425819.gif#pic_center)

**在做代码共享或者上传远程仓库时，建议忽略此文件夹**。所以我们在拿到一个Vue项目时，一般都是没有这个文件夹的。需要我们自己使用命令去生成：

```javascript
npm install
```

这样他就会去下载项目所需的所有依赖文件。除了基础的依赖文件之外，他还会去识别我们`package.json`文件中保存的依赖信息并逐一安装。

之后项目开发中，我们也可以根据需要继续增添其他依赖，仍然会保存在`node_modules`文件夹中。

```javascript
npm install [依赖包名称]
```

### 三. public

顾名思义，`public`文件夹中存放的是项目公共资源。比如网站LOGO等，还会有项目的主入口文件`index.html`。通常我们不需要对`public`文件夹内的资源做任何修改。

```javascript
vue-demo
├── public
│   └── index.html  // 项目主入口文件
│   └── ...  // 其他公共资源
1234
```

后续在构建打包时，`public`文件内容会直接放到`dist`文件夹内。比如：

![image-20201120143357178](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120143357178.png)

### 四. src（基础版）

`src`文件夹是我们项目的核心文件夹：包括项目源码以及各种静态资源等等。是我们开发的重点工作目录。

如果此时你的项目在创建的时候，并没有选装其他的配置，仅仅是一个最简单的空白项目，那么你的src文件夹应该是这样的：

![image-20201120143522113](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120143522113.png)

```
├── src
│   └── assets  //静态资源
│   └── components  //公共组件
│   └── App.vue  //根组件
│   └── main.js  //入口文件
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015152957904.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0paZXZpbg==,size_16,color_FFFFFF,t_70#pic_center)

这也是Vue框架最基础的工作原理。后续可以在此基础上继续增添其他的Vue生态产品或插件，比如Vue Router，Vuex或者Element UI组件库等等。

#### 4.1 main.js

`main.js`文件是一个很重要的文件，是浏览器解析最先加载的入口文件。这个文件的主要功能是通过import的方式导入各种资源，然后新建了一个vue实例。

![image-20201120155641396](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120155641396.png可以看到这里只导入了两种资源：

![image-20201120175127713](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120175127713.png)

- **Vue框架** —— 从`node_modules`文件夹中导入；

- **根组件App** —— 从`App.vue`文件导入；

  然后新建了一个vue实例：

  ```js
  createApp(App).mount('#app')
  ```

  该Vue实例中通过h函数渲染根组件App，并把他手动挂载到id为`#app`的节点上。该节点位于`public`目录下的主入口文件`index.html`中。

  其中`< div id=“app”></div>`将会被根组件App替换掉，即所谓的挂载。

  ![image-20201120161128632](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120161128632.png)

#### 4.2 App.vue

所以进一步加载的是根组件App文件 —— `App.vue`。之前我们在讲Vue框架入门的时候，说到Vue框架中有一个很重要的核心理念：

- **组件化开发** —— 每一个Vue页面都可以看作是由组件填充而成的，组件之间支持层级嵌套。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015162154689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0paZXZpbg==,size_16,color_FFFFFF,t_70#pic_center)
App组件就属于根组件，其他组件的使用均需要由App组件直接或间接引入。这里就继续引入了如下两项资源：

- 图片`logo.png` —— 位于`src / assets`目录；

- 公共组件`HelloWorld` —— 位于`src / components`目录；

  ![image-20201120162308391](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120162308391.png)

#### 4.3 src / assets

`src / assets` 文件夹内保存的是各种静态资源，比如css、img、js、font等。新建项目的`assets`文件夹内就只有一张首页上的LOGO图片资源：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015111117693.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0paZXZpbg==,size_16,color_FFFFFF,t_70#pic_center)

![image-20201120163234216](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120163234216.png)

#### 4.4 src / components

这里定义的组件都属于公共组件，任意的Vue页面中都可以（多次）调用。后缀为`.vue`的文件即为组件文件，每一个vue组件通常都由以下三部分组成：

- `<template>`标签 —— HTML模板代码片段；
- `<script>`标签 —— javascript代码；
- `<style>`标签 —— 样式代码（原生CSS / 预处理语言Sass，Less，Stylus）；

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015181534270.gif#pic_center)

可以看到原生开发中的三剑客html，js，css都整合在了一个组件文件内（麻雀虽小五脏俱全呀）。这样的开发方案无疑与原生开发区别很大，所以才需要最后的构建打包。

我们在运行这个项目后看到的内容，其实是公共组件`HelloWorld`的内容：

![image-20201120165759883](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120165759883.png)

还有一点是这里用到了子父组件传值。根组件App在调用公共组件`HelloWorld`时，向下传递了一个msg值

![image-20201120170401185](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120170401185.png)

然后`HelloWorld`公共组件内通过props方法接收，并渲染。

![image-20201120173358416](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120173358416.png)

我们可以来尝试修改msg的值和`HelloWorld`公共组件内的其他内容。比如：

```javascript
// App.vue

<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome ! zevin ."/>
  </div>
</template>
12345678
// HelloWorld.vue

<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>
1234567
```

现在的页面效果就是这样啦~
![image-20201120174252447](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120174252447.png)

### 五. src（顶配版）

刚才讲的基础版`src`，如果用来理解Vue框架原理是极好的，不过在实际开发中肯定是不够用的。我们或多或少都会用到Vue生态里的其他产品。

所以就有了顶配版的`src`，哈哈。详细的目录结构如下：

```javascript
vue-demo
├── src
│   └── assets  //静态资源
│   └── components  //公共组件
│   └── plugins  //插件资源
│   └── router  //路由配置
│   └── store  //vuex文件
│   └── views  //视图组件
│   └── App.vue  //根组件
│   └── main.js  //入口文件
12345678910
```

对比基础版你会发现，这里又多出了四个文件夹：

- `plugins` —— 插件资源；
- `router` —— 路由配置；
- `store` —— vuex文件；
- `views` —— 视图组件；

对应的文件之间的调用图也该丰富一下了。请看下图：

![image-20201120174739218](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201120174739218.png)

而且Vue实例中也增加了相应的内容：

```vue\
createApp(App).use(store).use(router).use(Antd).mount('#app')
```

#### 5.1 src / plugins

这个文件夹是你项目开发过程中，手动安装过插件而产生的。比如我这里安装了axios：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015191930527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0paZXZpbg==,size_16,color_FFFFFF,t_70#pic_center)

#### 5.2 src / store

store文件夹是你选配了Vuex之后才会有的文件，主要用于项目内某些状态的保存。比如state、mutations、actions、getters、modules等。

#### 5.3 src / router

在**单页面应用**中，路由是指：**根据不同的链接展示不同的页面视图，可以来回切换**。如下图所示：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015202841972.gif#pic_center)

在看`src / router`文件之前，安装xxx插件后，先来看看根组件App—— `App.vue`文件的变化：不再直接调用公共组件了，而是引用了`< router-view/>`路由。

```VUE
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>
```

路由的具体原理是：

根据上边`<router-link>`标签内`to`字段的取值，决定了`<router-view/>`这里具体挂载哪个视图组件。

这种路由链接的对应关系就保存在`src/router/index.js`文件中。	

![image-20201123143921919](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201123143921919.png)

#### 5.4 src / views

`src / views`文件夹内存放的是视图组件。选配了Vue Router的新建项目，都会默认有以下两个视图组件：

- `Home.vue` —— 首页版块；
- `About.vue` —— 关于版块；

而在`Home.vue`中可以看到很熟悉的，和之前基础版中的根组件`App.vue`相同的模板内容。这样就接上之前基础版的思路啦~

### 六. tests

选装了测试模块（Unit Testing / E2E Testing）才会有的文件夹。不过我自己对测试没什么研究，也就不班门弄斧了~

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015205538674.png#pic_center)

### 七. .gitignore

这个也没什么好说的，也就是罗列一下，之后Git上传时，需要忽略的文件。默认的 `.gitignore`文件内就总结了挺多，可以自由的增删改。

![image-20201123150140954](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201123150140954.png)

### 八. README.md

`README.md`文件是你项目的描述文件。可有可无，不影响什么。它的作用就是：

在你上传为Git远程仓库资源时，比如GitHub。他就会解析你项目根目录下的`README.md`文件，并展示在你仓库目录的下方。方便别人快速了解你的项目。

### 九. …（其他依赖的独立配置信息文件）

在创建项目时，如果你选择把依赖的配置信息以独立文件形式存放，那么你的项目内，和`package.json`文件同级的，就还会有比如`.browserslistrc`， `.eslintrc`， `babel.config.js`， `jest.config.js`， `tsconfig.json`等独立配置文件。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015210012347.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0paZXZpbg==,size_16,color_FFFFFF,t_70#pic_center)

## 附：详细目录结构树

我这里是尽可能全的总结了一个Vue项目的实际目录结构。仅供参考，具体的目录结构请以你们的实际为主。

```javascript
vue-demo
├── dist  //项目构建后的输出目录
├── node_modules
│   └── ...  // 各类依赖
├── public
│   └── index.html  // 项目主入口文件
│   └── ...  // 其他公共资源
├── src
│   └── assets  //静态资源
│       └── logo.png
│   └── components  //公共组件
│       └── HelloWorld.vue
│   └── plugins  //插件资源
│       └── axios.js
│   └── router  //路由配置
│       └── index.js
│   └── store  //vuex文件
│       └── index.js
│   └── views  //视图组件
│       └── About.vue
│       └── Home.vue
│   └── App.vue  //根组件
│   └── main.js  //入口文件
├── tests  //选装：测试模块
├── .gitignore  //Git上传时需要忽略的文件列表
├── package-lock.json  //版本管理使用的文件
├── package.json  //项目的基本配置信息文件
├── README.md  //项目的描述文件
├── ...  //其他依赖的独立配置信息文件
```

...

