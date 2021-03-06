---
title: Node.js基础知识
date: 2021-07-10
tags:
 - Nodejs
categories: 
 - Node.js
---

# 1-node环境和浏览器区别
```javascript
/*
内置对象不同
浏览器环境中提供了window全局对象
NodeJS环境中的全局对象不叫window,叫global
 */
console.log(global);
/*
this默认指向不同
浏览器this默认指向window
Nodejs环境默认指向空对象{}
Node禁止函数函数this指向global
 */
console.log(this); // {}
/*
Node中没有DOM BOM相关的api
 */



```
# function
```javascript
//1.Node中任何一个模块(js) 都被外层函数所包裹
function (exports, require, module, __filename, __dirname) {
    exports:用于支持CommonJs模块化规范的暴露语法
    require:用于支持CommonJs模块化规范的引入语法
    module:用于支持CommonJs模块化规范的暴露语法
    __filename:当前文件的绝对路径
    __dirname:当前文件夹的绝对路径
}
设计外层函数作用
    1.用于支持模块化语法
    2.隐藏服务器内部实现
```

# Node-模块 exports/require
```javascript
NodeJS采用CommonJS规范现了模块系统  
CommonJS规范规定了如何定义一个模块，如何暴露(导出)模块中的变量函数，以及如何使用定义好的模块
在CommonJS规范中一个文件就是一个模块
在CommonJS规范中每个文件中的变量函数都是私有的，对其他文件不可见的
在CommonJS规范中每个文件中的变量函数必须通过exports暴露(导出)之后其它文件才可以使用
在CommonJS规范中想要使用其它文件暴露的变量函数必须通过require()导入模块才可以使用

let name = "feifan";
function sum(a,b) {
    return a + b;
}
//exports.xxx = xxx 暴露给外界
exports.str = name;
exports.sum = sum;
//方法2
module.exports.name = name;

//方法3 
global.sum = sum;

//require("路径") //导入模块
let aModule = require("./06-a");
console.log(aModule.str);
console.log(aModule.sum(9, 8));
```

# Buffer对象
```javascript
1.什么是Buffer?
*    1.它是一个【类似于数组】的对象，用于存储数据（存储的是二进制数据）。
*    2.Buffer的效率很高，存储和读取很快，它是直接对计算机的内存进行操作。
*    3.Buffer的大小一旦确定了，不可修改。
*    4.每个元素占用内存的大小为1字节。
*    5.Buffer是Node中的非常核心的模块，无需下载、无需引入,直接即可使用

Buffer是NodeJS全局对象上的一个类，是一个专门用于存储字节数据的类Node
JS提供了操作计算机底层API，而计算机底层只能识别0和1,所以就提供了一个专门用于存储字节数据的类

2.如何创建一个Buffer对象
2.1创建一个指定大小的Buffer
Buffer.alloc(size[, fill[, encoding]])
//size 新 Buffer 所需的长度。
//fill 用于预填充新 Buffer 的值。 默认值: 0。
//encoding  如果 fill 是字符串，则这就是它的编码。 默认值: 'utf8'。

2.2根据数组/字符串创建一个Buffer对象
Buffer.from(string[, encoding])
//string 要编码的字符串。encoding 默认值: 'utf8'。
 */
 
```
## Buffer常用实例方法
```javascript
/*
1.将二进制数据转换成字符串
返回: <string> 转换后的字符串数据。
buf.toString();

 2.往Buffer中写入数据
 buf.write(string[, offset[, length]][, encoding])

 string <string> 要写入 buf 的字符串。
 offset <integer> 开始写入 string 之前要跳过的字节数。默认值: 0。
 length <integer> 要写入的字节数。默认值: buf.length - offset。
 encoding <string> string 的字符编码。默认值: 'utf8'。
 返回: <integer> 已写入的字节数。



  3.从指定位置截取新Buffer
  buf.slice([start[, end]])
  start <integer> 新 Buffer 开始的位置。默认值: 0。
  end <integer> 新 Buffer 结束的位置（不包含）
```
## Buffer常用静态方法
```javascript
1.检查是否支持某种编码格式
Buffer.isEncoding(encoding)

2.检查是否是Buffer类型对象
Buffer.isBuffer(obj)

3.获取Buffer实际字节长度
Buffer.byteLength(string[, encoding])
注意点: 一个汉字占用三个字节

4.合并Buffer中的数据
Buffer.concat(list[, totalLength])
```
# 路径Patn
```javascript
1.路径模块(path)
封装了各种路径相关的操作
和Buffer一样,NodeJS中的路径也是一个特殊的模块
不同的是Buffer模块已经添加到Global上了, 所以不需要手动导入
而Path模块没有添加到Global上, 所以使用时需要手动导入

2.获取路径的最后一部分
path.basename(path[, ext])

3.获取路径
path.dirname(path)

4.获取扩展名称
path.extname(path)

5.判断是否是绝对路径
path.isAbsolute(path)

6.获取当前操作系统路径分隔符
path.delimiter  (windows中使用; linux中使用:)

7.获取当前路径环境变量分隔符
path.sep （windows是\ Linux是/） 

1.路径的格式化处理
// path.parse()  string->obj
// path.format() obj->string

2.拼接路径
path.join([...paths])

3.规范化路径
path.normalize(path)

4.计算相对路径
path.relative(from, to)

5.解析路径
path.resolve([...paths])

```
# npm包管理器
```javascript
  1.什么是包？
      我们电脑上的文件夹，包含了某些特定的文件，符合了某些特定的结构，就是一个包。

  2.一个标准的包，应该包含哪些内容？
      1)   package.json ------- 描述文件（包的 “说明书”，必须要有！！！）
      2)   bin -----------------可执行二进制文件
      3)   lib ---------------- 经过编译后的js代码
      4)   doc    ---------------- 文档（说明文档、bug修复文档、版本变更记录文档）
      5)   test --------------- 一些测试报告

  3.如何让一个普通文件夹变成一个包？
        让这个文件夹拥有一个：package.json文件即可,且package.json里面的内容要合法。
        执行命令：npm init
        包名的要求：不能有中文、不能有大写字母、同时尽量不要以数字开头、不能与npm仓库上其他包同名。

  4.npm与node的关系？（npm：node package manager）
        安装node后自动安装npm（npm是node官方出的包管理器，专门用于管理包）

  5.npm的常用命令？

      一、【搜索】：
            1.npm search xxxxx
            2.通过网址搜索：www.npmjs.com

      二、【安装】：(安装之前必须保证文件夹内有package.json，且里面的内容格式合法)

            1.npm install xxxxx --save   或   npm i xxxx -S   或   npm i xxxx
                备注：
                (1).局部安装完的第三方包，放在当前目录中node_modules这个文件夹里
                (2).安装完毕会自动产生一个package-lock.json(npm版本在5以后才有)，里面缓存的是每个下载过的包的地址，目的是下次安装时速度快一些。
                (3).当安装完一个包，该包的名字会自动写入到package.json中的【dependencies(生产依赖)】里。npm5及之前版本要加上--save后缀才可以。

            2.npm install xxxxx --save-dev  或  npm i xxxxx -D  安装包并将该包写入到【devDependencies(开发依赖中)】
              备注：什么是生产依赖与开发依赖？

                    1.只在开发时(写代码时)时才用到的库，就是开发依赖 ----- 例如：语法检查、压缩代码、扩展css前缀的包。
                    2.在生产环境中(项目上线)不可缺少的，就是生产依赖 ------ 例如：jquery、bootStrap等等。
                    3.注意：某些包即属于开发依赖，又属于生产依赖 -------例如：jquery。

            3.npm i xxxx -g  全局安装xxxx包（一般来说，带有指令集的包要进行全局安装，例如：browserify、babel等）
              全局安装的包，其指令到处可用，如果该包不带有指令，就无需全局安装。
              查看全局安装的位置：npm root -g

             npm view xxx versions 查看有那些版本
            4.npm i xxx@yyy :安装xxx包的yyy版本

            5.npm i ：安装package.json中声明的所有包

      三、【移除】：
            npm remove xxxxx  在node_module中删除xxxx包，同时会删除该包在package.json中的声明

      四、【其他命令】：
            1.npm aduit fix :检测项目依赖中的一些问题，并且尝试着修复。

            2.npm view xxxxx versions :查看远程npm仓库中xxxx包的所有版本信息

            3.npm view xxxxx version :查看npm仓库中xxxx包的最新版本

            4.npm ls xxxx :查看我们所安装的xxxx包的版本

       五、【关于版本号的说明】：
            "^3.x.x" ：锁定大版本，以后安装包的时候，保证包是3.x.x版本，x默认取最新的。
            "~3.1.x" ：锁定小版本，以后安装包的时候，保证包是3.1.x版本，x默认取最新的。
            "3.1.1" ：锁定完整版本，以后安装包的时候，保证包必须是3.1.1版本。
```

# mongooDB-mongoose连接数据库
```javascript
/*
* mongoDB:一个数据库品牌的名字
* mongod:启动数据库的命令
* mongo:连接接数据库的命令
* mongoose：在Node平台下，一个知名的用于帮助开发者连接mongoDB的包
* */
//为什么用mongoose？ 想在Node平台下，更加简单、高效、安全、稳定的操作mongoDB
//当引入第三方库的时候，如果在本文件夹内没有找到node_modules，找外层文件夹，直到根目录

//引入mongoose
let mongoose = require('mongoose');

//1.连接数据库
mongoose.connect('mongodb://localhost:27017/test')

//2.绑定数据库连接的监听
mongoose.connection.on('open', function (err) {
    if (err) {
        console.log("连接失败", err);
    } else {
        console.log("连接成功");
    }
})

```


# mogooseCRUD
```javascript
 -Create

      模型对象.create(文档对象，回调函数)
 -Read

      模型对象.find(查询条件[,投影],回调函数)不管有没有数据，都返回一个数组
      模型对象.findOne(查询条件[,投影],回调函数)找到了返回一个对象，没找到返回null
 -Update

     模型对象.updateOne(查询条件,要更新的内容[,配置对象],回调函数)
     模型对象.updateMany(查询条件,要更新的内容[,配置对象],回调函数)
     备注：存在update方法，但是即将废弃，查询条件匹配到多个时，依然只修改一个，强烈建议用updateOne或updateMany
 -Delete

      模型对象.deleteOne(查询条件,回调函数)
      模型对象.deleteMany(查询条件,回调函数)
      备注：没有delete方法，会报错！
      
备注： 以上所有方法，如果没有指定回调函数，则返回值是一个Promise的实例
```

# Node文件操作-写入
## 简单文件操作
```javascript
/*
*1. Node中的文件系统：
*     1.在NodeJs中有一个文件系统，所谓的文件系统，就是对计算机中的文件进行增删改查等操作。
*     2.在NodeJs中，给我们提供了一个模块，叫做fs模块(文件系统)，专门用于操作文件。
*     3.fs模块是Node的核心模块，使用的时候，无需下载，直接引入。
*
*2.异步文件写入 （简单文件写入）
*   fs.writeFile(file, data[, options], callback)
*           --file：要写入的文件路径+文件名+后缀
*           --data：要写入的数据
*           --options：配置对象(可选参数)
*                 --encoding:设置文件的编码方式，默认值：utf8(万国码)
*                 --mode:设置文件的操作权限，默认值是：0o666 = 0o222 + 0o444
*                      --0o111:文件可被执行的权限  .exe .msc 几乎不用，linux有自己一套操作方法。
*                      --0o222:文件可被写入的权限
*                      --0o444:文件可别读取的权限
*                 --flag:打开文件要执行的操作，默认值是'w'
*                      --a ：追加
*                      --w ：写入
*           --callback：回调函数
*                 --err：错误对象
*
*  在Node中有这样一个原则：错误优先
*/

let fs = require('fs')

fs.writeFile(__dirname + '/fs.txt', ',2.0', {mode: 0o666, flag: 'a'}, err => {
    if (err) console.log('文件写入失败', err)
    else console.log('文件写入成功')
})
```
## 流式文件写入
```javascript
/*
* 创建一个可写流
*   fs.createWriteStream(path[, options])
*       --path:要写入文件的路径+文件名+文件后缀
*       --options：配置对象（可选参数）
*           --flags:
*           --encoding :
*           --fd : 文件统一标识符，linux下文件标识符
*           --mode :
*           --autoClose : 自动关闭 --- 文件，默认值：true
*           --emitClose : 关闭 --- 文件，默认值：false
*           --start : 写入文件的起始位置(偏移量)
* */



let fs = require('fs')

//创建一个可写流
let ws = fs.createWriteStream(__dirname+'/demo.txt')

//只要用到了流，就必须监测流的状态
ws.on('open',function () {
    console.log('可写流打开了')
})
ws.on('close',function () {
    console.log('可写流关闭了')
})

//使用可写流写入数据
ws.write('1号\n')
ws.write('2号？\n')
ws.write('到底是哪一个？\n')
//ws.close() //如果在Node的8版本中，使用此方法关闭流会造成数据丢失
ws.end() //在Node的8版本中，要用end方法关闭流

```
## 追加写入
```javascript
let fs = require('fs');
let push = require('path');

let res = push.join(__dirname, "data.txt");

fs.appendFile(res, " 新加内容3.0", 'utf8', (err) => {
    if (err) throw new Error("失败");
    console.log("新增成功");
});

fs.appendFileSync(res, "666", 'utf8');
```


# Node文件操作-读取
## 简单文件读取
```javascript
/*
*  fs.readFile(path[, options], callback)
*     --path：要读取文件的路径+文件名+后缀
*     --options：配置对象（可选）
*     --callback：回调
*         --err：错误对象
*         --data：读取出来的数据
* */

//简单文件写入和简单文件读取，都是一次性把所有要读取或要写入的内容加到内存中红，容易造成内存泄露。

let fs = require('fs')

fs.readFile(__dirname+'/test.mp4',function (err,data) {
    if(err) console.log(err)
    //为什么读取出来的东西是Buffer？ 用户存储的不一定是纯文本
    else console.log(data)
    fs.writeFile('../haha.mp4',data,function (err) {
      if(err) console.log(err)
      else console.log('文件写入成功')
    })

})
```
## 流式文件读取
```javascript
/*
* fs.createReadStream(path[, options])
*     --path:尧都区的文件路径+文件名+后缀
*     --options:
*         --flags
*         --encoding
*         --fd
*         --mode
*         --autoClose
*         --emitClose
*         --start ：起始偏移量
*         --end : 结束偏移量
*         --highWaterMark：每次读取数据的大小，默认值是64 * 1024
* */

let {createReadStream,createWriteStream} = require('fs')

//创建一个可读流
let rs = createReadStream(__dirname+'/music.mp3',{
  highWaterMark:10 * 1024 * 1024,
  //start:60000,
  //end:120000
})

//创建一个可写流
let ws = createWriteStream('../haha.mp3')

//只要用到了流，就必须监测流的状态
rs.on('open',function () {
  console.log('可读流打开了')
})
rs.on('close',function () {
  console.log('可读流关闭了')
  ws.close()
})
ws.on('open',function () {
  console.log('可写流打开了')
})
ws.on('close',function () {
  console.log('可写流关闭了')
})

//给可读流绑定一个data事件，就会触发可读流自动读取内容。
rs.on('data',function (data) {
  //Buffer实例的length属性，是表示该Buffer实例占用内存空间的大小
  console.log(data.length) //输出的是65536，每次读取64KB的内容
  ws.write(data)
  //ws.close() //若在此处关闭流，会写入一次，后续数据丢失
})
//ws.close() //若在此处关闭流，导致无法写入数据
```
# Node文件操作-大文件操作
```javascript
前面讲解的关于文件写入和读取操作都是一次性将数据读入内存或者一次性写入到文件中
但是如果数据比较大, 直接将所有数据都读到内存中会导致计算机内存爆炸,卡顿,死机等
所以对于比较大的文件我们需要分批读取和写入

et fs = require('fs');
let res = require('path');

// let str = res.join(__dirname, 'data.txt');
/*
//创建一个读取流
//默认读取64kb数据
let readStream = fs.createReadStream(str, {encoding: 'utf8', highWaterMark: 1});

//添加事件监听
readStream.on("open", () => {
    console.log("数据流和文件建立关系");
});
readStream.on("error", () => {
    console.log("数据流和文件建立关系失败");
});
readStream.on("data", (data) => {
    console.log("通过读取流从文件中读取到了数据", data);
});
readStream.on("close", () => {
    console.log("数据流断开了和文件关系，并且读取到了数据");
});
*/

/*
let writeStream = fs.createWriteStream(str, {encoding: 'utf8'});

writeStream.on("open", () => {
    console.log("写入流和文件建立关系");
});
writeStream.on("error", () => {
    console.log("写入流和文件建立关系失败");
});

writeStream.on("close", () => {
    console.log("写入流断开了和文件关系");
});

let data = '963852741';
let index = 0;
let timeId = setInterval(() => {
    let ch = data[index];
    index++;
    writeStream.write(ch);
    console.log("写入了", ch);
    if (index === data.length) {
        clearInterval(timeId)
        writeStream.end()
    }
    ;

}, 1000);
*/

let readPath = res.join(__dirname, "test.jpg");
let writePath = res.join(__dirname, "aaa.jpg");

/*
let readTest = fs.createReadStream(readPath);
readTest.on("open", () => {
    console.log("数据流和文件建立关系");
});
readTest.on("error", () => {
    console.log("数据流和文件建立关系失败");
});
readTest.on("data", (data) => {
    // console.log("通过读取流从文件中读取到了数据", readTest);
    //写入数据
    writeStream.write(data);
});
readTest.on("close", () => {
    console.log("数据流断开了和文件关系，并且读取到了数据");
    //断开写入数据流
    writeStream.end();
});

let writeStream = fs.createWriteStream(writePath);

writeStream.on("open", () => {
    console.log("写入流和文件建立关系");
});
writeStream.on("error", () => {
    console.log("写入流和文件建立关系失败");
});

writeStream.on("close", () => {
    console.log("写入流断开了和文件关系");
});
*/

let readStream = fs.createReadStream(readPath);
let writeStream = fs.createWriteStream(writePath);

//利用读取流的管道方法来快速实现文件拷贝
readStream.pipe(writeStream);
```
Node文件操作-目录操作
```plain
1、创建目录
fs.mkdir(path[, mode], callback)
fs.mkdirSync(path[, mode])

2、读取目录
fs.readdir(path[, options], callback)
fs.readdirSync(path[, options])

3、删除目录
fs.rmdir(path, callback)
fs.rmdirSync(path)

---------------------------------

let fs = require('fs');
let path = require('path');
let res = path.join(__dirname, '创建新目录');
fs.mkdir(res, (err) => {
    if (err) {
        throw new Error('创建失败');
    } else {
        console.log('创建成功');
    }
});

fs.rmdir(res, (err) => {
    if (err) {
        throw new Error('删除失败');
    } else {
        console.log('删除成功');
    }
});
fs.readdir(__dirname, function (err, files) {
    if (err) {
        throw new Error("读取失败")
    } else {
        files.forEach(function (obj) {
            let filePath = path.join(__dirname, obj);
            let stats = fs.statSync(filePath);
            if (stats.isFile()){
                console.log("是一个文件",obj);
            }else if (stats.isDirectory()){
                console.log("是一个目录",obj);
            }
        })
    }
});

```


# express服务器
```javascript
let express = require('express')

let app = express()

app.get('/',function(request,response){
response.send('主页')
})

app.listen(3000,function(err){
if(!err) console.log('服务器启动成功')
else console.log(err)
})
```

```javascript
let express = require('express');
//创建app服务对象
let app = express()
//禁止服务器返回X-Powered-By
app.disable('x-powered-by')

//配置路由 对请求的urL进行分类，服务器根据分类决定交给谁去处理。
// 路由可以理解为:一组组key-value的组合，key:请求方式＋URI路径﹐value：回调函数

//根路由
app.get('/', function (request, response) {
    /*
    请求方式 get    uri 必须为 /xxx
     */
    console.log(request.query)
    response.send('来到主页')
})
//一级路由
app.get('/haixin', function (request, response) {
    response.send('海星')
})

app.post('/', function (request, response) {
    response.send('我是post请求');
})

//指定端口号
app.listen(3000, err => {
    if (!err) console.log('服务器启动成功')
    else console.log(err)
})
```

# HTTP状态码
```plain
###Http状态码（服务器给客户端的东西）

###作用：  
* 告诉客户端，当前服务器处理请求的结果
    
###http状态码的分类
 * 1xx : 服务器已经收到了本次请求，但是还需要进一步的处理才可以。
 * 2xx : 服务器已经收到了本次请求，且已经分析、处理等........最终处理完毕！
 * 3xx : 服务器已经接收到了请求，还需要其他的资源，或者重定向到其他位置，甚至交给其他服务器处理。
 * 4xx ：一般指请求的参数或者地址有错误， 出现了服务器无法理解的请求（一般是前端的锅）。
 * 5xx ：服务器内部错误（不是因为请求地址或者请求参数不当造成的），无法响应用户请求（一般是后端人员的锅）。
 
###常见的几个状态码
 * 200 ：成功（最理想状态）
 * 301 ：重定向，被请求的旧资源永久移除了（不可以访问了），将会跳转到一个新资源，搜索引擎在抓取新内容的同时也将旧的网址替换为重定向之后的网址；
 * 302 ：重定向，被请求的旧资源还在（仍然可以访问），但会临时跳转到一个新资源，搜索引擎会抓取新的内容而保存旧的网址。
 * 304 ：请求资源重定向到缓存中（命中了协商缓存）。
 * 404 ：资源未找到，一般是客户端请求了不存在的资源。
 * 500 ：服务器收到了请求，但是服务器内部产生了错误。
 * 502 ：连接服务器失败（服务器在处理一个请求的时候，或许需要其他的服务器配合，但是联系不上其他的服务器了）。
 
```
# Node原生服务器
```javascript
//1.引入Node内置的http模块
let http = require('http')
//引入一个内置模块，用于解析key=value&key=value.....这种形式的字符串为js中的对象
/*
备注：
  1.key=value&key=value.....的编码形式：urlencoded编码形式。
  2.请求地址里携带urlencoded编码形式的参数，叫做：查询字符串参数(query参数)。
* */
//引入的qs是一个对象，该对象身上有着很多有用的方法，最具代表性的：parse()
let qs = require('querystring')

//2.创建服务对象
let server = http.createServer(function (request,response) {
    /*
    * (1).request:请求对象，里面包含着客户端给服务器的“东西”
    * (2).response：响应对象，里面包含着服务器要返回给客户端的“东西”
    * */
    //获取客户端携带过来的urlencoded编码形式的参数
    let params = request.url.split('?')[1] //name=zhangsan&age=18
    //console.log(params)
    let objParams = qs.parse(params) //
    //console.log(objParams)
    let {name,age} =  objParams

    response.setHeader('content-type','text/html;charset=utf-8')
    response.end(`<h1>你好${name},你的年龄是${age}</h1>`)
})

//3.指定服务器运行的端口号(绑定端口监听)
server.listen(3000,function (err) {
    if (!err) console.log('服务器启动成功了')
    else console.log(err)
})



```


# 中间件
```javascript
/*
 中间件：
     概念：本质上就是一个函数，包含三个参数：request、response、next

 作用：
        1) 执行任何代码。
        2) 修改请求对象、响应对象。
        3) 终结请求-响应循环。(让一次请求得到响应)
        4) 调用堆栈中的下一个中间件或路由。
  分类：
        1) 应用(全局)级中间件（过滤非法的请求，例如防盗链）
              --第一种写法：app.use((request,response,next)=>{})
              --第二种写法：使用函数定义
        2) 第三方中间件，即：不是Node内置的，也不是express内置的（通过npm下载的中间件，例如body-parser）
              --app.use(bodyParser.urlencoded({extended:true}))
        3) 内置中间件（express内部封装好的中间件）
              --app.use(express.urlencoded({extended:true}))
              --app.use(express.static('public')) //暴露静态资源
        4) 路由器中间件 （Router）

   备注：
        1.在express中，定义路由和中间件的时候，根据定义的顺序（代码的顺序），将定义的每一个中间件或路由，
        放在一个类似于数组的容器中，当请求过来的时候，依次从容器中取出中间件和路由，进行匹配，如果匹配
        成功，交由该路由或中间件处理，如果全局中间件写在了最开始的位置，那么他就是请求的“第一扇门”。
        2.对于服务器来说，一次请求，只有一个请求对象，和一个响应对象，其他任何的request和response都是对二者的引用。
 */

const express = require('express')
//引入body-parser，用于解析post参数
//const bodyParser = require('body-parser')
const app = express()

//【第一种】使用应用级(全局)中间件------所有请求的第一扇门-------所有请求都要经过某些处理的时候用此种写法
/*app.use((request,response, )=>{
  //response.send('我是中间件给你的响应')
  //response.test = 1 //修改请求对象
  //图片防盗链
  if(request.get('Referer')){
    let miniReferer = request.get('Referer').split('/')[2]
    if(miniReferer === 'localhost:63347'){
      next()
    }else{
      //发生了盗链
      response.sendFile(__dirname+'/public/err.png')
    }
  }else{
    next()
  }
  //next()
})*/

//【第二种】使用全局中间件的方式------更加灵活，不是第一扇门，可以在任何需要的地方使用。
function guardPic(request,response,next) {
  //防盗链
  if(request.get('Referer')){
    let miniReferer = request.get('Referer').split('/')[2]
    if(miniReferer === 'localhost:63347'){
      next()
    }else{
      //发生了盗链
      response.sendFile(__dirname+'/public/err.png')
    }
  }else{
    next()
  }
}

//使用第三方中间件bodyParser

//解析post请求请求体中所携带的urlencoded编码形式的参数为一个对象，随后挂载到request对象上
//app.use(bodyParser.urlencoded({extended: true}))

//解析post请求请求体中所携带的urlencoded编码形式的参数为一个对象，随后挂载到request对象上
app.use(express.urlencoded({extended: true}))

//使用内置中间件去暴露静态资源 ---- 一次性把你所指定的文件夹内的资源全部交出去。
app.use(express.static(__dirname+'/public'))

app.get('/',(request,response)=>{
    console.log(request.demo)
    response.send('ok')
})

app.get('/demo',(request,response)=>{
    console.log(request.demo)
    console.log(request.query)
    response.send('ok2')
})

app.get('/picture',guardPic,(request,response)=>{
  response.sendFile(__dirname+'/public/demo.jpg')
})

app.post('/test',(request,response)=>{
  console.log(request.body)
  response.send('ok')
})



app.listen(3000,function (err) {
  if(!err) console.log('ok')
  else console.log(err)
})
```
# 路由
```javascript
let express = require('express')

let app = express()

//request和response都有什么API？
/*
  1.request对象：
      request.query    获取查询字符串参数（query参数），拿到的是一个对象
      request.params 获取get请求参数路由的参数，拿到的是一个对象
      request.body 获取post请求体参数，拿到的是一个对象（不可以直接用，要借助一个中间件）
      request.get(xxxx)    获取请求头中指定key对应的value。
  2.response对象：
      response.send()  给浏览器做出一个响应
      response.end()   给浏览器做出一个响应（不会自动追加响应头）
      response.download()  告诉浏览器下载一个文件，可以传递相对路径
      response.sendFile()  给浏览器发送一个文件 备注：必须传递绝对路径
      response.redirect()  重定向到一个新的地址（url）
      response.set(key,value)  自定义响应头内容
      response.get(key)    获取响应头指定key对应的value  很少使用
      response.status(code)    设置响应状态码
 */

//1.过滤掉非法请求、带有攻击性的请求
//2.在匹配路由之前批量处理request
//3.防盗链

//根路由
app.get('/',function (request,response) {
  //.log(request.query)
  //console.log(request.get('Host'))
  //console.log(request.get('Referer'))
  response.send('250') //send方法里不能传入纯数字，express会当成状态码
})

//根路由
app.post('/',function (request,response) {
  console.log(request.body)
  response.send('我是根路由返回的数据--post')
})

//一级路由
app.get('/demo',function (request,response) {
  /*
  * 什么叫做服务器给浏览器响应了？
  *   1.服务器给浏览器一段文字
  *   2.服务器给了浏览器一个图片
  *   3.服务器给了浏览器一个视频
  *   4.服务器告诉浏览器下载一个文件
  *   5.服务器告诉浏览器重定向
  *   备注：多个响应以response.send为主
  * */
  //response.download('./public/vue.png')
  //response.sendFile(__dirname+'/public/demo.zip')
  //response.redirect('https://www.baidu.com')
  //response.redirect('/demo/test')
  //response.set('demo','0719')
  //response.status(200)
  response.send('我是demo路由返回的数据')
  //console.log(response.get('demo'));
  //response.send('等等我还有点东西') //会抛一个异常，不能send两次
})

//二级路由
app.get('/demo/test',function (request,response) {
  response.send('我是demo/test路由返回的数据')
})

//参数路由---可以动态接收参数
app.get('/meishi/:id',function (request,response) {
  console.log(request.params);
  let {id} = request.params
  response.send(`我是变化为${id}的商家`)
})



app.listen(3000,function (err) {
  if(!err) console.log('ok')
  else console.log(err)
})
```
# Cookie
```plain

* 关于cookie:
*     1.是什么？
*         本质就是一个【字符串】，里面包含着浏览器和服务器沟通的信息（交互时产生的信息）。
*         存储的形式以：【key-value】的形式存储。
*         浏览器会自动携带该网站的cookie，只要是该网站下的cookie，全部携带。
*     2.分类：
*           --会话cookie（关闭浏览器后，会话cookie会自动消失，会话cookie存储在浏览器运行的那块【内存】上）。
*           --持久化cookie：（看过期时间，一旦到了过期时间，自动销毁，存储在用户的硬盘上,备注：如果没有到过期时间，同时用户清理了浏览器的缓存，持久化cookie也会消失）。
*
*     3.工作原理：
*           --当浏览器第一次请求服务器的时候，服务器可能返回一个或多个cookie给浏览器
*           --浏览器判断cookie种类
*               --会话cookie：存储在浏览器运行的那块内存上
*               --持久化cookie：存储在用户的硬盘上
*           --以后请求该网站的时候，自动携带上该网站的所有cookie（无法进行干预）
*           --服务器拿到之前自己“种”下cookie，分析里面的内容，校验cookie的合法性，根据cookie里保存的内容，进行具体的业务逻辑。
*
*      4.应用：
*           解决http无状态的问题（例子：7天免登录，一般来说不会单独使用cookie，一般配合后台的session存储使用）
*
*      5.不同的语言、不同的后端架构cookie的具体语法是不一样的，但是cookie原理和工作过程是不变的。
*         备注：cookie不一定只由服务器生成，前端同样可以生成cookie，但是前端生成的cookie几乎没有意义。



```


# ejs模板
```javascript
const express = require('express')

const app = express()
//让你的服务器知道你在用哪一个模板引擎-----配置模板引擎
app.set('view engine','ejs')
//让你的服务器知道你的模板在哪个目录下，配置模板目录
app.set('views','./public')

//如果在express中基于Node搭建的服务器，使用ejs无需引入。
app.get('/show',function (request,response) {
    let personArr = [
        {name:'peiqi',age:4},
        {name:'suxi',age:5},
        {name:'peideluo',age:6}
    ]
    response.render('person',{persons:personArr})
})

app.listen(3000,function (err) {
    if (!err) console.log('服务器启动成功了')
    else console.log(err)
})
```
# 模块原理分析
```javascript
//存在依赖关系，字符串可以访问外界数据，不安全
/*
let str = 'enenen'
let name = 'console.log(str)'
eval(name)

let str1 = console.log('enenen')
let fn = Function(str1)
fn()
*/

let vm = require("vm")

/*
global.name = 'jj'
//runInThisContext : 提供了一个安全的环境，给我们执行字符串中的代码
//runInThisContext 不能访问本地变量，但是可以访问 全局global 里的变量

let name = 'lj'
let str = "console.log(name)"
vm.runInThisContext(str) //name is not defined
*/



global.name = 'jj' //name is not defined
let name = 'lj'
let str = "console.log(name)"
vm.runInNewContext(str) //name is not defined

//runInNewContext : 提供了一个安全的环境，给我们执行字符串中的代码
//runInNewContext 不能访问本地变量，也不能访问 全局global 里的变量



/*
1.Node模块
1.1在CommonJS规范中一个文件就是一个模块
1.2在CommonJS规范中通过exports暴露数据
1.3在CommonJS规范中通过require()导入模块

2.Node模块原理分析
既然一个文件就是一个模块,
既然想要使用模块必须先通过require()导入模块
所以可以推断出require()的作用其实就是读取文件
所以要想了解Node是如何实现模块的, 必须先了解如何执行读取到的代码

3.执行从文件中读取代码
我们都知道通过fs模块可以读取文件,
但是读取到的数据要么是二进制, 要么是字符串
无论是二进制还是字符串都无法直接执行

但是我们知道如果是字符串, 在JS中是有办法让它执行的
eval  或者 new Function;

4.通过eval执行代码
缺点: 存在依赖关系, 字符串可以访问外界数据,不安全

5.通过new Function执行代码
缺点: 存在依赖关系, 依然可以访问全局数据,不安全
-->
<!--
6.通过NodeJS的vm虚拟机执行代码
runInThisContext: 无权访问外部变量, 但是可以访问global
runInNewContext:  无权访问外部变量, 也不能访问global
 */
```

# Node模块加载流程分析
```plain
/*
1.内部实现了一个require方法
    function require(path) {
      return self.require(path);
    }

2.通过Module对象的静态__load方法加载模块文件
    Module.prototype.require = function(path) {
      return Module._load(path, this, isMain  false);
    };

3.通过Module对象的静态_resolveFilename方法, 得到绝对路径并添加后缀名
    var filename = Module._resolveFilename(request, parent, isMain);

4.根据路径判断是否有缓存, 如果没有就创建一个新的Module模块对象并缓存起来
    var cachedModule = Module._cache[filename];
    if (cachedModule) {
        return cachedModule.exports;
    }
    var module = new Module(filename, parent);
    Module._cache[filename] = module;

    function Module(id, parent) {
        this.id = id;
        this.exports = {};
    }
5.利用tryModuleLoad方法加载模块
    tryModuleLoad(module, filename);
- 6.1取出模块后缀
    var extension = path.extname(filename);

- 6.2根据不同后缀查找不同方法并执行对应的方法, 加载模块
    Module._extensions[extension](this, filename);

- 6.3如果是JSON就转换成对象
    module.exports = JSON.parse(internalModule.stripBOM(content));

- 6.4如果是JS就包裹一个函数
    var wrapper = Module.wrap(content);
    NativeModule.wrap = function(script) {
        return NativeModule.wrapper[0] + script + NativeModule.wrapper[1];
    };
    NativeModule.wrapper = [
        '(function (exports, require, module, __filename, __dirname) { ',
        '\n});'
    ];
- 6.5执行包裹函数之后的代码, 拿到执行结果(String -- Function)
    var compiledWrapper = vm.runInThisContext(wrapper);

- 6.6利用call执行fn函数, 修改module.exports的值
    var args = [this.exports, require, module, filename, dirname];
    var result = compiledWrapper.call(this.exports, args);

- 6.7返回module.exports
    return module.exports;
*/

```
# 浏览器事件环
```plain
1.JS是单线程的
  JS中的代码都是串行的, 前面没有执行完毕后面不能执行

2.执行顺序
2.1程序运行会从上至下依次执行所有的同步代码
2.2在执行的过程中如果遇到异步代码会将异步代码放到事件循环中
2.3当所有同步代码都执行完毕后, JS会不断检测 事件循环中的异步代码是否满足条件
2.4一旦满足条件就执行满足条件的异步代码

3.宏任务和微任务
在JS的异步代码中又区分"宏任务(MacroTask)"和"微任务(MicroTask)"
宏任务: 宏/大的意思, 可以理解为比较费时比较慢的任务
微任务: 微/小的意思, 可以理解为相对没那么费时没那么慢的任务

4.常见的宏任务和微任务
MacroTask: setTimeout, setInterval, setImmediate（IE独有）...
MicroTask: Promise, MutationObserver ,process.nextTick（node独有) ...
注意点: 所有的宏任务和微任务都会放到自己的执行队列中, 也就是有一个宏任务队列和一个微任务队列
        所有放到队列中的任务都采用"先进先出原则", 也就是多个任务同时满足条件, 那么会先执行先放进去的

5.完整执行顺序
1.从上至下执行所有同步代码
2.在执行过程中遇到宏任务就放到宏任务队列中,遇到微任务就放到微任务队列中
3.当所有同步代码执行完毕之后, 就执行微任务队列中满足需求所有回调
4.当微任务队列所有满足需求回调执行完毕之后, 就执行宏任务队列中满足需求所有回调
... ...
注意点:
每执行完一个宏任务都会立刻检查微任务队列有没有被清空, 如果没有就立刻执行清空
```

# Node.js事件环
```plain
1.概述
和浏览器中一样NodeJS中也有事件环(Event Loop),
但是由于执行代码的宿主环境和应用场景不同,
所以两者的事件环也有所不同.

>扩展阅读: 在NodeJS中使用libuv实现了Event Loop.
>源码地址: https://github.com/libuv/libuv

2.NodeJS事件环和浏览器事件环区别
2.1任务队列个数不同
浏览器事件环有2个事件队列(宏任务队列和微任务队列)
NodeJS事件环有6个事件队列
2.2微任务队列不同
浏览器事件环中有专门存储微任务的队列
NodeJS事件环中没有专门存储微任务的队列
2.3微任务执行时机不同
浏览器事件环中每执行完一个宏任务都会去清空微任务队列
NodeJS事件环中只有同步代码执行完毕和其它队列之间切换的时候会去清空微任务队列
2.4微任务优先级不同
浏览器事件环中如果多个微任务同时满足执行条件, 采用先进先出
NodeJS事件环中如果多个微任务同时满足执行条件, 会按照优先级执行
process.nextTick() 能在任意阶段优先执行(不包括主线程)

2.NodeJS中的任务队列
    ┌───────────────────────┐
┌> │timers          │执行setTimeout() 和 setInterval()中到期的callback
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │pending callbacks│执行系统操作的回调, 如:tcp, udp通信的错误callback
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │idle, prepare   │只在内部使用
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │poll            │执行与I/O相关的回调
    │                  (除了close回调、定时器回调和setImmediate()之外，几乎所有回调都执行);
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │check           │执行setImmediate的callback
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
└─┤close callbacks │执行close事件的callback，例如socket.on("close",func)
    └───────────────────────┘



    ┌───────────────────────┐
┌> │timers          │执行setTimeout() 和 setInterval()中到期的callback
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │poll            │执行与I/O相关的回调
    │                  (除了close回调、定时器回调和setImmediate()之外，几乎所有回调都执行);
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
└─┤check           │执行setImmediate的callback
    └───────────────────────┘

1.注意点:
和浏览器不同的是没有宏任务队列和微任务队列的概念
宏任务被放到了不同的队列中, 但是没有队列是存放微任务的队列
微任务会在执行完同步代码和队列切换的时候执行

什么时候切换队列?
当队列为空(已经执行完毕或者没有满足条件回到)
或者执行的回调函数数量到达系统设定的阈值时任务队列就会切换

2.注意点:
在NodeJS中process.nextTick微任务的优先级高于Promise.resolve微任务
-->
<!--
            ┌───────────────────────┐
            │                同步代码
            └──────────┬────────────┘
                                  │
                                  │
                                  │
            ┌──────────┴────────────┐
        ┌> │timers          │执行setTimeout() 和 setInterval()中到期的callback
        │  └──────────┬────────────┘
        │                        │
        │                        │ <---- 满足条件微任务代码
        │                        │
        │  ┌──────────┴────────────┐
        │  │poll            │执行与I/O相关的回调
        │  │                  (除了close回调、定时器回调和setImmediate()之外，几乎所有回调都执行);
        │  └──────────┬────────────┘
        │                        │
        │                        │ <---- 满足条件微任务代码
        │                        │
        │  ┌──────────┴────────────┐
        └─┤check           │执行setImmediate的callback
            └───────────────────────┘

注意点:
执行完poll, 会查看check队列是否有内容, 有就切换到check
如果check队列没有内容, 就会查看timers是否有内容, 有就切换到timers
如果check队列和timers队列都没有内容, 为了避免资源浪费就会阻塞在poll

——————————————————————————————————————————————————————————————

第一个阶段: timers(定时器阶段--setTimeout,setInterval)
            1.开始记时
            2.执行定时器的回调

第二个阶段: pending callbacks(系统阶段)

第三个阶段: idle,prepare(准备阶段)

第四个阶段: poLl(轮询阶段，核心)
            ---.如果回调队列里有待执行的回调函数
                从回调队列中取出回调函数，同步执行，直到回调队列为空，或者达到系统最大限度
            ---.如果回调队列为空
                设置 setImmediate()，进入下一个check阶段，为了执行setImmediate所设置的回调
                未设置 setImmediate()，则事件循环将等待回调被添加到队列中，然后立即执行。
               1.JS是单线程的
  JS中的代码都是串行的, 前面没有执行完毕后面不能执行

2.执行顺序
2.1程序运行会从上至下依次执行所有的同步代码
2.2在执行的过程中如果遇到异步代码会将异步代码放到事件循环中
2.3当所有同步代码都执行完毕后, JS会不断检测 事件循环中的异步代码是否满足条件
2.4一旦满足条件就执行满足条件的异步代码

3.宏任务和微任务
在JS的异步代码中又区分"宏任务(MacroTask)"和"微任务(MicroTask)"
宏任务: 宏/大的意思, 可以理解为比较费时比较慢的任务
微任务: 微/小的意思, 可以理解为相对没那么费时没那么慢的任务

4.常见的宏任务和微任务
MacroTask: setTimeout, setInterval, setImmediate（IE独有）...
MicroTask: Promise, MutationObserver ,process.nextTick（node独有) ...
注意点: 所有的宏任务和微任务都会放到自己的执行队列中, 也就是有一个宏任务队列和一个微任务队列
        所有放到队列中的任务都采用"先进先出原则", 也就是多个任务同时满足条件, 那么会先执行先放进去的

5.完整执行顺序
1.从上至下执行所有同步代码
2.在执行过程中遇到宏任务就放到宏任务队列中,遇到微任务就放到微任务队列中
3.当所有同步代码执行完毕之后, 就执行微任务队列中满足需求所有回调
4.当微任务队列所有满足需求回调执行完毕之后, 就执行宏任务队列中满足需求所有回调
... ...
注意点:
每执行完一个宏任务都会立刻检查微任务队列有没有被清空, 如果没有就立刻执行清空
```
![图片](data:image/png;base64,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fLZt28Z3v/tdUqkU7e3tfPnLX+app57irW99K3v37uWSSy6htbWVaDTKrbfeypYtW+js7KRSqfAP//APdHZ20t7ezg033MDGjRtZvnw5/f39XH311ZTLZebPn8/GjRv54he/yOmnn47Wmi984QtMT0/T2dnJ5s2bue6662hsbGTevHlcfPHFPPzww5x44on09/ezYcMGWltbicfj3HTTTWzbto3W1lZm/N3f/R3pdJru7m5uuukmNm3axPLly+nr6+Pqq69GRMhms9xzzz1s2LCBt73tbcz4yle+wvT0NJ2dndxzzz1cd911NDc3k8lk+PznP8/mzZtZt24du3fvZsOGDbS2tpJMJrnuuut46qmnaGlpwfM8LrroIlzXZcGCBfzwhz9k06ZNLFu2jIGBAa666ioikQipVIqHH36Yb37zmxx33HEopdiwYQPT09N0d3ezceNGrrnmGnp6eojFYnz961/ngQceYNWqVezYsYN/+Zd/oa2tjYaGBq644gq2b9/OvHnzUErxz//8z9RqNRYsWMCtt97Kxo0bWbJkCePj4/zrv/4rDQ0NRKNRHn/8ca666ip6e3txHIdvfOMbFItFurq62LhxI9dccw3HHXccQRDwne98h4ceeoilS5eyY8cONmzYQHt7O01NTVxyySVs376dzs5OisUiX//615menqanp4dbb72VO+64g3PPPZdTTz2VXC6H53mICIdCzHOwrKNQPp9nYGCASqXCq9Fa88ADD/Dgpp/zlx/6OMu7F3Gs6RsZ4GMX/xUX/dH/4JQVazgWnfWX5/Op913Au086g7AX4ljzF1+/iK6Wds4/8/dpbsxyrPm3H36PB/b8kve97310dnZyKFzXpaWlhVwuh2W9qYyByUl44AGoVkEpXjcRiEapv3UNj9ca2DIAgeGIYoyhODnCji130tS+kOae43C8MOX8BA/efBk9b30n27fcyVvP/RiJxhaM1jx93w8RpWhfto6dWzdSqxTpeevpGB3w8+u/Qio3HxFB+3V6172LjuNOxg1F8OtVnvjPG4jEG+hcuR4vmgBjGN39BH2P3YsBJvY+S+eK9Yz3PUXvunOYGu6jUpzCDYXJj/Tx1nf/CaXJUZ7dfBtdq95BZXqSvb+8j1AswUT/Dla/6/+l/8nNpFu66Vh+Esr1KE2O8tCPLqc8OU7HcSex4IQz8cJRxvc+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)


