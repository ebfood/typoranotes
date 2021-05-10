# axios与封装

## 封装

创建新的axios实例，接收请求配置参数，处理参数，添加配置，每次请求都会创建新的axios实例.

```javascript
const request = axios.create({
  baseURL: url,
  headers: { 'Content-Type': 'application/json; charset=utf-8' },
  // ... 其他需要的配置
})

// 拦截器等

export default request
```



## 根据环境配置采用不同的baseURL

开发环境, 测试环境, 生产环境对应不同的baseURL

封装函数再通过NODE_ENV, VUE_APP_ENV自动判断.

```javascript
const getBaseUrl = (env) => {
  let base = {
    pro: '/',
    dev: 'http://localhost:3000',
    test: 'http://localhost:3001',
  }[env];
  if (!base) {
    base = '/';
  }
  return base;
};


const request = axios.create({
  baseURL: getBaseUrl(process.env.VUE_APP_ENV)
})
```

或者通过模块导出

```javascript
// baseURL.js
let url;

switch (process.env.VUE_APP_ENV) {
  case 'test':
    url = ''
    break

  case 'pro':
    url = ''
    break

  default:
    url = ''
}

export default url;
```


## 拦截器

**截每一次request和response，然后进行相应的处理**

```javascript
const instance = axios.create({
    // ...
})
    
// http request 拦截器
instance.interceptors.request.use(
    (config) => {
    	// 例如判断是否存在token
        const token = sessionStorage.getItem('token')
        if (token) { 
          config.headers.authorization = token
        }
    	return config
	},
    (err) => {
        //uni.showToast ...
        return Promise.reject(err)
    }
)

// response拦截器
request.interceptors.response.use(
  (res) => {
    // 对响应成功的处理
  	// 根据业务码来判断本次操作是否成功
    const { data } = res
    if (data.code !== 0) { // 错误
        // ... 
        return Promise.reject(data)
    } else {
        // ...
        return data
    }
  },
  (err) => {
  	// 对响应错误的处理
    return Promise.reject(err)
  } 
)
```



## 参考文章

- https://juejin.cn/post/6844904033782611976
- https://www.jianshu.com/p/a0c67f4e145e