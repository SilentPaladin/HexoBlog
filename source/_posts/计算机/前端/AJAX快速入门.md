---
title: AJAX快速入门
img: https://images.pexels.com/photos/7881305/pexels-photo-7881305.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500
excerpt: 介绍了前后端的通信方式，以及Ajax的封装原理，以及jQuery的Ajax的使用，以及常见的HTTP请求响应构成。
top: false
toc: true
tocOpen: true
onlyTitle: false
comments: true
share: true
copyright: true
donate: false
prismjs: default
categories: 计算机
tags: [前端,Ajax,前后端通信]
mathjax: true
date: 2022-04-26 17:02:22
---

# AJAX快速入门

## 一、Ajax入门

### 1.前言

本笔记参考视频：<https://www.bilibili.com/video/BV1zs411h74a>

客户端请求数据：

1. 向服务器请求数据资源
2. 服务器处理这次数据请求
3. 把数据响应给客户端

在网页请求服务器的数据资源，需要用到`XMLHttpRequest`对象

资源请求方式：

* get请求，常用于获取服务端资源
* post请求，常用于向服务器提交数据

网页中利用`XMLHttpRequest`对象和服务器进行数据交互的方式，称为Ajax。

### 2.前后端通信

#### 2.1jQuery中的Ajax

浏览器中提供的`XMLHttpRequest`用法比较复杂，jQuery对`XMLHttpRequest`进行了封装，提供了一系列Ajax相关的函数，大大的降低了Ajax的使用难度。
> jQuery是类似vue和react的框架，现在用的很少

jQuery中发起Ajax请求的常用三个方法：

* $.get()
  功能单一，专门发起get请求。

```javascript
$.get(url,[data],[callback])
```

| 参数名   | 参数类型 | 是否必选 | 说明                     |
| -------- | -------- | -------- | ------------------------ |
| url      | string   | 是       | 要请求的资源地址         |
| data     | object   | 否       | 请求资源期间要携带的参数 |
| callback | function | 否       | 请求成功时的回调函数     |

* $.post()
  功能单一，专门发起post请求。

```javascript
$.post(url,[data],[callback])
```

| 参数名   | 参数类型 | 是否必选 | 说明                     |
| -------- | -------- | -------- | ------------------------ |
| url      | string   | 是       | 提交数据的地址           |
| data     | object   | 否       | 要提交的数据             |
| callback | function | 否       | 数据提交成功时的回调函数 |

* $.ajax()
  功能综合，可以发送GET或者POST请求

```javascript
$.ajax({
  type: '',  //请求方式，GET 或者 POST
  url: '',   //请求的URL地址
  data: { },  //这次请求携带的数据
  success: function(res){} //请求成功后的回调函数
})
```

#### 2.2 接口

* 使用Ajax请求数据时，被请求的URL地址，就叫做数据接口。同时，每个接口必须有请求方式。
* 接口测试工具Postman
* 接口文档书写规范
  1. 接口名称：用来标识各个接口的简单说明
  2. 接口URL：接口调用地址
  3. 调用方式：接口调用方式
  4. 参数格式：接口需要传递的参数，包括参数名词、参数类型、是否必选、参数说明这四项内容。
  5. 响应格式：接口的返回值的详细描述，一般包括数据名称、数据类型、说明这三项内容。
  6. 返回示例（可选）：通过对象的形式，返回数据的结构。

#### 2.3 from表单

* 作用：在网页中主要负责数据采集功能。
* 组成部分：
  1. 表单标签
  2. 表单域：文本框、密码框等。
  3. 表单按钮。
* 属性：
  * action
    * 属性值：URL地址（默认值为当前页面的URL地址）
    * 说明：规定当提交表单时，向何处发送数据
  * target
    * 属性值
  
      | 值        | 说明                     |
      | --------- | ------------------------ |
      | _blank    | 在新窗口打开             |
      | _self     | 默认，在相同的框架中打开 |
      | _parent   | 在父框架集中打开         |
      | _top      | 在整个窗口中打开         |
      | framename | 在指定的框架中打开       |
    * 说明：规定何处打开action URL
  * method
    * 属性值：get（默认，通过URL地址来提交，适合少量数据）或post（隐藏提交的数据，适合大量、复杂的数据提交）
    * 说明：规定以何种方式把表单数据提交到action URL
  * enctype
    * 属性值
      | 值                                | 说明                                                   |
      | --------------------------------- | ------------------------------------------------------ |
      | application/x-www-form-urlencoded | 在发送前编码所有字符（默认）                           |
      | multipart/form-data               | 不对字符编码。在使用包含文件上传的控件的表单，必须使用 |
      | text/plain                        | 空格转化为“+”加号，但不对特殊字符编码                  |
    * 说明：规定在发送表单数据之前如何对其编码
* 表单提交的缺点
  * 页面会发生跳转
  * 页面之前的状态和数据会丢失
  * 解决方案：表单只负责采集数据，Ajax负责将数据提交到服务器

#### 2.4 Ajax提交表单数据

```js
$('#form1').submit(function(e)){
  alert('监听到了表单的提交事件');
  e.preventDefault(); //阻止表单的默认提交行为
  $(selector).serialize(); //一次性获取表单中的所有数据
}

$('#form1').on('submit',function(e))){
  alert('监听到了表单的提交事件');
  e.preventDefault();//阻止表单的默认提交行为
  $(selector).serialize(); //一次性获取表单中的所有数据
}
```

#### 2.5 模板引擎

>模板引擎现在以及内置于大型框架中了，其中art-template为最快的模板引擎

* art-template使用步骤
  * 导入art-template
  * 定义数据
  * 定义模板
  * 调用template函数
  * 渲染HTML结构
* art-template标准语法
  * 输出：变量输出、对象属性的输出、三元表达式的输出、逻辑输出、算术输出
  `{{value}}，{{obj.key}}，{{obj.['key']}}，{{a?b:c}}，{{a||b}}，{{a+b}}`
  * 原文输出：如果value包括HTML标签结构，需要原文输出才能保证能够被渲染
  `{{@ value}}`
  * 条件输出
  `{{if value}} 按需输出的内容 {{/if}}`
  `{{if value1}} 按需输出的内容 {{else if value2}} 按需输出的内容{{/if}}`
  * 循环输出：`{{$index}}`来得到索引，`{{$value}}` 来得到值
  `{{each arr}} 循环内容 {{/each}}`
  * 过滤器：把`value`提交给`filterName`函数进行处理，得到一个新值
  `{{value|filterName}}`
  `template.defaults.imports.filterName=function(value){ }`
* 模板引擎原理：利用正则表达式提取数据，再用字符串替换方法替换字符串
* 正则与字符串操作
  * 语法：`RegExpObject.exec(string)`函数用来检索字符串中的正则表达式的匹配，如果字符串有匹配的值，则返回该匹配值，否则返回Null

  ```js
  var str = 'hello'
  var pattern = /o/
  console.log(pattern.exec(str))
  ```

  * `replace()`函数用于一些字符替换另一些字符
* 实现简易模板引擎
  * 定义模板结构
  * 预调用模板引擎
  * 封装template函数
  * 导入并使用自定义的模板引擎

#### 2.6 XMLHttpRequest

>XMLHttpRequest是浏览器提供的JavaScript对象，可以请求服务器的数据资源，JQuery的Ajax函数就是基于xhr封装的

```js
//1. 创建xhr对象
var xhr = new XMLHttpRequest();
//2. 调用open()函数
xhr.open('GET','url');
//3. 调用send()函数
xhr.send();
//4. 监听onreadystatechange事件
xhr.onreadystatechange=function(){
  //监听请求状态与服务器响应状态
  if(xhr.readyState===4 && xhr.status===200){
    console.log(xhr.responseText)
  }
}
```

* readyState的请求状态

| 值  | 状态             | 说明                                         |
| --- | ---------------- | -------------------------------------------- |
| 0   | unsent           | XMLHttpRequest对象已被创建，但未调用open方法 |
| 1   | opened           | open方法已经被调用                           |
| 2   | headers_received | send方法已经被调用                           |
| 3   | loading          | 数据接收中                                   |
| 4   | done             | Ajax请求完成                                 |

* URL参数传递需要在地址后面添加?号，多个参数使用&进行分割
  * 参数不能是其他语言，其他语言需要进行编码
    `encodeURI()`编码函数
    `decodeURI()`解码函数

```js
//1. 创建xhr对象
var xhr = new XMLHttpRequest();
//2. 调用open()函数
xhr.open('POST','url');
//3.设置Content-Type属性
xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
//3. 调用send()函数
xhr.send('参数1&参数2');
//4. 监听onreadystatechange事件
xhr.onreadystatechange=function(){
  //监听请求状态与服务器响应状态
  if(xhr.readyState===4 && xhr.status===200){
    console.log(xhr.responseText)
  }
}
```

#### 2.7 数据交换格式

前后端数据传输的格式，分别为**XML**和**JSON**，其中**XML**用的很少，重点为**JSON**

* XML
  与html类似，为可扩展标记语言。
  缺点：
  * XML格式臃肿，数据无关代码多，传输效率低。
  * Javascript解析XML比较麻烦。
* JSON
  英文名JavaScript Object Notation，即"JavaScript对象表示方法"。本质是字符串。
  * 两种结构
    * 对象结构
      格式为花括号包裹的键值对，key必须为英文双引号表示的字符串，value可以是数字、字符串、布尔值、null、数组、对象这六种类型。
    * 数组结构
      格式为中括号包裹的数据，数据类型可以是数字、字符串、布尔值、null、数组、对象这六种类型。
  * JSON转化Javascript对象
    * 使用`JSON.parse('JSON字符串')`方法
  * Javascript对象转化JSON
    * 使用`JSON.stringify(JS对象)`方法
* 序列化与反序列化
  * 序列化：把**数据对象**转化为**字符串**的过程
  * 反序列化：把**字符串**转化为**数据对象**的过程

#### 2.8 自定义Ajax函数

* 定义参数选项
  * method 请求类型
  * url          请求的URL地址
  * data       请求携带的数据
  * success  请求成功之后的回调函数

```js
function resolveData(data){
  var arr=[];
  for(var k in data){
    arr.push(k+'='+data[k])
  }
  return arr.join('&');
}

function template(options){
  var xhr=new XMLHttpRequest();
  var qs = resolveData(options.data);
  if(options.method.toUpperCase()==='GET'){
    xhr.open('GET',options.url+'?'+qs);
    xhr.send();
  }else if(options.method.toUpperCase()==='POST'){
    xhr.open('POST',options.url);
    xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
    xhr.send(qs);
  }
  xhr.onreadystatechange=function(){
    if(xhr.readyState===4 && xhr.status===200){
      var result = JSON.parse(xhr.responseText);
      options.success(result);
    }
  }
}
```

#### 2.9 XMLHttpRequest Level2新特性

* 旧版缺点：
  1. 只支持文本传输，无法读取和上传文件
  2. 接收数据没有进度信息

* 设置HTTP请求时限
  * `xhr.timeout=3000`
  * `xhr.ontimeout=function(){}`
* FormData对象管理表单数据

```js
 //创建Form Data对象
 var fd =new FormData();
//为FormData添加表单项
 fd.append('uname','ithm');
//创建xhr对象
 var xhr=new XMLHttpRequest()
//指定请求与url地址
 xhr.open('POST','url');
//提交
 xhr.send(fd);


 //获取表单元素
 var form=document.querySelector('#form1');
 form.addEventListener('submit',function(e){
  e.preventDefault();
  var fd=new FormData(form);
  var xhr=new XMLHttpRequest();
  xhr.open('POST','url');
  xhr.send(fd);
  xhr.onreadystatechange=function(){}
 })
```

* 上传文件
  1. 定义UI结构
  2. 验证是否选择文件
  3. 向FormData中追加文件
  4. 使用xhr发起上传文件请求
  5. 监听onreadystatechange事件

```js
var btn = document.querySelector('#btnUpload');
btn.addEventListener('click',function(){
  var files=document.querySelector('#file1').files;
  if(files.length<=0){
    return alert('');
  }
  var fd=new FormData();
  fd.append('avatar',files[0]);
  var xhr=new XMLHttpRequest();
  xhr.open('POST','url');
  xhr.send(fd);
  //文件上传进度
  xhr.upload.onprogress=function(e){
    if(e.lengthComputable){
      var procent=Math.ceil((e.loaded/e.total)*100);
      console.log(procent);
    }

  }
  xhr.onreadystatechange=function(){
      if(xhr.readyState===4 && xhr.status===200){
        var data = JSON.parse(xhr.responseText);
        if(data.status===200){
          //上传成功
        }else{
          //上传失败
        }
    }
  }
})
```

#### 2.10 axios

专注于网络数据请求的库，更简单易用。

* 发起GET请求
  `axios.get('url',{params:{（js对象）参数}}).then(callback)`
* 发起POST请求
  `axios.post('url',{（js对象）参数}).then(callback)`
* 类似`$.ajax()`的函数

```js
axios({
  method: '请求类型',
  url: '请求的URL地址',
  data: {数据},
  params: {参数}
}).then(callback)
```

#### 2.11 同源策略

同源：如果两个页面的协议、域名和端口都相同，则两个页面具有相同的源。
同源策略：浏览器规定，A网站的JS，不允许和非同源的网站C之间进行资源交互。

1. 无法读取非同源网页的cookie、LocalStorage和IndexedDB
2. 无法接触非同源网页的DOM
3. 无法向非同源地址发送Ajax请求

跨域：非同源就是跨域。
跨域请求的方案：**JSONP**和**CORS**。

* JSONP，只支持GET请求，不支持POST请求。
  由于浏览器同源策略的限制，网页无法通过Ajax请求非同源的接口数据，但是`<script>`标签不受限制，可以通过src属性，请求跨域的数据接口，并通过函数调用的形式，接收跨域接口响应回来的数据。

  ```js
  //自己实现JSONP
  <script>
    function success(data){
      console.log(data);
    }
  </script>
  <script src="http://url?callback=success&name=zs"></script>

  //jQuery的JSONP
  $.ajax({
    url: 'http://url?callback=success&name=zs',
    dataType: 'jsonp',
    jsonp: 'callback',
    jsonpCallback: 'success',
    success: function(data){
      console.log(data);
    }
  })
  ```

* CORS，不兼容性低端浏览器，支持GET和POST请求。

#### 2.12 防抖和节流

节流策略：减少一段时间内事件的触发频率，防止事件无限制被触发。

```js
var timer =null;  //定义节流阀

if(timer){  //判断节流阀是否为空
  return;
}

timer=setTimeout(function(){
  //处理事件
  timer=null;  //清空节流阀
},16)
```

#### 2.13 HTTP协议加强

通信三要素：

* 通信主体
* 通信内容
* 通信方式

网页内容叫做超文本，因此网页内容传输协议又叫做超文本传输协议(**H**yper**T**ext **T**ransfer **P**rotocol)，简称HTTP协议。

**HTTP请求消息**由请求行、请求头部、空行和请求体四个部分组成。

1. 请求行

<style>
.table li{
  text-align: center;
  list-style:none;
  display: inline-block;

  margin: 0;
  padding: 10px;
  border: 2px solid black;
  border-right:none ;
}
.a :first-child{
  background-color: #2ca9e1;
}
.a :nth-child(3){
 background-color: #765c47;
}
.a :nth-child(5){
 background-color: #f8b500;
}
.a :last-child{
  border-right: 2px solid black;
}
</style>
<ul class="table a">
  <li>请求方式</li><li>空格</li><li>URL</li><li>空格</li><li>协议版本</li><li>回车符</li><li>换行符</li>
</ul>

2. 请求头部：多行键值对组成，用来描述客户端的基本信息，从而把客户端相关的信息告知服务器。

<style>
.b :first-child{
  background-color: #727171;
}
.b :nth-child(2){
 background-color: #007bbb;
}
.b :nth-child(3){
  padding-left: 40px;
  padding-right: 40px;
  background-color: #ffea00;
}
.b :nth-child(5){
  border-right: 2px solid black;
}
</style>
<ul class="table b">
  <li>头部字段名称</li><li>:(冒号)</li><li>值</li><li>回车符</li><li>换行符</li>
</ul>

|    头部字段     | 说明                                         |
| :-------------: | -------------------------------------------- |
|      Host       | 要请求的服务器域名                           |
|   Connection    | 客户端与服务器的连接方式（keep 或keepalive） |
| Content-Length  | 用来描述请求体的大小                         |
|     Accept      | 客户端可识别的响应内容类型列表               |
|   User-Agent    | 产生请求的浏览器类型                         |
|  Content-Type   | 客户端告诉服务器实际发送的数据类型           |
| Accept-Encoding | 客户端可接收的内容压缩编码形式               |
| Accept-Language | 用户期望获得的自然语言的优先顺序             |

3. 空行(回车符或者换行符)：分割请求头部与请求体。

4. 请求体：存放要通过**POST方式**提交到服务器的数据（GET没有请求体）。

**HTTP响应消息**由状态行、响应头部、空行和响应体四个部分组成。

1. 状态行

<style>
.c :first-child{
  background-color: #2ca9e1;
}
.c :nth-child(3){
 background-color: #765c47;
}
.c :nth-child(5){
  background-color: #f8b500;
}
.c :last-child{
  border-right: 2px solid black;
}
</style>
<ul class="table c">
  <li>协议版本</li><li>空格</li><li>状态码</li><li>空格</li><li>状态码描述</li><li>回车符</li><li>换行符</li>
</ul>

2. 响应头部：多行键值对组成，来描述服务器的基本信息。具体细节查看[MDN](https://developer.mozilla.org/zh-CN/docs/Glossary/Response_header)文档。

<style>
.b :first-child{
  background-color: #727171;
}
.b :nth-child(2){
 background-color: #007bbb;
}
.b :nth-child(3){
  padding-left: 40px;
  padding-right: 40px;
  background-color: #ffea00;
}
.b :nth-child(5){
  border-right: 2px solid black;
}
</style>
<ul class="table b">
  <li>头部字段名称</li><li>:(冒号)</li><li>值</li><li>回车符</li><li>换行符</li>
</ul>

3. 空行(回车符或者换行符)：分割响应头部与响应体。

4. 响应体：服务器给客户端的资源。

**HTTP的请求方法**:

| 方法   | 描述                                                                               |
| ------ | ---------------------------------------------------------------------------------- |
| GET    | （查询）发送请求来获得服务器上的资源，请求体中不会包含请求数据，请求数据放在协议头 |
| POST   | （新增）向服务器提交资源，数据被包含在请求体中提交给服务器                         |
| PUT    | （修改）向服务器提交资源，并使用提交的新资源，替换掉服务器对应的旧资源             |
| DELETE | （删除）请求服务器删除指定的资源。                                                 |

**HTTP响应状态码**用来标识响应状态。
HTTP状态码由三个十进制组成，第一个数字反应了状态码的类型。
| 分类 | 说明                                           |
| ---- | ---------------------------------------------- |
| 1**  | 信息，服务器收到请求，需要请求者继续执行操作   |
| 2**  | 成功，操作被成功接收并处理                     |
| 3**  | 重定向，需要进一步的操作以完成请求             |
| 4**  | 客户端错误，请求包含语法错误或无法完成请求     |
| 5**  | 服务器错误，服务器在处理请求的过程中发生了错误 |

| 状态码 | 状态码描述 | 说明                                                     |
| ------ | ---------- | -------------------------------------------------------- |
| 200    | OK         | 请求成功。一般用于GET与POST请求                          |
| 201    | Created    | 已创建。成功请求并创建了新的资源。 通常用于POST与PUT请求 |

| 状态码 | 状态码描述        | 说明                                                                                          |
| ------ | ----------------- | --------------------------------------------------------------------------------------------- |
| 301    | Moved Permanently | 永久移动。请求资源已经被永久的移动到了新的URL，返回信息包括新的URL，浏览器会自动定向到新URL。 |
| 302    | Found             | 临时移动。资源只是临时被移动。                                                                |
| 304    | Not Modified      | 未修改。所请求的资源未修改，客户端从缓存访问资源。                                            |

| 状态码 | 状态码描述      | 说明                                           |
| ------ | --------------- | ---------------------------------------------- |
| 400    | Bad Request     | 语义有误，服务器无法理解。请求参数有误。       |
| 401    | Unauthorized    | 当前请求需要用户验证。                         |
| 403    | Forbidden       | 服务器已经理解请求，但是拒绝执行。             |
| 404    | Not Found       | 服务器无法根据客户端的请求找到资源。           |
| 408    | Request Timeout | 请求超时。服务器等待客户端发送的请求时间过长。 |

| 状态码 | 状态码描述            | 说明                                               |
| ------ | --------------------- | -------------------------------------------------- |
| 500    | Internal Server Error | 服务器内部错误，无法完成请求。                     |
| 501    | Not Implemented       | 服务器不支持该请求方式，无法完成请求。             |
| 503    | Service Unavailable   | 由于超载或系统维护，服务器暂时无法处理客户端的请求 |
