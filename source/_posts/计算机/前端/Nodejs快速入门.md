---
title: Node.js快速入门
img: https://images.pexels.com/photos/10567277/pexels-photo-10567277.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500
excerpt: web应用的后端开发，其中有express后端开发框架
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
tags: [Node.js,前端,后端,JavaScript]
mathjax: true
date: 2022-04-27 19:57:18
---

# Node.js快速入门

## 一、Node.js

### 1.前言

本笔记学习视频： <https://www.bilibili.com/video/BV1a34y167AZ>

[Node.js](https://nodejs.org/zh-cn/)是一个基于Chrome V8引擎的JavaScript运行环境。

Node.js是Javascript的后端运行环境，无法调用DOM和BOM等浏览器内置API。

```bash
#执行js文件
node "要执行的js文件"
```

### 2.模块

#### 2.1 fs文件系统模块

Node.js提供的用来操作文件的模块。

```js
//导入fs模块
const fs=require('fs')
```

* `fs.readFile(path[,options],callback)`方法，读取指定文件的内容
  * 参数：文件路径、表示以什么编码格式读取文件、文件读取完成后，通过回调函数拿到读取的结果。
  * 回调函数的参数err对象是否为null，是读取成功，否则读取失败`err.message`返回失败信息。
* `fs.writeFile(file,data[,options],callback)`方法，用来向指定文件写入内容
  * 参数：指定存放的文件路径、表示写入的内容、表示写入格式、写入完成后的回调函数。
  * 写入成功err为null，否则为错误对象。

#### 2.2 path路径模块

```js
//导入path模块
const path=require('path')
```

* `path.join([...path])`方法，将多个路径片段拼接成完整的路径
  * 参数：多个路径字符串
* `path.basename(path[,ext])`方法，从路径解析文件名
  * 参数：路径、文件扩展名

#### 2.3 http模块

用来创建web服务器的模块。

创建web服务器步骤：

```js
//1.导入http模块
const http=require('http')
//2.创建http实列
const server=http.createServer()
//3.绑定request事件
server.on('request',function(req,res){
  console.log('客户端请求了服务器')
  //req是请求对象，包含了与客户端相关的数据和属性
  const url = req.url   //客户端请求的URL地址
  const method = req.method  // 客户端的method请求类型
  const str = '你的请求地址为'+url+'，请求类型是'+method
  res.setHeader('Content-Type','text/html; charset=utf-8')  //防止中文乱码
  res.end(str) //向客户端发送指定内容，并结束这次请求的处理过程
})
//4.启动服务器
server.listen(8008,()=>{
  console.log('http server running at http://127.0.0.1:8008')
})
```

根据url响应不同的html内容：

```js
server.on('request',(req,res)=>{
  const url = req.url 
  let content = '<h1>404 Not found!<h1>' //默认输出内容
  if(url==='/'||url==='/index.html'){
    content='<h1>首页<h1>'
  }else if(url==='/about.html'){
    content='<h1>关于页面<h1>'
  }
  res.setHeader('Content-Type','text/html; charset=utf-8') 
  res.end(content) 
})
```

#### 2.4 模块化

模块化是指解决一个复杂问题时，自顶向下逐层把系统划分成若干模块的过程。
优点：

1. 提高代码的复用性
2. 提高代码的可维护性
3. 可以实现按需加载

分类：

* 内置模块：Node.js提供的模块
* 自定义模块：用户自己创建的js文件
* 第三方模块：第三方开发出来的模块，需要先下载

加载模块：

```js
const fs=require('fs')  //加载内置模块，直接写名字
const fs=require('./custom.js') //加载用户自定义模块，需要添加路径
const fs=require('moment') //加载第三方模块，直接写名字
```

注意：`require()`方法加载模块，就会执行模块

模块作用域：
自定义模块的变量、方法，只能在当前模块内被访问。

每个js模块都有一个module对象，它里面存储了和当前模块有关的信息。

`moudle.exports`对象，将模块内的成员共享出去。

```js
//1.挂载属性
moudle.exports.uname='zs';
moudle.exports.sayHello=function(){
  console.log('hello '+uname);
}

//2.定义新对象
module.exports = {
    uname: 'zs',
    sayHello(){
        console.log('hello '+uname);
    }
}
```

简化代码使用`exports`对象，与`moudle.exports`对象用法一样，且两者指向一样。

#### 2.5 包

**包**是指第三方模块。
`node_modules`文件夹存放项目安装的包，`package-lock.json`存放包的配置文件

```bash
#安装全局包
npm install 包名 -g
#安装核心依赖包
npm install 包名
#安装开发依赖包
npm install 包名 -D
#安装指定版本的包
npm install 包名@2.22.2
# 版本号：第一位数字：大版本、第二位数字：功能版本、第三位数字：bug修复版本 
#卸载核心依赖包
npm uninstall 包名
#卸载全局包
npm uninstall 包名 -g
```

npm规定，在项目根目录中，必须提供一个叫做`package.json`的包管理配置文件，记录一些项目信息

* 项目的名称、版本号、描述等
* 项目中都用到了哪些包
* 哪些在开发中使用
* 哪些在开发和部署都要使用

```bash
#新建项目时，创建package.json文件
npm init -y
```

注意：

* 项目所在的文件夹路径只能为英文
* 运行npm install安装包时，会自动向`package.json`添加信息
* 运行npm uninstall卸载包时，会自动删除`package.json`中包相关信息
* 节点dependencies会记录开发和部署都会用到的包
* 节点devDependencies记录仅开发时用的包

切换国内源下载包

```bash
# 查看当前包的下载源
npm config get registry
# 切换国内源
npm config set registry=https://registry.npm.taobao.org/
```

**模块加载**:

* 模块在第一次加载后会被缓存
* 无论什么模块，都会先从缓存加载
* 被加载目录先查找`package.json`的文件，并寻找`main`属性，作为`require()`加载入口
* 如果没有`package.json`文件，查找该目录下的`index.js`文件
* 如果没有找到`index.js`文件，会输出错误信息

### 3.Express

> 基于Node.js平台，快速、开发、极简的Web开发框架

#### 3.1 基本使用

```js
//导入express
const express=require('express')
//创建web服务器
const app=express()
//启动web服务器
app.listen(80,()=>{
    console.log('express server running at http://127.0.0.1');
})
```

监听请求

* GET请求

```js
app.get('/user/:id',function(req,res){
  //函数
  //req请求对象
  //res响应对象
  res.send() //把内容响应给客户端
  req.query() //访问客户端通过查询字符串的形式发送到服务器的参数
  req.params   // 访问url中，通过: 匹配到动态参数
})
```

* POST请求

```js
app.post('url',function(req,res){
  //函数
  //req请求对象
  //res响应对象
  res.send() //把内容响应给客户端
})
```

```js
//托管静态资源，让文件夹都能让人访问，但存放静态资源的目录名不会出现在url中
app.use(express.static('文件夹'))
//可以在url添加文件夹1的名称来访问内容
app.use('文件夹1',express.static('文件夹2'))
```

#### 3.2 路由

> Express 中，客户端请求与服务器处理函数之间的映射关系。

Express的路由分为3部分，分别是请求的类型、请求的URL、处理函数

为了方便管理需要对路由进行模块化：

1. 创建路由模块的js文件
2. 调用`express.Router()`函数创建路由对象
3. 向路由对象上挂载路由
4. 使用`module.exports`向外共享路由对象
5. 使用`app.use()`函数注册路由模块

```js
var express = require('express')
var router = express.Router()

//挂载路由
router.get('/user',(req,res)=>{
  res.send('GET user request')
})

router.post('/user',(req,res)=>{
  res.send('POST user request')
})

//向外导出路由对象
module.exports = router
```

#### 3.3 中间件

当一个请求到达Express的服务器后，可以连续调用多个中间件，从而对这次请求进行预处理。

中间件函数的形参必须包含next参数。

next()函数把流转关系转到下一个中间件。

```js
//中间件函数
const mw = (req,res,next)=>{
  next()   //转到下一个中间件，没有就转给路由
}
```

调用`app.use(中间件函数)`，可以定义一个全局生效的中间件。

多个中间件共享同一个**req**和**res**，可以在上游添加自定义的属性或者方法，给下游使用

局部中间件：把中间件对象放在路由的url与处理函数之间，该中间件只对这个路由产生效果。

分类：

1. 应用级别的中间件：绑定到app实例上的中间件
2. 路由级别的中间件：绑定到router实例上的中间件
3. 错误级别的中间件：专门捕获项目的抛出的异常错误（需要放在所有路由之后），必须有四个形参，(err,req,res,next)
4. Express内置的中间件
   1. `express.static`快速托管静态资源
   2. `express.json`解析JSON格式的请求体数据
   3. `express.urlencoded`解析url格式的请求数据
5. 第三方的中间件

自定义中间件：

```js
const qs = require('querystring')
app.use((req,res,next)=>{
  let str =''
  req.on('data',(chunk)=>{   //监听data事件
    str += chunk
  })
  req.end('end',()=>{  //监听end事件
    console.log(str)
    //查询字符串解析成对象
    const body = qs.parse(str)
    console.log(body)
    // 挂载解析属性
    req.body = body
    next()
  })
})
```

#### 3.4 接口

```js
//创建服务器
const express=require('express')
const app=express()

//解析post请求的body
app.use(express.urlencoded({ extended: false }))

//配置跨域cors中间件
const cors = require('cors')
app.use(cors())

const router=require('./router')
app.use('/api',router)


app.listen(8008,()=>{
    console.log('express server running at http://127.0.0.1:8008');
})

```

```js
const express = require('express')
const router = express.Router()

//GET接口
router.get('/get',(req,res)=>{
  const query=req.query
  res.send({
    status: 0,
    msg: 'GET 请求成功',
    data: query
  })
})

//POST接口
router.post('/post',(req,res)=>{
  const body=req.body
  res.send({
    status: 0,
    msg: 'POST 请求成功',
    data: body
  })
})

//向外导出路由对象
module.exports = router
```

> 跨域问题：CORS和JSONP
CORS实现跨域响应步骤：

1. 安装cors中间件：`npm i cors`
2. 使用`const cors=require('cors')`导入中间件，要在路由之前
3. 在路由之前调用配置中间件`app.use(cors())`

### 4.数据库

#### 4.1 MySQL使用入门

结构：数据库、数据表、数据行、字段（列）

SQL：结构化查询语言，专门访问和处理**关系数据库**。

sql的关键字大小写不敏感

```sql
--向表中插入数据
INSERT INTO 表名称(列名称)  values(数据)
```

```sql
-- 删除表中某一行数据
DELETE FROM 表名称  WHERE 列名称 = 某值
```

```sql
-- 更新表中某一行某一列数据
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```

```sql
--从指定的表中查询所有的数据
SELECT * FROM 表名称
--从指定的表中查询特定列的数据，查询多个列用英文逗号隔开
SELECT 列名称 FROM 表名称
```

**WHERE**可以添加运算符来筛选特定字段

* `=`、`<>`、`>`、`<`、`>=`、`<=`、`BETWEEN`、`LIKE`
* `AND`、`OR`运算符就是且和或，用来增加筛选条件

```sql
--按照列名称大小进行升序排序
SELECT * FROM 表名称 ORDER BY 列名称
--按照列名称大小进行降序排序 DESC
SELECT * FROM 表名称 ORDER BY 列名称 DESC
--按照列名称1进行大范围的降序排序再按照列名称2进行小范围的升序排序
SELECT * FROM 表名称 ORDER BY 列名称1 DESC, 列名称2 ASC
```

```sql
--统计表中列为某值的个数
SELECT COUNT(*) FROM 表名称 WHERE 列名称 = 某值
--统计表中列为某值的个数，并给结果起名字
SELECT COUNT(*) AS 名称 FROM 表名称 WHERE 列名称 = 某值
```

#### 4.2 项目中连接MySQL数据库

>1.安装MySQL数据库第三方模块`mysql`

```bash
#mysql数据库
npm i mysql
```

>2.通过mysql模块连接到MySQL数据库

```js
//导入模块
const mysql = require('mysql')
//创建连接池
const db = mysql.createPool({
  host: '127.0.0.1',
  user: 'root',
  port: 3306,
  password: 'root',
  database: 'mydb_01'
})
//检查连接正常
db.query('select 1',(err,res)=>{
  if(err) return console.log(err.message)
  console.log(res)
})
```

>3.通过mysql模块执行SQL语句

```js
const sqlStr = 'select * from users'
db.query(sqlStr,(err,res)=>{
  if(err) return console.log(err.message)   //查询失败
  console.log(res)   // 查询成功
})

// 插入字段
const user={name: 'Spuer-Man' password: '123'}
//定义待执行的sql语句
const sqlStr = 'insert into users (name,password) values (?,?)'    //英文问号作为占位符
db.query(sqlStr,[user.name,user.password],(err,res)=>{
  if(err) return console.log(err.message)
  if(res.affectedRows === 1){  //判断数据是否插入成功
    console.log('插入数据成功')
  }
})
```

#### 4.3 身份验证

服务端渲染：在服务器动态拼接字符串数据，然后发送HTML页面
优点：

* 前端耗时少
* 有利于SEO
缺点：
* 占用服务器端资源
* 不利于前后端分离，开发效率低

前后端分离：后端只负责API接口，前端使用Ajax调用接口
优点：

* 开发体验好
* 用户体验好
* 减轻服务器端的渲染压力
缺点：
* 不利于SEO

**身份认证**：

>1.服务端渲染：Session认证机制

* HTTP请求的无状态性，每次HTTP请求都是独立的。
* Cookie存储在浏览器中不超过4kb的字符串。不同域名的cookie各自独立，每当客户端发起请求时，会自动把当前域名下的未过期的cookie一同发送给服务器。
* 服务器先通过响应头发送cookie给浏览器，浏览器保存cookie，浏览器再通过请求头发送cookie给服务器，服务器再响应内容。
* cookie不具有安全性
* session验证

```js
//使用session中间件
const session = require('express-session')
app.use(
  session({
    secret: 'zs',
    resave: false,
    saveUninitialized: true
  })
)
//解析提交的表单数据
app.use(express.urlencoded{extended: false})
//登录API接口
app.post('/api/login',(req,res)=>{
  //判断用户提交信息是否正确
  if(req.body.username !== 'admin' || req.body.password !== '000000'){
    return res.send({ status: 1,msg: '登陆失败'})
  }
  req.session.user = req.body
  req.session.islogin = true
  res.send({status: 0,msg: '登录成功'})
})
//取出session信息
app.get('/api/username',(req,res)=>{
  if(!req.session.islogin){
    return res.send({status: 1,msg: 'failed'})
  }
  res.send({
    status: 0,
    msg: 'success',
    username: req.session.user.username
  })
})
//清除session
app.post('/api/logout',(req,res)=>{
  req.session.destroy()
  res.send({status: 0,msg: '退出成功'})
})
```

>2.前后端分离：JWT认证机制

由于session需要配合cookie才能实现，而cookie默认不支持跨域访问，此时需要额外配置。

JWT是最流行的跨域认证解决方案。
服务器将用户信息加密成Token字符串，发送到客户端，客户端保存后，再发送Token，服务器把Token还原后进行身份验证。

JWT的组成部分：Header（头部）、Payload（有效荷载）、Signature（签名），三者使用英文点号分割

JWT放在HTTP请求头的Authorization字段中

安装JWT包

```bash
npm i jsonwebtoken express-jwt
```

```js
//导入JWT
const jwt = require('jsonwebtoken')
const expressJwt = require('express-jwt')
//定义secret密钥，用来加密和解密
const secretKey = 'my_secret_key'
//解析token，把用户信息挂载在req.user上
app.use(expressJwt({ secret: secretKey }).unless({ path: [/^\/api\//] }))
//生成token
const tokenStr =jwt.sign({username: 'admin'},secretKey,{expiresIn: 60*60 })
```

### 5.在express中使用swagger写API文档

#### 5.1 安装和使用swagger

>方法一：使用jsdoc文档注释来生成API文档

```bash
# 安装相关工具
npm i swagger-ui-express
npm i swagger-jsdoc
```

```js
// app.js
const swaggerUi = require('swagger-ui-express');
const swaggerJsdoc = require('swagger-jsdoc');
const options = {
    definition: {
      openapi: '3.0.0',
      info: {
        title: 'Hello World',
        version: '1.0.0',
        description: '项目描述',
      },
    },
    apis: ['./router/*.js'], //路由文件夹下面的所有js文件
};
const swaggerSpec = swaggerJsdoc(options);
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));
```

```js
//在路由模块前写下面的注释 
/**
 * @swagger
 * /my/article/addcate:
 * post:
 *   tags:
 *      - 文章分类
 *   summary: 新增文章分类
 *   description: 来新增加一个文章的分类
 *   parameters:
 *      - name: name
 *        description: 文章分类名
 *        in: body
 *        required: true
 *        type: string
 *      - name: alias
 *        description: 文章分类的别名
 *        in: body
 *        required: true
 *        type: string
 *   responses:
 *      0:
 *         description: success，新增文章分类成功
 *      1:
 *         description: failed，新增文章分类失败
 */
```

> 方法二：使用json文件来写

```bash
# 安装相关工具
npm i swagger-ui-express
```

```js
// app.js
const swaggerUi = require('swagger-ui-express');
const swaggerDocument = require('./swagger.json');
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument));
```

不熟悉语法时，使用在线的 [swagger editor](https://editor.swagger.io/) 进行API接口的编辑
选择`File->Convert and save as JSON`，下载`swagger.json`放进项目中
熟悉语法后，直接对`swagger.json`进行修改，不需要编辑器了
官方文档：<https://swagger.io/docs/specification/about/>

> 方法三：使用yaml文件来写

其实是利用yamljs模块把yaml文件转化为json文件，如果直接写json文件，会发现json文件的所有属性都会加双引号，还有很多格式类的限制，没有yaml写起来舒服，快速。
而且yaml文件写法才是最贴合官网的语法。

```bash
# 安装相关工具
npm i swagger-ui-express
npm i --save yamljs
```

```js
// app.js
const swaggerUi = require('swagger-ui-express');
const YAML = require('yamljs');
const swaggerDocument = YAML.load('./swagger.yaml');
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument));
```

#### 5.2 YAML

YAML是一种标记语言，文件名为`.yaml`

1. 大小写敏感
2. 使用缩进标识层级关系
3. 缩进不允许Tab，只允许空格
4. 缩进的空格个数不重要，但是同级元素需要左对齐
5. 注释：`#`
6. 支持的数据类型：
   1. 对象
   2. 数组：使用`-`添加在元素前，表示该数组的元素，每一个数组元素都得添加。
   3. 纯数：任意不可分的数据
7. `|`使用换行 `>`忽略换行
8. `$变量`标记一个属性，用`<<: *变量`来导入标记非重复的属性值

#### 5.3 OpenAPI 规范

* 数据类型

| `type` | `format`| 评论 |
| --- | --- | --- |
| `integer` | `int32` | 有符号 32 位 |
| `integer` | `int64` | 有符号 64 位 (a.k.a long) |
| `number` | `float` |  |
| `number` | `double` |  |
| `string` |  |  |
| `string` | `byte` | base64 编码字符 |
| `string` | `binary` | 任何八位字节序列 |
| `boolean` |  |  |
| `string` | `date` | 由`full-date`定义 [RFC3339](https://xml2rfc.ietf.org/public/rfc/html/rfc3339.html#anchor14) |
| `string` | `date-time` | 由`date-time`定义 [RFC3339](https://xml2rfc.ietf.org/public/rfc/html/rfc3339.html#anchor14) |
| `string` | `password` | 提示 UI 以隐藏输入 |

> URL中的相对引用

除非另有说明，否则所有属于 URL 的属性都可以是 [RFC3986](https://tools.ietf.org/html/rfc3986#section-4.2) 定义的相对引用。使用[服务器对象](#66-服务器对象)中定义的 URL 作为基本 URI 来解析相对引用。
`$ref` 中使用的相对引用按照 JSON 引用进行处理，使用当前文档的 URL 作为基本 URI。

具体规范见：[OpenAPI规范英文版](https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.0.3.md)，[OpenAPI规范中文版](https://openapi.apifox.cn/)
