---
nav:
  title: 拦截器
  order: 1
  second:
    title: axios的拦截器
    order: 0
---

# axios的拦截器

## 基本概念

在 Axios 中，拦截器是一个由两个部分组成的对象：请求拦截器（request interceptors）和响应拦截器（response interceptors）。这两种拦截器允许我们在请求发出之前和收到响应后对其进行预处理。

**请求拦截器**（request interceptors） 用于在请求被发送之前修改请求配置，或者在发送请求前进行一些操作，例如添加认证信息、设置请求头等。

**响应拦截器**（response interceptors） 用于在接收到响应数据之后进行处理，可以对响应数据进行转换、错误处理等操作。

Axios 拦截器是链式结构的，这意味着可以同时使用多个拦截器，并且它们按照添加顺序依次执行。

## 拦截器用法

在请求或响应被 then 或 catch 处理前拦截它们。

```javascript
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

如果稍后需要移除拦截器，可以这样：

```javascript
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

可以给自定义的 axios 实例添加拦截器。

```javascript
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```

## 实战案例

### 定义axios的请求拦截器

```javascript
axios.interceptors.request.use((config) => {
  // 添加请求token
  config.headers['Authorization'] = sessionStorage.getItem('jwtToken');
  // 如果是get请求，那么就给params赋值，否则给data赋值
  if (config.method !== 'get') {
    config.data = config.params || {};
    delete config.params;
  }
  return config;
});
```

1. 从`sessionStorage`中获取名为`jwtToken`的值，并将其添加到请求头中，以便在后续的请求中使用。
2. 根据请求的方法（GET、POST等）来决定将请求参数（`params`）还是数据（`data`）赋值给`config`对象。如果是GET请求，则将`params`赋值给`data`，并将`data`删除（因为GET请求中通常不携带数据）。
3. 最后，返回修改后的`config`对象，以便在发送请求时使用。

### 定义axios的响应拦截器

```javascript
axios.interceptors.response.use((response) => {
  return response;
}, (error) => {
  console.log('请求失败', error)
  // 如果没有开启自动异常捕获，那么就直接返回错误
  if (!error.config.catchError) {
    return Promise.reject(error);
  }
  const url = error.config.url;
  // 对url进行简化，仅保留最后两个斜杠后的内容
  const simpleUrl = url.replace(/.*\/.*\//, '');
  notification.error({
    key: 'requestError',
    message: '请求失败',
    description: '接口：/' + simpleUrl + ' 发生了' + httpCodeTextObj[error.response.status] || '未知错误'
  });
  return Promise.reject(error);
});
```

这段代码定义了一个axios的响应拦截器，它的作用是拦截axios发出的响应。当拦截到的响应被处理完毕并返回给调用者时，这段代码会执行相应的处理。以下是这段代码的实现原理、用途和注意事项：

1. 定义一个拦截器，接收两个参数：响应（response）和错误（error）。
2. 在拦截器内部，首先判断是否没有开启自动异常捕获（`error.config.catchError`）。如果没有开启，那么就直接返回错误。
3. 对响应进行处理：从响应中提取出接口的URL，并将其简化。
4. 使用`notification`组件发送一个错误通知。通知的内容包括接口、错误码和错误信息。
5. 最后，返回错误。

## 其他

官方文档：https://www.axios-http.cn/docs/interceptors

vue3的axios学习：https://juejin.cn/post/7240789855138676795
