---
title: 封装 axios 和 api
date: 2020-08-11 14:13:20
tags: basic
categories: Vue
---

# 封装 axios

axios 相比于 vue-resource，具有很多新特性，例如拦截请求和响应、取消请求、转换 json、客户端防御 XSRF 等。所以目前官方也是推荐使用 axios 库。

一般可以在项目的 src 目录中，新建一个 request 文件夹。其中新建一个 http.js 和 api.js 文件。http.js 用于封装 axios 文件，api.js 用来同意管理接口。

1. 安装 npm install axios

2. 在 http.js 中引入 axios

```javascript
import axios from 'axios'
import QS from 'qs' // qs模块用来序列化 post 类型的数据
```

3. 环境的切换

由于项目的环境可能有开发环境、测试环境和生产环境，通过 node 的环境变量来匹配默认的接口 url 的前缀，使用 axios.default.baseURL 设置默认的请求地址。

```javascript
// 环境的切换
if (process.env.NODE_ENV == 'development') {    
    axios.defaults.baseURL = 'https://www.baidu.com'
} 
else if (process.env.NODE_ENV == 'debug') {    
    axios.defaults.baseURL = 'https://www.ceshi.com'
} 
else if (process.env.NODE_ENV == 'production') {    
    axios.defaults.baseURL = 'https://www.production.com'
}
```

4. 设置接口请求超时

使用 axios.default.timeout 设置默认的请求超时时间。

```javascript
axios.defaults.timeout = 10000  // 10s
```

5. post 请求头的设置

post 请求的时候，我们需要加上请求头，所以可以对其进行一个默认设置将其请求头设为 application/x-www-form-urlencoded;charset=UTF-8。

```javasript
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded;charset=UTF-8';
```

6. 请求拦截

对请求进行拦截的原因是，有的数据接口，是需要用户有相关的权限才可以进行访问（比如登录之后才可以查看个人信息）。又或者在发送 post 请求的时候，需要序列化我们提交的数据，这时候需要在发送请求之前进行一个拦截，进行相关的其他操作。

```javascript
// 请求拦截器

axios.interceptors.request.use(
  config => {
    // 发送请求之前进行相应的操作
    ...
  },
  error => {
    return Promise.error(error)
  }
)
```

7. 响应拦截

响应拦截，就是针对服务器返回的数据，我们在拿到之前对数据进行相应的处理。例如后台返回的状态码是正常，则正常返回数据，否则，需要根据错误的状态码类型，进行统一的错误处理。

```javascript
// 响应拦截器
axios.interceptors.response.use(
  response => {
    // 如果返回的状态码是 200，则说明接口请求成功，返回正常数据
    // 否则抛出错误
    if(response.status === 200) {
      return Promise.resolve(response)
    } else {
      return Promise.reject(response)
    }
  },
  error => {
    if (error.response.status) {
      switch (error.response.status){
      // 401: 未登录，则跳转登录页面，并且携带当前页面的路径
      case 401:
        router.replace({
          path: '/login',
          query: {
            redirect: router.currentRoute.fullPath
          }
        })
        break;
      // 403 token 过期，提示用户并且清除本地 token，并跳转至登录页
      case 403:
        ...
      // 404 请求不存在
      case 404:
        ...
      // 其他错误，直接抛出错误
      default: 
        ...
      }
      return Promise.reject(error.response)
    }
  }
)
```

# 在 http.js 中使用 axios 封装 get 和 post 方法

1. 封装 get 方法

```javascript
/**
 * @desc 封装 get 方法，对应 get 请求，接收两个参数
 * @param {String} url [请求的 url 地址]
 * @param {Object} params [请求时携带的参数]
 */
export function get (url,params) {
  return new Promise((resolve,reject) => {
    axios.get(url, {
      params: params
    }).then(res => {
      resolve(res.data)
    }).catch(err =>{
      reject(err.data)
    })
  })
}
```

2. 封装 POST 方法

原理同 get，但是需要注意 post 方法需要对提交过来的参数进行序列化操作。所以这里需要使用到 node 的 qs 模块，否则后台无法拿到前端提交的数据。

```javascript
import QS from 'qs'
/**
 * @desc 封装 post 方法，对应 post 请求，接收两个参数
 * @param {String} url { 请求的url地址 }
 * @param {Object} params [ 请求时携带的参数 ]
 */
export function post(url,params) {
  return new Promise((resolve,reject) => {
    axios.post(url,QS.stringify(params))
    .then(res => {
      resolve(res.data)
    })
    .catch(err => {
      reject(err.data)
    })
  })
}
```

# 封装 api.js 类

```javascript
import querystring from 'querystring'
import request from '@/utils/request'   // axios 封装的实例
class Api {
  constructor(resource) {
    this.resource = resource
  }
  
  async list(payload) {
    const resp = await request.get(`/${this.resource}?${querystring.stringify(payload)}`)
    return resp
  }

  async get(id) {
    const resp = await request.get(`/${this.resource}/${id}`)
    return resp
  }

  async post(payload) {
    const resp = await request.post(`/${this.resource}`, payload)
    return resp
  }

  async delete(uuid) {
    const resp = await request.delete(`/${this.resource}/${uuid}`)
    return resp
  }

  async update(uuid, payload) {
    const resp = await request.put(`/${this.resource}/${uuid}`, payload)
    return resp
  }

  async create(payload) {
    const resp = await request.post(`/${this.resource}`, payload)
    return resp
  }
}

export default Api
```

1. api 文件夹中 user.js： 

```javascript
import Api from './api'
// 传入参数中的 api 是在 vue.config.js 中使用代理配置的 target 前缀
const wechatApi = new Api('api/notice/wechat')

export async function sendByWechat (payload) {
  const res = wechatApi.post(payload)
  return res
}
```

2. 使用 Vuex 时在 store/modules 文件夹：

```javascript
import { sendByWechat } from '@/api/user'

const state = {}

const mutations = {}

const actions = {
  async sendByWechat (_, payload) {
    const res = await sendByWechat({ user_id: payload })
    return res
  }
}

const getters = {}

export default {
  namespaced: true,
  state,
  getters,
  actions,
  mutations
}
```

3. 在页面中调用方法

```javascript
    const { data: res } = await this.$store.dispatch(
      "user/sendByWechat",
      row.uuid.toString()
    )
```

# 完整的 axios 配置

```javascript
/**
 * axios封装
 * 请求拦截、响应拦截、错误统一处理
 */
import axios from 'axios'
import router from '../router'
import store from '../store/index'
import { Toast } from 'vant'

/** 
 * 提示函数 
 * 禁止点击蒙层、显示一秒后关闭
 */
const tip = msg => {    
    Toast({        
        message: msg,        
        duration: 1000,        
        forbidClick: true    
    })
}

/** 
 * 跳转登录页
 * 携带当前页面路由，以期在登录页面完成登录后返回当前页面
 */
const toLogin = () => {
    router.replace({
        path: '/login',        
        query: {
            redirect: router.currentRoute.fullPath
        }
    });
}

/** 
 * 请求失败后的错误统一处理 
 * @param {Number} status 请求失败的状态码
 */
const errorHandle = (status, other) => {
    // 状态码判断
    switch (status) {
        // 401: 未登录状态，跳转登录页
        case 401:
            toLogin();
            break;
        // 403 token过期
        // 清除token并跳转登录页
        case 403:
            tip('登录过期，请重新登录');
            localStorage.removeItem('token');
            store.commit('loginSuccess', null);
            setTimeout(() => {
                toLogin();
            }, 1000);
            break;
        // 404请求不存在
        case 404:
            tip('请求的资源不存在'); 
            break;
        default:
            console.log(other);   
        }}

// 创建axios实例
const instance = axios.create({
  timeout: 1000 * 12,
  withCredentials: true,
  xsrfHeaderName: 'x-csrf-token',
  headers: {
    'Content-Type': 'application/json'
  }
})
// 设置post请求头
instance.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'

/** 
 * 请求拦截器 
 * 每次请求前，如果存在token则在请求头中携带token 
 */ 
instance.interceptors.request.use(    
    config => {        
        // 登录流程控制中，根据本地是否存在token判断用户的登录情况        
        // 但是即使token存在，也有可能token是过期的，所以在每次的请求头中携带token        
        // 后台根据携带的token判断用户的登录情况，并返回给我们对应的状态码        
        // 而后我们可以在响应拦截器中，根据状态码进行一些统一的操作。        
        const token = store.state.token
        token && (config.headers.Authorization = token)
        return config
    },    
    error => Promise.error(error))

// 响应拦截器
instance.interceptors.response.use(    
    // 请求成功
    res => res.status === 200 ? Promise.resolve(res) : Promise.reject(res),    
    // 请求失败
    error => {
        const { response } = error;
        if (response) {
            // 请求已发出，但是不在2xx的范围 
            errorHandle(response.status, response.data.message);
            return Promise.reject(response);
        } else {
            // 处理断网的情况
            // eg:请求超时或断网时，更新state的network状态
            // network状态在app.vue中控制着一个全局的断网提示组件的显示隐藏
            // 关于断网组件中的刷新重新获取数据，会在断网组件中说明
            if (!window.navigator.onLine) {
               store.commit('changeNetwork', false)
            } else {
                return Promise.reject(error)
            }
        }
    });

export default instance
```

# 参考文章

(vue中Axios的封装和API接口的管理)[https://juejin.im/post/6844903652881072141]