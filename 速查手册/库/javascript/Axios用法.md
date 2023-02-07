

# Axios用法


## 功能
* 支持Promise的网络请求库
* 支持浏览器和node.js平台


## 请求

### 创建实例
```ts
const instance = axios.create({
	//基础链接地址
	baseURL: 'https://some-domain.com/api/',  
	//连接超时时间
	timeout: 1000,
	//自定义请求头
	headers: {'X-Custom-Header': 'foobar'}
	})
```

### 发送GET请求
```ts
axios.get('/user?ID=12345')
  .then((resp) => { 
	  //处理成功情况 
	  resp.data  //返回的响应数据
	  resp.status  //返回的HTTP状态码
	  resp.statusText //返回的HTTP状态信息
	  resp.headers   //返回的响应头
  })
  .catch((error) => { //处理错误情况 })
  .then(() => { //总是会执行 })
```

### 发送POST请求
```ts
axios.post('/user', { //body数据
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then((resp) => { //处理成功情况 })
  .catch((error) => { //处理错误情况 })
```


### 取消请求
```ts
const source = axios.CancelToken.source()

axios.get('/user/12345', {
  cancelToken: source.token
}).catch((error) => {
  if (axios.isCancel(error)) {
    console.log('Request canceled', error.message)
  } else {
    // 处理错误
  }
})

// 取消请求（message 参数是可选的）
source.cancel('Operation canceled by the user.')
```


## 其他


### 拦截器
```ts
// 请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前处理
    return config
  }, function (error) {
    // 处理请求错误
    return Promise.reject(error)
  })

// 响应拦截器
axios.interceptors.response.use(function (resp) {
    // 2xx内的状态码进入该函数
    // 处理响应数据
    return resp
  }, function (error) {
    // 2xx外的状态码进入该函数
    // 处理响应错误
    return Promise.reject(error)
  })
```


