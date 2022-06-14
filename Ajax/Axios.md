## 准备工作

1. 作为一个前端开发工程师，在后端还没有ready的时候，不可避免的要使用mock的数据。很多时候，我们并不想使用简单的静态数据，而是希望自己起一个本地的mock-server来完全模拟请求以及请求回来的过程。json-server是一个很好的可以替我们完成这一工作的工具。我们只需要提供一个json文件，或者写几行简单的js脚本就可以模拟出RESTful API的接口。
2. 安装json-server `npm install -g json-server`
3. 创建db.json 在一个文件夹下新建一个db.json文件

```js
{
"posts": [
  { "id": 1, "title": "json-server", "author": "typicode" }
],
"comments": [
  { "id": 1, "body": "some comment", "postId": 1 }
],
"profile": { "name": "typicode" }
}
```

4. 启动json-server在当前文件夹下输入如下命令:  `json-server db.json`

# 一、Axios的理解与使用

## 1.1 axios概念

1. 前端最流行的ajax请求库
2. react/vue 官方推荐使用axios发送ajax请求

## 1.2 axios特点

1. 基于 xhr + promise 的异步 ajax 请求库
2. 浏览器端/node 端都可以使用
3. 支持请求 / 响应拦截器
4. 支持请求取消
5. 请求/响应数据转换
6. 批量发送多个请求

## 1.3 axios常用语法

> 1. axios(config): `通用/最本质`的发任意类型请求的方式
> 2. axios(url[, config]): 可以只指定 url 发 get 请求
> 3. axios.request(config): 等同于 axios(config)
> 4. axios.get(url[, config]): 发 get 请求
> 5. axios.delete(url[, config]): 发 delete 请求
> 6. axios.post(url[, data, config]): 发 post 请求
> 7. axios.put(url[, data, config]): 发 put 请求
> 8. axios.defaults.xxx: 请求的默认全局配置
> 9. axios.interceptors.request.use(): 添加请求拦截器
> 10. axios.interceptors.response.use(): 添加响应拦截器
> 11. axios.create([config]): 创建一个新的 axios(它没有下面的功能)
> 12. axios.Cancel(): 用于创建取消请求的错误对象
> 13. axios.CancelToken(): 用于创建取消请求的 token 对象
> 14. axios.isCancel(): 是否是一个取消请求的错误
> 15. axios.all(promises): 用于批量执行多个异步请求
> 16. axios.spread(): 用来指定接收所有成功数据的回调函数的方法

## 1.4 难点语法的理解和使用

#### 1. axios.create(config)

1. 根据指定配置创建一个新的axios, 也就是每个新的 axios 都有自己的配置
2. 新axios只是没有取消请求和批量发送请求的方法, 其它所有语法都是一致的
3. 为什么设计这个语法？
   1. 需求：项目中有部分接口需要的配置与另一部分接口需要的配置不太一样，如何处理。
   2. 解决：创建两个新axios，每个都有自己特有的配置，分别应用到不同要求的接口请求中。


```js
// 创建实例对象 /getJoke接口已不可访问
const duanzi = axios.create({
    baseURL: 'http://localhost:3000/posts',
    timeout: 2000
});
const another = axios.create({
    baseURL: 'https://b.com',
    timeout: 2000
});
// 这里 duanzi 和 axios 对象的功能几乎是一样的
// duanzi({
//    url:'/getJoke',
// }).then(response => {
//     console.log(response);
// });
duanzi.get('/getJoke').then(response => {
    console.log(response.data);
});
```

#### 2. 拦截器函数/ajax请求/请求的回调函数的调用顺序

1. 说明:  调用axios()并不是立即发送ajax请求, 而是需要经历一个较长的流程
2. 流程:  请求拦截器2 => 请求拦截器1 => 发送ajax请求 => 响应拦截器1 => 响应拦截器2 => 请求的回调
3. 注意:  此流程是通过`promise`串联起来的,  请求拦截器传递的是`config`,  响应拦截器传递的是`response`

```js
<script>
	// Promise
	// 设置请求拦截器 config 配置对象
	axios.interceptors.request.use(function(config) {
			console.log("请求拦截器 成功 - 1号");
			// 修改 config 中的参数
			config.params = {
					a: 100
			};
			return config;
	}, function(error) {
			console.log("请求拦截器 失败 - 1号");
			return Promise.reject(error);
	});

	axios.interceptors.request.use(function(config) {
			console.log("请求拦截器 成功 - 2号");
			// 修改 config 中的参数
			config.timeout = 2000;
			return config;
	}, function(error) {
			console.log("请求拦截器 失败 - 2号");
			return Promise.reject(error);
	});

	// 设置响应拦截器
	axios.interceptors.response.use(function(response) {
			console.log("响应拦截器 成功 1号");
			return response.data;
			
	}, function(error) {
			console.log("响应拦截器 失败 1号");
			return Promise.reject(error);
	});

	axios.interceptors.response.use(function(response) {
			console.log("响应拦截器 成功 2号");
			return response;
	}, function(error) {
			console.log("响应拦截器 失败 2号");
			return Promise.reject(error);
	});

	// 发送请求
	axios({
			method: 'GET',
			url: 'http://localhost:3000/posts'
	}).then(response => {
			console.log("自定义回调处理成功的结果");
			console.log(response);
	}).catch(reason => {
		console.log('自定义失败回调');
	});
</script>
```

#### 3. 取消请求

1. 基本流程 配置 `cancelToken`对象
2. 缓存用于取消请求的`cancel`函数
3. 在后面特定时机调用`cancel`函数取消请求
4. 在错误回调中判断如果`error`是`cancel`,  做响应处理
5. 实现功能`点击按钮`, 取消某个正在请求中的请求

```js
<script>
  // 获取按钮
  const btns = document.querySelectorAll('button');
  // 2.声明全局变量
  let cancel = null;
  // 发送请求
  btns[0].onclick = function() {
    // 检测上一次的请求是否已完成
    if (cancel !== null) {
      // 取消上一次的请求
      cancel();
    }
    axios({
      method: 'GET',
      url: 'http://localhost:3000/posts',
      // 1.添加配置对象的属性
      cancelToken: new axios.cancelToken(function(c) {
        // 3.将 c 赋值给cancel
        cancel = c;
      })
    }).then(response => {
      console.log(response);
      // 将 cancel 的值初始化
      cancel = null;
    })
  }

  // 绑定第二个事件取消请求
  btns[1].onclick = function(){
    cancel();
  }
</script>
```

#### 4. 默认配置

```js
//默认配置
axios.defaults.method = 'GET';//设置默认的请求类型为 GET
axios.defaults.baseURL = 'http://localhost:3000';//设置基础 URL
axios.defaults.params = {id:100};
axios.defaults.timeout = 3000;//

btns[0].onclick = function(){
    axios({
        url: '/posts'
    }).then(response => {
        console.log(response);
    })
}
```

# 二、Axios难点问题

#### 1. axios与Axios的关系

1. 从 **语法** 上来说:  axios不是Axios的实例
2. 从 **功能** 上来说:  axios是Axios的实例
3. axios是`Axios.prototype.request`函数`bind()`返回的函数
4. axios作为对象有Axios原型对象上的所有方法,  有Axios对象上所有属性

#### 2. instance与axios的区别

**相同点:**

1. 都是一个能发送任意请求的函数:  `request(config)`
2. 都有发送特定请求的各种方法:  `get()/post()/put()/delete()`
3. 都有默认配置和拦截器的属性:  `defaults/interceptors`

**不同点:**

1. 默认配置很可能不一样
2. instance没有axios后面添加的一些方法:  `create()/CancelToken()/all()`

#### 3. axios运行的整体流程

1. 整体流程: `request(confit) => dispatchRequest(config) => xhrAdapter(config)`
2. `request(config)`:  将请求拦截器 / dispatchRequest() / 响应拦截器 通过promise链串联起来, 返回promise
3. `dispatchRequest(config)`:  转换请求数据 => 调用xhrAdapter()发送请求 => 请求返回后转换响应数据, 返回 promise
4. `xhrAdapter(config)`:  创建xhr对象, 根据config进行响应设置,  发送特定请求,  并接收响应数据,  返回promise

#### 4. axios的请求/响应拦截器

1. 请求拦截器
   1. 在真正发送请求前执行的回调函数
   2. 可以对请求进行检查或配置进行特定处理
   3. 成功的回调函数, 传递的默认是config(也必须是)
   4. 失败的回调函数, 传递的默认是error
2. 响应拦截器
   1. 在请求得到响应后执行的回调函数
   2. 可以对响应数据进行特定处理
   3. 成功的回调函数, 传递的默认是response
   4. 失败的回调函数, 传递的默认是error

#### 5. axios的请求/响应数据转换器

1. 请求转换器:  对请求头和请求体数据进行特定处理的函数

```js
if (utils.isObject(data)) {
    setContentTypeIfUnset(headers, 'application/json; charset=utf-8');
    return JSON.stringify(data);
}
```

2. 响应转换器:  将响应体json字符串解析为js对象或数组的函数

```js
response.data = JSON.parse(response.data);
```

#### 6. response 与 error 的整体结构

1. response的整体结构

```js
{data, status, statusText, headers, config, request}
```

2. error的整体结构

```js
{message, response, request}
```

#### 7. 如何取消未完成的请求

1. 当配置了`cancelToken`对象时, 保存`cancel`函数
   1. 创建一个用于将来中断请求的`cancelPromise`
   2. 并定义了一个用于取消请求的`cancel`函数
   3. 将`cancel`函数传递出来
2. 调用`cancel()`取消请求
   1. 执行`cancel()`, 传入错误信息
   2. 内部会让`cancelPromise`变为成功, 且成功的值为一个`Cancel`对象
   3. 在`cancelPromise`的成功回调中中断请求, 并让发送请求的`promise`失败, 失败的reason为Cancel对象

# 三、Axios源码模拟实现

#### 1. 模拟axios创建过程

```js
<script>
  // 构造函数
  function Axios(config) {
    // 初始化
    this.defaults = config; // 创建 default 默认属性
    this.intercepters = {
      request: {},
      response: {}
    };
  }
  // 原型添加相关方法
  Axios.prototype.request = function(config) {
    console.log("发送 AJAX 请求, 请求的类型为 " + config.method);
  };
  
  Axios.prototype.get = function(config) {
    return this.request({method: 'GET'});
  };
  
  Axios.prototype.post = function(config) {
    return this.request({method: 'POST'});
  };
  
  // 声明函数
  function createInstance(config) {
    // 实例化一个对象
    // 此时可以 context.get() context.post(), 但是不能当作函数使用 context()
    let context = new Axios(config); 
    // 创建请求函数
    // 此时, instance 是一个函数 并且可以 instance({})  但是不能使用方法 instance.get X
    let instance = Axios.prototype.request.bind(context);

    // 将Axios.prototype 对象中的方法添加到instance函数对象中
    Object.keys(Axios.prototype).forEach(key => {
      instance[key] = Axios.prototype[key].bind(context); // this.default  this.interceptors
    });
    // 为 instance 函数对象添加属性 default 与 interceptors
    Object.keys(context).forEach(key => {
      instance[key] = context[key];
    });
    return instance;
  }

  let axios = createInstance();
  // 发送请求
  axios.get({});
  axios({method: 'POST'});
</script>
```

#### 2. 模拟axios发送请求

```js
<script>
  // axios 发送请求
  // 1. 声明构造函数
  function Axios(config) {
    this.config = config;
  }
  Axios.prototype.request = function(config) {
    // 发送请求
    // 创建一个 promise 对象
    let promise = Promise.resolve(config);
    // 声明一个数组
    let chains = [dispatchRequest, undefined]; // undefined 占位
    // 调用 then 方法指定回调
    let result = promise.then(chains[0], chains[1]);
    // 返回 promise 的结果
    return result;
  }
  // 2. dispatchRequest函数
  function dispatchRequest(config) {
    // 调用适配器发送请求
    return xhrAdapter(config)
    .then(response => {
      // 响应的结果进行转换处理
      return response;
    }, error => {
      throw error;
    });
  }
  // 3. adapter 适配器
  function xhrAdapter(config) {
    console.log('xhrAdapter 函数执行');
    return new Promise((resolve, reject) => {
      // 发送AJAX请求
      let xhr = new XMLHttpRequest();
      // 初始化
      xhr.open(config.method, config.url);
      // 发送
      xhr.send();
      // 绑定事件
      xhr.onreadystatechange = function() {
        if (xhr.readyState === 4) {
          // 判断成功的条件
          if (xhr.status >= 200 && xhr.status < 300) {
            // 成功的状态
            resolve({
              // 配置对象
              config: config,
              // 响应体
              data: xhr.response,
              // 响应头
              headers: xhr.getAllResponseHeaders(),
              // xhr 请求对象
              request: xhr,
              // 响应状态码
              status: xhr.status,
              // 响应状态字符串
              statusText: xhr.statusText
            });
          } else {
            // 失败的状态
            reject(new Error("请求失败的状态码为" + xhr.status));
          }
        }
      }
    });
  }
  // 4. 创建 axios 函数
  let axios = Axios.prototype.request.bind(null);
  axios({
    method: 'GET',
    url: 'http://localhost:3000/posts',
  }).then(response => {
    console.log(response);
  });
</script>
```

#### 3. 模拟拦截器

1. `array.shift()`该方法用于把数组的第一个元素删除, 并返回第一个元素的值
2. 思路为先将拦截器的响应回调与请求回调都压入一个数组中,  之后再遍历
3. `promise = promise.then(chain.shift(), chains.shift());` 通过循环使用promise的then链条得到最终的结果,  等式前面的promise将被最终的结果覆盖

```js
<script>
  // 构造函数
  function Axios(config) {
    this.config = config;
    this.interceptors = {
      request: new InterceptorManager(),
      response: new InterceptorManager(),
    }
  }
  // 发送请求   难点与重点
  Axios.prototype.request = function(config) {
    // 创建一个 promise 对象
    let promise = Promise.resolve(config);
    // 创建一个数组
    const chains = [dispatchRequest, undefined];
    // 处理拦截器
    // 请求拦截器 将请求拦截器的回调 压入到 chains 的前面 request.handles = []
    this.interceptors.request.handlers.forEach(item => {
      chains.unshift(item.fulfilled, item.rejected);
    });
    //响应拦截器
    this.interceptors.response.handlers.forEach(item => {
      chains.push(item.fulfilled, item.rejected);
    });
    // 遍历
    while (chains.length) {
      promise = promise.then(chains.shift(), chains.shift());
    }
    return promise;
  }

  // 发送请求
  function dispatchRequest(config) {
    // 返回一个promise 对象
    return new Promise((resolve, reject) => {
      resolve({
        status: 200,
        statusText: 'OK'
      });
    });
  }

  // 创建实例
  let context = new Axios({});
  // 创建axios函数
  let axios = Axios.prototype.request.bind(context);
  // 将context 属性 config interceptors 添加至 axios 函数对象身上
  Object.keys(context).forEach(key => {
    axios[key] = context[key];
  });

  // 拦截器管理器构造函数
  function InterceptorManager() {
    this.handlers = [];
  }
  InterceptorManager.prototype.use = function(fulfilled, rejected){
      this.handlers.push({
          fulfilled,
          rejected
      })
  }

  // 设置请求拦截器  config 配置对象
  axios.interceptors.request.use(function one(config) {
      console.log('请求拦截器 成功 - 1号');
      return config;
  }, function one(error) {
      console.log('请求拦截器 失败 - 1号');
      return Promise.reject(error);
  });

  axios.interceptors.request.use(function two(config) {
      console.log('请求拦截器 成功 - 2号');
      return config;
  }, function two(error) {
      console.log('请求拦截器 失败 - 2号');
      return Promise.reject(error);
  });

  // 设置响应拦截器
  axios.interceptors.response.use(function (response) {
      console.log('响应拦截器 成功 1号');
      return response;
  }, function (error) {
      console.log('响应拦截器 失败 1号')
      return Promise.reject(error);
  });

  axios.interceptors.response.use(function (response) {
      console.log('响应拦截器 成功 2号')
      return response;
  }, function (error) {
      console.log('响应拦截器 失败 2号')
      return Promise.reject(error);
  });

  //发送请求
  axios({
      method: 'GET',
      url: 'http://localhost:3000/posts'
  }).then(response => {
      console.log(response);
  });
</script>
```

#### 4. 模拟请求取消功能

```js
<script>
  // 构造函数
  function Axios(config) {
    this.config = config;
  }
  // 原型 request 方法
  Axios.prototype.request = function(config) {
    return dispatchRequest(config);
  }
  // dispatchRequest 函数
  function dispatchRequest(config) {
    return xhrAdapter(config);
  }
  // xhrAdapter
  function xhrAdapter(config) {
    // 发送 AJAX 请求
    return new Promise((resolve, reject) => {
      // 实例化对象
      const xhr = new XMLHttpRequest();
      // 初始化
      xhr.open(config.method, config.url);
      // 发送
      xhr.send();
      // 处理结果
      xhr.onreadystatechange = function() {
        if (xhr.readyState === 4) {
          // 判断结果
          if (xhr.status >= 200 && xhr.status < 300) {
            // 设置为成功的状态
            resolve({
              status: xhr.status,
              statusText: xhr.statsuText
            });
          } else {
            reject(new Error('请求失败'));
          }
        }
      }
      // 关于取消请求的处理
      if (config.cancelToken) {
        // 对 cancelToken 对象身上的 promise 对象指定成功的回调
        config.cancelToken.promise.then(value => {
          xhr.abort();
          // 将整体结果设置为失败
          reject(new Error('请求已经被取消'));
        });
      }
    })
  }

  // 创建axios函数
  const context = new Axios({});
  const axios = Axios.prototype.request.bind(context);

  // CancelToken 构造函数
  function CancelToken(executor) {
    // 声明一个变量
    let resolvePromise;
    // 为实例对象添加属性
    this.promise = new Promise((resolve) => {
      // 将resolve赋值给 resolvePromise
      resolvePromise = resolve;
    })
    // 调用 executor函数
    executor(function() {
      // 执行 resolvePromise() 函数
      resolvePromise();
    });
  }

  //获取按钮 以上为模拟实现的代码
  const btns = document.querySelectorAll('button');
  //2.声明全局变量
  let cancel = null;
  //发送请求
  btns[0].onclick = function(){
    //检测上一次的请求是否已经完成
    if(cancel !== null){
      //取消上一次的请求
      cancel();
    }

    //创建 cancelToken 的值
    let cancelToken = new CancelToken(function(c){
      cancel = c;
    });

    axios({
      method: 'GET',
      url: 'http://localhost:3000/posts',
      //1. 添加配置对象的属性
      cancelToken: cancelToken
    }).then(response => {
      console.log(response);
      //将 cancel 的值初始化
      cancel = null;
    })
  }

  //绑定第二个事件取消请求
  btns[1].onclick = function(){
    cancel();
  }
</script>
```

