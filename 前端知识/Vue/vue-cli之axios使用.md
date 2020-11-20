**axios**

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

- 在项目所在文件夹下运行以下命令：

```shell
npm install axios
```

- 在`src`目录下创建`utils/`， 并创建`request.js`用来封装`axios`

```js
import axios from 'axios'
 
// 创建axios实例
const instance = axios.create({
  baseURL: 'http://test.apiserver.com/', // api的base_url
  timeout: 15000,                 // 请求超时时间
  headers: {
    'key': 'value'
  }
})
 
// 拦截请求
instance.interceptors.request.use(config => {
  // 发送请求之前做些什么
  return config
}, error => {
//  请求错误做些什么
  return Promise.reject(error)
})
 
// 拦截响应
instance.interceptors.response.use(res => {
  // 对响应数据做些什么
  return res
}, error => {
// 对响应错误做些什么
  return Promise.reject(error)
})
 
export default instance
```

- 在`src/`下创建`api`目录，用来统一管理所有的请求：

例如创建user.js：

```js
import request from '@utils/request.js'
 
export default{
  login(data){
    return request({
      url:'/login/',
      method:'post',
      data,
    })
  }
}
```

这样的好处是方便管理、后期维护，还可以和后端的微服务对应，建立多文件存放不同模块的`api`。剩下的就是你使用到哪个api时，自己引入便可。