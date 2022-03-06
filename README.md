# AJAX 学习笔记
> 笔记主要基于尚硅谷《3小时Ajax入门到精通》的课程，同时会加入一些自己搜索的文档进行补充
>[课程B站连接](https://www.bilibili.com/video/BV1WC4y1b78y)

## Introduction
### 1. What is AJAX
> Asynchronous JavaScript and XML, while not a technology in itself, is a term coined in 2005 by Jesse James Garrett, that describes a "new" approach to using a number of existing technologies together, including HTML or XHTML, CSS, JavaScript, DOM, XML, XSLT, and most importantly the XMLHttpRequest object. **When these technologies are combined in the Ajax model, web applications are able to make quick, incremental updates to the user interface without reloading the entire browser page.** This makes the application faster and more responsive to user actions.
>
>Although X in Ajax stands for XML, JSON is preferred over XML nowadays because of its many advantages such as being a part of JavaScript, thus being lighter in size. Both JSON and XML are used for packaging information in the Ajax model.
>
>Ref: https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX
 
### 2. Case Examples

* 浏览器输入搜索词后下拉框的关键字提醒
* 注册页面检测如用户名字段是否可用，并即时作出反馈 
* 网站（如电商平台）的商品分类板块的二级分类，鼠标到一级分类后会显示二级分类（如鼠标不放到一级分类上去，就不会向服务器请求二级分类，即用则加载，不用则不加载,即懒加载）
* 列表类的内容（如新闻列表）在滚到或块滚到底部的时候，有会出现新内容（滚到或快滚到底部的时候，向服务端发出请求，得到结果做一个遍历，动态创作节点，把节点插入到列表里面，实现在不需要用户手动刷新即可显示更多内容的目的，。

### 3. Contents

#### Essentials
* Navive Ajax method
* jQuery 
* Fetch API
* axios

#### Extra
* HTTP

## 第1章：原生Ajax

### 1.1 Ajax简介
* `AJAX` 全称为 Asynchronous JavaScript And XML，就是异步的 `JavaScript` 和 `XML`。
* 通过 `AJAX` 可以在浏览器中向服务器发送异步请求，**最大的优势:无刷新获取数据**。 
* `AJAX` 不是新的编程语言，而是一种将现有的标准组合在一起使用的新方式。

### 1.2 XML
>XML (Extensible Markup Language) is a markup language similar to HTML, but without predefined tags to use. Instead, you define your own tags designed specifically for your needs. This is a powerful way to store data in a format that can be stored, searched, and shared. Most importantly, since the fundamental format of XML is standardized, if you share or transmit XML across systems or platforms, either locally or over the internet, the recipient can still parse the data due to the standardized XML syntax.

#### XML简介:
* `XML` 可扩展标记语言。
* `XML` 被设计用来传输和存储数据。
* `XML` 和 `HTML` 类似，不同的是 `HTML` 中都是预定义标签，而 `XML` 中没有预定义标签，
* 全都是自定义标签，用来表示一些数据。

#### 举例:
比如说有一个学生数据:
name = "孙悟空" ; age = 18 ; gender = "男" ;

1. 用`XML`表示：
```xml
<student>
    <name>孙悟空</name>
    <age>18</age>
    <gender>男</gender>
</student>
```
2. 用`JSON`表示（ `XML` 现在已经被 `JSON` 取代了。）
```json
{"name":"孙悟空","age":18,"gender":"男"}
```

### 1.3 Ajax的特点
#### 1.3.1 AJAX 的优点
1. 可以无需刷新页面而与服务器端进行通信。
2. 允许你根据用户事件来更新部分页面内容。
    *事件： 如鼠标事件、键盘输入事件、表单事件、文档事件
#### 1.3.2 AJAX 的缺点
1. 没有浏览历史，不能回退
2. 存在跨域问题(同源)
3. `SEO` 不友好 （Search Engine Optimization）


### 1.4 Ajax 的使用
#### 1.4.1 核心对象
XMLHttpRequest，AJAX 的所有操作都是通过该对象进行的。
#### 1.4.2 使用步骤
1. 创建 `XMLHttpRequest` 对象
```javascript
const xhr = new XMLHttpRequest();
```
2. 设置请求信息 
```javascript
xhr.open(method, url); //可以设置请求头，一般不设置
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
```

3. 发送请求
```javascript
xhr.send(body) //get 请求不传 body 参数，只有 post 请求使用
```

4. 接收响应

```javascript
//xhr.responseXML 接收 xml 格式的响应数据
//xhr.responseText 接收文本格式的响应数据
xhr.onreadystatechange = function (){ if(xhr.readyState == 4 && xhr.status == 200){
const text = xhr.responseText;
console.log(text); }
}
```

#### 1.4.3 解决 IE 缓存问题
1. 问题:
在一些浏览器中(IE),由于缓存机制的存在，ajax 只会发送的第一次请求，剩 余多次请求不会在发送给浏览器而是直接加载缓存中的数据。
2. 解决方式:
浏览器的缓存是根据 url 地址来记录的，所以我们只需要修改 url 地址 即可避免缓存问题
```javascript
xhr.open("get","/testAJAX?t="+Date.now());
```

#### 1.4.4 AJAX 请求状态

`xhr.readyState` 可以用来查看请求当前的状态 
Ref: https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/readyState

`0 `: 表示 `XMLHttpRequest` 实例已经生成，但是 `open()` 方法还没有被调用。
`1 `: 表示 `send()` 方法还没有被调用，仍然可以使用 `setRequestHeader()`，设定 HTTP请求的头信息。
`2 `: 表示 send()方法已经执行，并且头信息和状态码已经收到。
`3 `: 表示正在接收服务器传来的 body 部分的数据。
`4 `: 表示服务器数据已经完全接收，或者本次接收已经失败了

## 第 2 章: jQuery 中的 AJAX

### 2.1 get 请求
```javascript
$.get(url, [data], [callback], [type])
```
`url: `请求的 URL 地址。
`data: `请求携带的参数。
`callback: `载入成功时回调函数。 
`type: `设置返回内容格式，xml, html, script, json, text, _default。

### 2.2 post 请求
```javascript
$.post(url, [data], [callback], [type])
```
`url: `请求的 URL 地址。
`data: `请求携带的参数。
`callback: `载入成功时回调函数。 
`type: `设置返回内容格式，xml, html, script, json, text, _default。

## 第3章:跨域

### 3.1 同源策略
同源策略(Same-Origin Policy)最早由 Netscape 公司提出，是浏览器的一种安全策略。

1. 同源: 协议、域名、端口号 必须完全相同
2. 违背同源策略就是跨域。

### 3.2 如何解决跨域
#### 3.2.1 JSONP
1. `JSONP` 是什么
JSONP(JSON with Padding)，是一个非官方的跨域解决方案，纯粹凭借程序员的聪明
才智开发出来，只支持 get 请求。
2. `JSONP` 怎么工作的?
在网页有一些标签天生具有跨域能力，比如:img link iframe script。
JSONP 就是利用 script 标签的跨域能力来发送请求的。

3. JSONP 的使用

    3.1) 动态的创建一个 script 标签
    ```javascript
    const script = document.createElement("script");
    ```

    3.2) 设置 script 的 src，设置回调函数
    ```
    script.src = "http://localhost:3000/testAJAX?callback=abc"; 
    function abc(data) {
        alert(data.name); 
    };
    ```
    3.3）将 script 添加到 body 中 
    ```javascript
    document.body.appendChild(script);
    ```
    3.4) 服务器中路由的处理
    ```javascript
    router.get("/testAJAX" , function (req , res) {
        console.log("收到请求");
        const callback = req.query.callback; 
        const obj = {
            name:"孙悟空",
            age:18 }
        res.send(callback+"("+JSON.stringify(obj)+")"); });
    ```
#### 3.2.2 CORS
Ref: https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS

1. `CORS` 是什么?
CORS(Cross-Origin Resource Sharing)，跨域资源共享。`CORS` 是官方的跨域解决方案，它的特点是不需要在客户端做任何特殊的操作，完全在服务器中进行处理，支持 `get` 和 `post` 请求。跨域资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些 源站通过浏览器有权限访问哪些资源
2. `CORS` 怎么工作的?
CORS 是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应 以后就会对响应放行。
3. `CORS` 的使用
  主要是服务器端的设置:
    ```javascript
    router.get("/testAJAX" , function (req , res) {
        //通过 res 来设置响应头，来允许跨域请求 
        //res.set("Access-Control-Allow-Origin","http://127.0.0.1:3000");
        res.set("Access-Control-Allow-Origin","*");
        res.send("testAJAX 返回的响应");
        });
    ```

## Axios
### What is Axios?
Axios is a promise-based HTTP Client for node.js and the browser. It is isomorphic (= it can run in the browser and nodejs with the same codebase). On the server-side it uses the native node.js http module, while on the client (browser) it uses XMLHttpRequests. 

### Features

* Make XMLHttpRequests from the browser
* Make http requests from node.js
* Supports the Promise API
* Intercept request and response
* Transform request and response data
* Cancel requests
* Automatic transforms for JSON data
* Client side support for protecting against XSRF

### Installing

Using npm:
```
$ npm install axios
```



## Extra Topic
### 1. HTTP
>Hypertext Transfer Protocol (HTTP) is an application-layer protocol for transmitting hypermedia documents, such as HTML. It was designed for communication between web browsers and web servers, but it can also be used for other purposes. HTTP follows a classical client-server model, with a client opening a connection to make a request, then waiting until it receives a response. HTTP is a stateless protocol, meaning that the server does not keep any data (state) between two requests.
#### 1.1 简介
HTTP (Hypertext Transfer Protocol 超文本传输协议) 协议，协议详细规定了浏览器和万维网服务器之间互相通信的规则。

#### 1.2 请求报文
关注重点：格式与参数

```
行      POST  /s?ie=utf-8  HTTP/1.1 
头      Host: atguigu.com
        Cookie: name=guigu
        Content-type: application/x-www-form-urlencoded
        User-Agent: chrome 83
空行
体      username=admin&password=admin
```

#### 1.3 响应报文
```
行      HTTP/1.1  200  OK
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
```

### 2. Express的基本使用
```javascript
//1. 引入express
const express = require('express');

//2. 创建应用对象
const app = express();

//3. 创建路由规则
// request 是对请求报文的封装
// response 是对响应报文的封装
app.get('/', (request, response)=>{
    //设置响应
    response.send('HELLO EXPRESS');
});

//4. 监听端口启动服务
app.listen(8000, ()=>{
    console.log("服务已经启动, 8000 端口监听中....");
});
```
