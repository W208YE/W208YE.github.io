# 一、Ajax简介

AJAX全称为Asynchronous JavaScript And XML，即异步的JS和XML。

通过AJAX可以在浏览器中向服务器发送异步请求，最大的优势：**无刷新获取数据**。

AJAX不是新的编程语言，而是一种将现有的标准组合在一起使用的新方式。

## 1.XML简介

- XML可扩展标记语言

- XML被设计用来传输和存储数据
- XML和HTML类似，不同的是HTML中都是预定义标签，而XML中没有预定义标签，全都是自定义标签，用来表示一些数据

```javascript
// XML表示
<student>
	<name>孙悟空</name>
	<age>18</age>
	<gender>男</gender>
</student>
```

现在XML已经被JSON替代了，相比XML，JSON更加简洁，数据转换更加容易，灵活度高。

```javascript
// JSON表示
{"name":"孙悟空","age":18,"gender":"男"}
```

## 2.Ajax特点

- **优点**

​	1.可以无需刷新页面而与服务器进行通信

​	2.允许根据用户事件来更新部分页面内容

- 缺点

​	1.没有浏览历史，不能回退

​	2.存在跨域问题（同源）（默认不可以跨域）

​	3.SEO不友好（搜索引擎优化）（爬虫爬不到）

## 3.HTTP简介

### 3.1 HTTP

HTTP（hypertext transport protocol）`超文本传输协议`，协议详细规定了浏览器和万维网服务器之间互相通信的规则。

### 3.2 请求报文

重点是格式和参数

如果是GET请求，则请求体为空；POST请求，请求体可以为空，也可以不为空。

```javascript
行      POST(请求类型)   /s?ie=utf-8(URL)  HTTP/1.1(HTTP版本)
头      Host: atguihu.com
        Cookie: name=guigu
        Content-type: application/x-www-form-urlencoded
        User-Agent: chrom 83
空行
体      username=admin&&password=admin
```

### 3.3 响应报文

```javascript
行      HTTP/1.1(HTTP版本)  200(响应状态码)  OK(响应状态字符串)
头      Content-Type: text/html;charset=utf-8
        Content-length: 2048
        Content-encoding: gzip

空行
体      <html>
            <head>
            </head>
            <body>
                <h1>尚硅谷</h1>
            </body>
        </html>
200 成功
404 找不到
403 被禁止
401 未授权
500 内部错误
```

### 3.4 Chrome网络控制台查看通信报文

1、Response Headers：响应头和响应行位置

2、Request Hearders：请求头和请求行位置

3、Form Data：请求体

4、Query String Parameters：请求字符串参数（对请求的URL进行的解析）

5、Response：响应体，通常返回的是html

## 4. express基本使用

```javascript
// 1.引入express
const express = require("express");

// 2.创建应用对象
const app = express();

// 3.创建路由规则
// request 是对请求报文的封装
// response 是对响应报文的封装
app.get("/", (request, response)=>{
    // 设置响应
    response.send("HELLO EXPRESS");
});

// 4.监听端口启动服务
app.listen(8000, ()=>{
    console.log("服务已经启动，8000端口监听中....");
});
```

启动服务代码：`node express基本使用.js启动`

# 二、原生AJAX 

1. XMLHttpRequest,AJAX的所有操作都是通过该对象进行的.
2. 当你前端想设置自定义的请求头时,需要设置后端响应头, 代表允许跨域.

```javascript
app.all('/server', (request, response) => {
	response.setHeader('Access-Control-Allow-Origin', '*');
	response.setHeader('Access-Control-Allow-Headers, '*');
})
```

## 2.1 GET请求

**GET.html**

```javascript
<script>
    // 获取button元素
    const btn = document.getElementsByTagName("button")[0];
    const result = document.getElementById("result");
    // 绑定事件
    btn.onclick = function() {
        // 1.创建对象
        const xhr = new XMLHttpRequest();
        // 2.初始化 设置请求方法和url
        /**
         * AJAX设置请求参数: ? 分割url和变量名,变量和变量之间用&分割
         * xhr.open("GET", "http://127.0.0.1:8000/server?a=100&b=200&c=300");
         * 
        */
        xhr.open("GET", "http://127.0.0.1:8000/server");

        // 3.发送
        xhr.send();
        // 4.事件绑定 处理服务端返回的结果
        // on when 当...时候
        /* readyState 是 xhr 对象中的属性，表示状态 0 1 2 3 4
            0表示未初始化，最开始属性值就是0，
            1表示open()已经调用完毕，
            2表示send()已经调用完毕，
            3表示服务端返回部分结果，
            4表示服务端返回全部结果。
        */
        // change 改变的时候触发

        // 触发4次：0-1,1-2,2-3,3-4
        xhr.onreadystatechange = function() {
            // 判断（服务端返回了所有的结果）
            if (xhr.readyState === 4) {
                // 判断响应状态码 200 404 403 401 500
                // 2xx都表示成功
                if (xhr.status >= 200 && xhr.status < 300) {
                    // 处理结果 行 头 空行 体
                    // 1. 响应行
                    console.log(xhr.status); // 状态码
                    console.log(xhr.statusText); // 状态字符串
                    console.log(xhr.getAllResponseHeaders()); // 所有响应头
                    console.log(xhr.response); // 响应体
                    // 设置 result 的文本
                    result.innerHTML = xhr.response;
                } else {

                }
            }
        }
    }
</script>
```

**server.js**

```javascript
// 发送get请求
app.get("/server", (request, response)=>{
    // 设置响应头   设置允许跨域
    response.setHeader("Access-Control-Allow-Origin", "*");
    // 设置响应体
    response.send("HELLO AJAX");
});
```

## 2.2 Post请求

**POST.html**

```javascript
<script>
    // 获取元素对象
    const result = document.getElementById("result");
    // 绑定事件（鼠标悬浮事件 mouseover）
    result.addEventListener("mouseover", function(){
        // 1. 创建对象
        const xhr = new XMLHttpRequest();
        // 2. 初始化 设置类型与URL
        xhr.open("POST", "http://127.0.0.1:8000/server");

        /**
         * 设置请求头(要在open()方法之后，send()方法之前进行设置)
         * 参数1：请求头的名字
         * 参数2：请求头的值
        */
        xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
        // 自定义请求头浏览器会报错可以在server.js中添加响应头
        xhr.setRequestHeader("name", "aiguigu");

        // 3. 发送
        // xhr.send();
        /**
         *  设置请求体(格式非常灵活)，只要服务端能够解析
         *  xhr.send("a:100&b:200&c:300");
         *  xhr.send('a=100&b=200&c=300'); // 此格式运用居多
        */
        xhr.send('a=100&b=200&c=300');

        // 4. 事件绑定
        xhr.onreadystatechange = function() {
            // 判断
            if (xhr.readyState === 4) {
                if (xhr.status >= 200 && xhr.status < 300) {
                    // 处理服务端返回的结果
                    result.innerHTML = xhr.response;
                }
            }
        }
    });
</script>

```

**server.js**

`app.post改为app.all原因`:

浏览器会发送一个OPTIONS请求, 会有一个全新的校验来检验我们发送的头信息可不可用,

而这个请求没有得到对应的结果, 所以依旧会报错, 改为.all之后可以接受任意的请求.

```javascript
// 发送post请求
// 注意：修改后要使自定义请求头不再报错要重启服务器
app.all("/server", (request, response)=>{
    // 设置响应头   设置允许跨域
    response.setHeader("Access-Control-Allow-Origin", "*");
    // 所有类型头信息都可以接受,用于自定义响应头
    response.setHeader("Access-Control-Allow-Headers","*");
    // 设置响应体
    response.send("HELLO AJAX POST");
});
```

**HTTP请求方式中的8种请求方式:**

| 方法    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| GET     | 请求指定的页面内容，并返回实体主体                           |
| HEAD    | 类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头 |
| POST    | 向指定资源提交数据进行处理请求(例如提交表单或者上传文件)。数据包含在请求体中。POST请求可能会导致新的资源的建立或者已有资源的修改。 |
| PUT     | 从客户端向服务器传送的数据取代指定的文档的内容               |
| DELETE  | 请求服务器删除指定的页面                                     |
| CONNECT | HTTP1.1协议中预留给能够将连接方式改为管道方式的代理服务器    |
| OPTIONS | 允许客户端查看服务器的性能                                   |
| TRACE   | 回显服务器收到的请求，主要用于测试或诊断                     |

## 2.3 响应JSON数据

**JSON.html**

```javascript
<script>
    const result = document.getElementById("result");
    // 绑定键盘按下事件
    window.onkeydown = function() {
        // 1.发送请求
        const xhr = new XMLHttpRequest();
        // 设置响应体数据类型(自动转换)
        xhr.responseType = "json";
        // 2.初始化
        xhr.open("GET", "http://127.0.0.1:8000/json-server");
        // 3.发送
        xhr.send();
        // 4.事件绑定
        xhr.onreadystatechange = function() {
            if (xhr.readyState === 4) {
                if (xhr.status >= 200 && xhr.status < 300) {
                    // console.log(xhr.response);
                    // result.innerHTML = xhr.response;
                    // 1.手动对数据转化
                    // let data = JSON.parse(xhr.response);
                    // console.log(data);
                    // 2.自动转化
                    console.log(xhr.response);
                    result.innerHTML = xhr.response.name;
                    // result.innerHTML = data.name;
                }
            }
        }
    }
</script>
```

**server.js**

```javascript
app.all("/json-server", (request, response)=>{
    // 设置响应头   设置允许跨域
    response.setHeader("Access-Control-Allow-Origin", "*");
    // 所有类型头信息都可以接受
    response.setHeader("Access-Control-Allow-Headers","*"); 
    // 响应一个数据
    const data = {
        name: "aiguigu"
    };
    // 对对象进行字符串转换
    let str = JSON.stringify(data);
    // 设置响应体
    // send()只能接收字符串和buffer
    response.send(str);
});
```

## 2.4 解决IE缓存问题

**IE缓存问题**：在一些浏览器中(IE),由于`缓存机制`的存在，ajax 只会发送的第一次请求，剩余多次请求不会再发送给浏览器而是直接加载缓存中的数据, 对于时效性比较强的场景, 本地AJAX的缓存就会影响到我们的结果，不能正常显示。

**解决方式**：浏览器的缓存是根据 url地址来记录的，所以我们只需要修改 url 地址, 每次传递不同的参数即可, `xhr.open("get","/testAJAX?t="+Date.now());`

```javascript
const btn = document.getElementsByTagName("button")[0];
const result = document.querySelector("#result");        btn.addEventListener("click", function() {
	const xhr = new XMLHttpRequest();
    // 解决IE缓存问题，每次传递不同的参数即可
    xhr.open("GET", "http://127.0.0.1:8000/ie?t="+Date.now());
    xhr.send();
    xhr.onreadystatechange = function() {
    	if (xhr.readyState === 4) {
        	if (xhr.status >= 200 && xhr.status < 300) {
            	result.innerHTML = xhr.response;
            }
        }
  	}
})
```

**server.js**

```javascript
// 针对IE缓存问题
app.get("/ie", (request, response)=>{
    // 设置响应头 允许跨域
    response.setHeader("Access-Control-Allow-Origin", "*");
    // 设置响应体
    response.send("HELLO IE1");
});
```

## 2.5 请求超时与网络异常

当请求事件过长，或者无网络时，进行的响应处理

```javascript
<script>
    const btn = document.getElementsByTagName("button")[0];
    const result = document.querySelector("#result");

    btn.addEventListener("click", function() {
        // 1.发送请求
        const xhr = new XMLHttpRequest();
        // 超时设置2s设置
        xhr.timeout = 2000;
        // 超时回调
        xhr.ontimeout = function() {
            alert("网络异常，请稍后重试！");      
        }
        // 网络异常回调
        xhr.onerror = function() {
            alert("你的网络似乎出了一些问题！");
        }
        // 2.初始化
        xhr.open("GET", "http://127.0.0.1:8000/delay")
        // 3.发送
        xhr.send();
        // 4.事件绑定
        xhr.onreadystatechange = function() {
            if (xhr.readyState === 4) {
                if (xhr.status >= 200 && xhr.status < 300) {
                    result.innerHTML = xhr.response;
                }
            }
        }
    });
</script>
```

**server.js**

```javascript
// 延时响应
app.get("/delay", (request, response)=>{
    // 设置响应头 允许跨域
    response.setHeader("Access-Control-Allow-Origin", "*");
    // 定时器
    setTimeout(() => {
        // 设置响应体
        response.send("延时响应");
    }, 3000) ;
});
```

## 2.6 取消请求

```javascript
<script>
    // 获取元素对象
    /**
     * 1.querySelectorAll 是找出所有匹配的节点后，返回对应的元素节点数组
     * 2.querySelector 是找到一个后立刻返回找到的第一个节点对象，如果没有则返回null
     * 3.返回的结果是静态的，之后对document结构的改变不会影响到之前获取到的结果
     * 
    */
    const btns = document.querySelectorAll("button");
    let x = null;
    btns[0].onclick = function() {
        x = new XMLHttpRequest();
        x.open("GET", "http://127.0.0.1:8000/delay");
        x.send();
    }
    // abort() 取消请求
    btns[1].onclick = function() {
        x.abort();
    }
</script>
```

## 2.7 重复请求问题

```javascript
<script>
    const btns = document.querySelectorAll("button");
    let x = null;
    // 标识变量：是否正在发送AJAX请求
    let isSending = false;

    btns[0].onclick = function() {
        // 判断标识变量，如果正在发送，则取消发送，创建一个新的请求
        if (isSending) x.abort();
        x = new XMLHttpRequest();
        // 修改标识变量
        isSending = true;
        x.open("GET", "http://127.0.0.1:8000/delay");
        x.send();
        x.onreadystatechange = function() {
            if (x.readyState === 4) {
                // 修改标识变量
                isSending = false;
            }
        }
    }

    // // abort
    // btns[1].onclick = function() {
    //     x.abort();
    // }
</script>
```

# 三、常见三种AJAX请求方式

## 3.1 jQuery发送AJAX请求

**引入jQuery**

```javascript
<!-- 
    crossorigin 跨语言请求设置 anonymous：匿名的
    加上属性之后，向src资源发送请求时，不会携带当前域名下的cookie
-->
<script crossorigin="anonymous" src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
```

**server.js**

```javascript
app.all("/jquery-server", (request, response)=>{
    // 设置响应头 允许跨域
    response.setHeader("Access-Control-Allow-Origin", "*");
    response.setHeader("Access-Control-Allow-Headers","*"); 
    // 设置响应体
    const data = {name: "尚硅谷"};
    // response.send("HELLO jQuery AJAX");
    response.send(JSON.stringify(data));
});
```

### 3.1.1 GET请求

```javascript
// 绑定事件
$("button").eq(0).click(function() {
	// 发送请求
    /*
    	参数1：url
    	参数2：发送参数的键值（类型为对象）
    	参数3：回调函数（接收的参数为 响应体）
    	参数4：响应体类型
    */
    $.get("http://127.0.0.1:8000/jquery-server", {a: 100, b:200}， function(data) {
		console.log(data); // 打印json对象
	}, 'json'); // 第四个参数：响应体类型
});
```

### 3.1.2 POST请求

```javascript
$('button').eq(1).click(function() {
    $.post('http://127:0.0.1:8000/jquery-server', {a:100, b:200}, function(data) {
        console.log(data); // 打印结果字符串
    });
});
```

### 3.1.3 jQuery通用型方法AJAX请求

```javascript
$('button').eq(2).click(function() {
	// 参数为一个对象
    $.ajax({
        // url
        // url改为delay可以触发超时事件
        // url: "http://127.0.0.1:8000/delay",
        url: "http://127.0.0.1:8000/jquery-server",
        // 参数
        data: {a:100, b:200},
        // 请求类型
        type: 'GET',
        // 响应体结果类型
        dataType: 'json',
        // 成功的回调
        success: function(data) {
			console.log(data);
        },
        // 超时事件
        timeout: 2000,
        // 失败的回调
        error: function(data) {
            console.log("出错了");
        },
        // 头信息
        headers: {
            c: 300,
            d: 400
        }
    });
});
```

## 3.2 axios发送AJAX请求

**引入axios**

```javascript
<script crossorigin="anonymous" src="https://cdn.bootcdn.net/ajax/libs/axios/0.26.1/axios.min.js"></script>
```

**server.js**

```javascript
// axios服务
app.all("/axios-server", (request, response)=>{
    // 设置响应头 允许跨域
    response.setHeader("Access-Control-Allow-Origin", "*");
    response.setHeader("Access-Control-Allow-Headers", "*"); 
    // 设置响应体
    const data = {name: "尚硅谷"};
    // response.send("HELLO jQuery AJAX");
    response.send(JSON.stringify(data));
});
```

### 3.2.1 GET请求

```javascript
const btns = document.querySelectorAll("button");

// 配置baseURL
axios.defaults.baseURL = "http://127.0.0.1:8000";

btns[0].onclick = function () {
    // GET请求
    axios.get("/axios-server", {
        // url参数
        params: {
            id: 100,
            vip: 7,
        },
        // 请求头信息
        headers: {
            name: "aiguigu",
            age: 20
        }
    }).then(value => {
        console.log(value);
    });
}
```

### 3.2.2 POST请求

```javascript
btns[1].onclick = function () {
    // POST请求
    /**
     * 注意：如果第二个参数是一个对象，则会直接默认为请求体内容
     * 参数1：url
     * 参数2：请求体
     * 参数3：其他配置
     * axios#post(url[, data[, config]])
    */
    axios.post("/axios-server", {
        // 请求体
        username: "admin",
        password: "admin"
    }, {
        // url
        params: {
            id: 200,
            vip: 9
        },
        // 请求头参数
        headers: {
            height: 180,
            weight: 180,
        }
    });
}
```

### 3.2.3 axios通用方法发送AJAX请求

```javascript
btns[2].onclick = function() {
    axios({
        // 请求方法
        method: "POST",
        // url
        url: "/axios-server",
        // url参数
        params: {
            vip: 10,
            level: 30
        },
        // 头信息
        headers: {
            a:100,
            b:200
        },
        // 请求体参数
        data: {
            username: "admin",
            password: "admin"
        }
    }).then(response=>{
        // 响应状态码
        console.log(response.status);
        // 响应状态字符串
        console.log(response.statusText);
        // 响应头信息
        console.log(response.headers);
        // 响应体
        console.log(response.data);
    })
}
```

## 3.3 fetch发送AJAX请求

**server.js**

```javascript
app.all("/fetch-server", (request, response)=>{
    // 设置响应头 允许跨域
    response.setHeader("Access-Control-Allow-Origin", "*");
    response.setHeader("Access-Control-Allow-Headers", "*"); 
    // 设置响应体
    const data = {name: "尚硅谷"};
    // response.send("HELLO jQuery AJAX");
    response.send(JSON.stringify(data));
});
```

```javascript
<script>
    const btn = document.querySelector("button");

    btn.onclick = function() {
        fetch("http://127.0.0.1:8000/fetch-server?vip=10", {
            // 请求方法
            method: "POST",
            // 请求头
            headers: {
                name: "aiguigu"
            },
            // 请求体
            body: "username=admin&password=admin"
        }).then(response=>{
            // return response.text();
            return response.json();
        }).then(response=>{
            console.log(response);
        });
    }
</script>
```

# 四、跨域与解决

## 4.1 同源策略与跨域

1. 什么是同源策略跨域？

   - 同源策略是Netscape提出的一个著名的安全策略，现在所有支持JavaScript的浏览器都会使用这个策略。同源策略是浏览器最核心也最基本的安全功能，如果缺少同源策略，浏览器的正常功能可能受到影响。可以说web是构建在同源策略的基础之上的，浏览器只是针对同源策略的一种实现。

   - 同源： 协议、域名、端口号 必须完全相同。 `违背同源策略就是跨域`。

2. 什么是跨域？

   - 一个网页向另一个不同域名/不同协议/不同端口的网页请求资源，这就是跨域。
   - 跨域原因产生：在当前域名请求网站中，默认不允许通过ajax请求发送其他域名。

3. 为什么浏览器要使用同源策略？

   - 为了保证用户的信息安全，防止恶意网站窃取数据，如果网页之间不满足同源要求，将不能
     1. 共享Cookie、LocalStorage、IndexDB
     2. 获取DOM
     3. AJAX请求不能发送

4. 跨域的五个解决方式：

   1. 前端使用jsonp （不推荐使用）
   2. 后台Http请求转发
   3. 后台配置同源Cors （推荐）
   4. 使用SpringCloud网关
   5. 使用nginx做转发 (推荐)

**同源策略案例**

**index.html**

```javascript
<script>
    const btn = document.querySelector('button');
    btn.onclick = function() {
        const x = new XMLHttpRequest();
        // 这里因为满足同源策略，所有 url 可以简写
        x.open('GET', '/data');
        x.send();
        x.onreadystatechange = function() {
            if (x.readyState === 4) {
                if (x.status >= 200 && x.status < 300) {
                    console.log(x.response);
                }
            }
        }
    }
</script>
```

**server.js**

```javascript
const express = require('express');
const app = express();

app.get('/home', (request, response) => {
    // 响应一个页面
    response.sendFile(__dirname + '/index.html');
});
app.get('/data', (request, response) => {
    response.send('用户数据');
});
app.listen(9000, () => {
    console.log("服务已经启动，9000端口监听中...");
})
```

## 4.2 JSONP

1. JSONP是什么？

   JSONP（JSON with Padding），是一个非官方的跨域解决方案，凭借程序员的聪明才智开发出来，只支持get请求。

2. JSONP怎么工作的？

   在网页中有一些标签天生具有跨域能力，比如：img，link，iframe，script

   JSONP就是利用script标签的跨域能力来发送请求的。

### 4.2.1 script标签跨域原理

通过本地默认浏览器打开html文件，此时为file协议，与script标签中的http不同，而此时控制台中的结果依旧可以呈现，可以验证，`<script>`标签是支持跨域的。

`file:///D:/%E6%A1%8C%E9%9D%A2/%E5%89%8D%E7%AB%AF/Ajax/5-%E8%B7%A8%E5%9F%9F/2-JSONP/%E5%8E%9F%E7%90%86.html`

**原理.html**

```javascript
<script>
    // 处理数据
    function handle(data) {
        // 获取result元素
        const result = document.getElementById('result');
        result.innerHTML = data.name;
    }
</script>
<script src="http://127.0.0.1:5500/5-%E8%B7%A8%E5%9F%9F/2-JSONP/js/app.js"></script>
```

**app.js**

```javascript
const data = {
    name: '尚硅谷atguigu'
};

handle(data);
```

### 4.2.2 JSONP返回结果

#### 4.2.2.1 返回一段字符串

**原理.html**

```javascript
<script>
    // 处理数据
    function handle(data) {
        // 获取result元素
        const result = document.getElementById('result');
        result.innerHTML = data.name;
    }
</script>
<script src="http://127.0.0.1:8000/jsonp-server"></script>
<!-- <script src="http://127.0.0.1:5500/5-%E8%B7%A8%E5%9F%9F/2-JSONP/js/app.js"></script> -->
```

**server.js**

```javascript
// jsonp服务
app.all('/jsonp-server', (request, response) => {
	// 返回一段字符串
    response.send('hello jsonp-server');
});
```

`Uncaught SyntaxError: Unexpected identifier`：意料之外的标识符

![image-20220501145401393](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220501145401393.png)

**错误原因**：返回结果应该是一段js代码，不然js引擎无法解析。

#### 4.2.2.2 返回一段js代码

**server.js**

```javascript
app.all('/jsonp-server', (request, response) => {
    // 返回一段js代码
    response.send('console.log("hello jsonp-server")');
});
```

![image-20220501150831230](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220501150831230.png)

#### 4.2.2.3 返回函数调用

```javascript
app.all('/jsonp-server', (request, response) => {
    // response.send('console.log("hello jsonp-server")');
    const data = {
        name: '尚硅谷atguigu'
    };
    // 将数据转化为字符串
    let str = JSON.stringify(data);
    // 返回结果，end()方法不会加特殊响应头
    response.end(`handle(${str})`);
});
```

![image-20220501150924999](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220501150924999.png)

## 4.3 JSONP实践

**需求**：输入一个用户名，当鼠标失去焦点时，将边框变红，且在下方显示“用户名已存在”文字。

**实践.html**

```javascript
<body>
    用户名: <input type="text" id="username">
    <p></p>
    <script>
        // 获取 input 元素
        const input = document.querySelector('input');
        const p = document.querySelector('p');

        // 声明 handle 函数
        function handle(data) {
            input.style.border = 'solid 1px #f00';
            // 修改p标签的提示文本
            p.innerHTML = data.msg;
        } 

        // 绑定丧失焦点事件
        input.onblur = function() {
            // 获取用户的输入值
            let username = this.value;
            // 向服务端发送请求 检测用户名是否存在
            // 1.创建 script标签
            const script = document.createElement('script') ;
            // 2.设置标签的src属性
            script.src = 'http://127.0.0.1:8000/check-username';
            // 3.将 script 插入到文档中
            document.body.appendChild(script);
        }
    </script>
</body>
```

**server.js**

```javascript
// 检测用户名是否存在
app.all('/check-username', (request, response) => {
    // response.send('console.log("hello jsonp-server")');
    const data = {
        exist: 1,
        msg: '用户名已经存在'
    };
    // 将数据转化为字符串
    let str = JSON.stringify(data);
    // 返回结果，end()方法不会加特殊响应头
    response.end(`handle(${str})`);
});
```

## 4.4 通过jQuery发送JSONP请求

**需求**：点击按钮向8000端口发送请求，返回结果在div中呈现。

**jQuery-jsonp.html**

```javascript
<button>点击发送 jsonp 请求</button>
<div id="result"></div>
<script>
    $('button').eq(0).click(function() {
        // 注意：使用jQuery发送JSONP请求时，url后面需要有参数callback=?（固定写法）
        $.getJSON('http://127.0.0.1:8000/jquery-jsonp-server?callback=?', function(data) {
            console.log(data);
            $('#result').html(`
                名称: ${data.name}<br>
                校区: ${data.city}
            `)
        })
    });
</script>
```

**server.js**

```javascript
// jQuery发送JSONP请求
app.all('/jquery-jsonp-server', (request, response) => {
    // response.send('console.log("hello jsonp-server")');
    const data = {
        name: '尚硅谷',
        city: ['北京', '上海', '深圳']
    };
    // 将数据转化为字符串
    let str = JSON.stringify(data);
    // 接收 callback 参数
    let cb = request.query.callback;
    // 返回结果，end()方法不会加特殊响应头
    response.end(`${cb}(${str})`);
});
```

## 4.5 CORS

 CORS（Cross-Origin Resource Sharing），跨域资源共享。CORS 是官方的跨域解决方 案，它的特点是不需要在客户端做任何特殊的操作，完全在服务器中进行处理，支持 get 和 post 请求。跨域资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些 源站通过浏览器有权限访问哪些资源。

CORS 是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应 以后就会对响应放行。

```javascript
app.all('/cors-server', (request, response) => {
    // 设置响应头
    // 响应首部中可以携带一个 Access-Control-Allow-Origin 字段
    response.setHeader("Access-Control-Allow-Origin", "*");
    // Access-Control-Allow-Headers 首部字段用于预检请求的响应。其指明了实际请求中允许携带的首部字
    response.setHeader("Access-Control-Allow-Headers", '*');
    // Access-Control-Allow-Methods 首部字段用于预检请求的响应。其指明了实际请求所允许使用的 HTTP
    response.setHeader("Access-Control-Allow-Method", '*');
    // response.setHeader("Access-Control-Allow-Origin", "http://127.0.0.1:5500");
    response.send('hello CORS');
});
```

```javascript
<script>
    const btn = document.querySelector('button');
    btn.onclick = function() {
        // 1.创建对象
        const x = new XMLHttpRequest();
        // 2.初始化设置
        x.open("GET", "http://127.0.0.1:8000/cors-server");
        // 3.发送
        x.send();
        // 4.绑定事情
        x.onreadystatechange = function() {
            if (x.readyState === 4) {
                if (x.status >= 200 && x.status < 300) {
                    // 输出响应体 
                    console.log(x.response);
                }
            }
        }
    }
</script>
```

**server.js**

```javascript
// CORS 服务
app.all('/cors-server', (request, response) => {
    // 设置响应头
    response.setHeader("Access-Control-Allow-Origin", "*");

    response.send('hello CORS');
});
```

