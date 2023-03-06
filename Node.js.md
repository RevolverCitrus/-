# Node.js

## 简介

    - 运行在服务器端的js
    - 用来编写服务器
    - 特点：
        - 单线程、异步、非阻塞
        - 统一API
    
    nvm
        - 命令
            nvm list - 显示已安装的node版本
            nvm install 版本 - 安装指定版本的node
            配置nvm的镜像服务器
                nvm node_mirror https://npmmirror.com/mirrors/node/
            nvm use 版本 - 指定要使用的node版本
    
    node.js和JavaScript有什么区别
        ECMAScript（node有） DOM（node没有） BOM（node没有）

## ==同步与异步==

###     进程和线程

        - 进程（厂房）
            - 程序的运行的环境
        - 线程（工人）
            - 线程是实际进行运算的东西

###     同步

        - 通常情况代码都是自上向下一行一行执行的
        - 前边的代码不执行后边的代码也不会执行
        - 同步的代码执行会出现阻塞的情况
        - 一行代码执行慢会影响到整个程序的执行
   - 现实生活
     1.点菜
     2.厨师做菜
     3.吃

​    解决同步问题：
​        - java python
​            - 通过多线程来解决
​        - node.js
​            - 通过异步方式来解决

###     异步

        - 一段代码的执行不会影响到其他的程序
        - 异步的问题：
            异步的代码无法通过return来设置返回值
        - 特点：
            1.不会阻塞其他代码的执行
            2.需要通过回调函数来返回结果
        - 基于回调函数的异步带来的问题
            1. 代码的可读性差
            2. 可调试性差
        - 解决问题：
            - 需要一个东西，可以代替回调函数来给我们返回结果
            - Promise横空出世
                - Promise是一个可以用来存储数据的对象
                    Promise存储数据的方式比较特殊，
                    这种特殊方式使得Promise可以用来存储异步调用的数据

## ==模块化==

```javascript
/* 
    早期的网页中，是没有一个实质的模块规范的
        我们实现模块化的方式，就是最原始的通过script标签来引入多个js文件
        问题：
            1. 无法选择要引入模块的哪些内容
            2. 在复杂的模块场景下非常容易出错
            ......
        于是，我们就继续在js中引入一个模块化的解决方案

    在node中，默认支持的模块化规范叫做CommonJS，
        在CommonJS中，一个js文件就是一个模块

    CommonJS规范
        - 引入模块
            - 使用require("模块的路径")函数来引入模块
            - 引入自定义模块时
                - 模块名要以./ 或 ../开头
                - 扩展名可以省略
                    - 在CommonJS中，如果省略的js文件的扩展名
                        node，会自动为文件补全扩展名
                            ./m1.js 如果没有js 它会寻找 ./m1.json
                            js --> json --> node（特殊）
            - 引入核心模块时
                - 直接写核心模块的名字即可
                - 也可以在核心模块前添加 node:
*/
```

```javascript
/* 
    默认情况下，node中的模块化标准是CommonJS
        要想使用ES的模块化，可以采用以下两种方案
            1. 使用mjs作为扩展名
            2. 修改package.json将模块化规范设置为ES模块
                当我们设置 "type": "module" 当前项目下所有的js文件都默认为es module
*/
// console.log(module)

// 导入m4模块，es模块不能省略扩展名（官方标准）
// import { a, b, c } from "./m4.mjs"

// 通过as来指定别名
// import { a as hello, b, c } from "./m4.mjs"

// 开发时要尽量避免import * 情况
// import * as m4 from "./m4.mjs"

// console.log(m4.c)

// 导入模块的默认导出
// 默认导出的内容，可以随意命名
// import sum, { a } from "./m4.mjs"

// console.log(sum, a)

import { a, b, c } from "./m4.mjs"

// 通过ES模块化，导入的内容都是常量
// es模块都是运行在严格模式下的
// ES模块化，在浏览器中同样支持，但是通常我们不会直接使用
//          通常都会结合打包工具使用
console.log(c)

c.name = "沙和尚"

console.log(c)
```

### 核心模块

```javascript
// 核心模块，是node中自带的模块，可以在node中直接使用
// window 是浏览器的宿主对象 node中是没有的
// global 是node中的全局对象，作用类似于window
// ES标准下，全局对象的标准名应该是 globalThis

// console.log(globalThis)


/* 
    核心模块
        process 
            - 表示当前的node进程
            - 通过该对象可以获取进程的信息，或者对进程做各种操作
            - 如何使用
                1. process是一个全局变量，可以直接使用
                2. 有哪些属性和方法：
                    process.exit()
                        - 结束当前进程，终止node
                    process.nextTick(callback[, …args])
                        - 将函数插入到 tick队列中
                        - tick队列中的代码，会在下一次事件循环之前执行
                            会在微任务队列和宏任务队列中任务之前执行
                    
                调用栈
                tick队列
                微任务队列
                宏任务队列

*/
```

### 自定义模块

```javascript
/* 
    在定义模块时，模块中的内容默认是不能被外部看到的
        可以通过exports来设置要向外部暴露的内容

    访问exports的方式有两种：
        exports
        module.exports
        - 当我们在其他模块中引入当前模块时，require函数返回的就是exports
        - 可以将希望暴露给外部模块的内容设置为exports的属性
*/
// console.log(exports)
// console.log(module.exports)

// 随意的编写代码


// 可以通过exports 一个一个的导出值
// exports.a = "孙悟空"
// exports.b = {name:"白骨精"}
// exports.c = function fn(){
//     console.log("哈哈")
// }

// 也可以直接通过module.exports同时导出多个值
module.exports = {
    a: "哈哈",
    b: [1, 3, 5, 7],
    c: () =>{
        console.log(111)
    }
}
```

### ES模块化

```javascript
/* 
    ES 模块化
*/

// 向外部导出内容
export let a = 10
export const b = "孙悟空"
export const c = { name: "猪八戒" }

// 设置默认导出， 一个模块中只有一个默认导出
// export default function sum(a, b) {
//     return a + b
// }
let d = 20
export default d 
```

## 核心模块

### path

```javascript
/*
    path
        - 表示的路径
        - 通过path可以用来获取各种路径
        - 要使用path，需要先对其进行引入
        - 方法：
            path.resolve([…paths]) 
                - 用来生成一个绝对路径
                    相对路径：./xxx  ../xxx  xxx
                    绝对路径：
                        - 在计算机本地
                            c:\xxx
                            /User/xxxx
                        - 在网络中
                            http://www.xxxxx/...
                            https://www.xxx/...

                    - 如果直接调用resolve，则返回当前的工作目录

                        C:\Program Files\nodejs\node.exe .\03_包管理器\01_path.js
                        c:\Users\lilichao\Desktop\Node-Course

                        node .\01_path.js
                        C:\Users\lilichao\Desktop\Node-Course\03_包管理器
                        - 注意，我们通过不同的方式执行代码时，它的工作目录是有可能发生变化的

                - 如果将一个相对路径作为参数，
                    则resolve会自动将其转换为绝对路径
                    此时根据工作目录的不同，它所产生的绝对路径也不同

                - 一般会将一个绝对路径作为第一个参数，
                    一个相对路径作为第二个参数
                    这样它会自动计算出最终的路径

*/
const path = require("node:path")

// const result = path.resolve()


// c:\Users\lilichao\Desktop\Node-Course\hello.js
// const result = path.resolve("./hello.js")
// const result = path.resolve(
//     "C:\\Users\\lilichao\\Desktop\\Node-Course\\03_包管理器",
//     "../../hello.js")


// 最终形态
// 以后在使用路径时，尽量通过path.resolve()来生成路径
// const result = path.resolve(__dirname, "./hello.js")
// console.log(result)


/* 
    fs （File System）
        - fs用来帮助node来操作磁盘中的文件
        - 文件操作也就是所谓的I/O，input output
        - 使用fs模块，同样需要引入
*/

const fs = require("node:fs/promises")
const { buffer } = require("stream/consumers")

    /* 
        Promise版本的fs的方法
    */
    // fs.readFile(path.resolve(__dirname, "./hello.txt"))
    //     .then(buffer => {
    //         console.log(buffer.toString())
    //     })
    //     .catch(e => {
    //         console.log("出错了~")
    //     })

    ; (async () => {

        try {
            const buffer = await fs.readFile(path.resolve(__dirname, "./hello.txt"))
            console.log(buffer.toString())
        } catch (e) {
            console.log("出错了~~")
        }


    })()

//readFileSync() 同步的读取文件的方法，会阻塞后边代码的执行
// 当我们通过fs模块读取磁盘中的数据时，读取到的数据总会以Buffer对象的形式返回
// Buffer是一个临时用来存储数据的缓冲区
// const buf = fs.readFileSync(path.resolve(__dirname, "./hello.txt"))
// console.log(buf.toString())

// readFile() 异步的读取文件的方法
// fs.readFile(
//     path.resolve(__dirname, "./hello.txt"),
//     (err, buffer) => {

//         if (err) {
//             console.log("出错了~")
//         } else {
//             console.log(buffer.toString())
//         }
//     }
// )
```

### fs

```javascript
const fs = require("node:fs/promises")
const path = require("node:path")

/* 
    fs.readFile() 读取文件
    fs.appendFile() 创建新文件，或将数据添加到已有文件中
    fs.mkdir() 创建目录
    fs.rmdir() 删除目录
    fs.rm() 删除文件
    fs.rename() 重命名
    fs.copyFile() 复制文件
*/

// fs.appendFile(
//     path.resolve(__dirname, "./hello123.txt"),
//     "超哥讲的真不错"
// ).then(r => {
//     console.log("添加成功")
// })

// 复制一个文件
// C:\Users\lilichao\Desktop\图片\jpg\an.jpg
fs.readFile("C:\\Users\\lilichao\\Desktop\\图片\\jpg\\an.jpg")
    .then(buffer => {

        return fs.appendFile(
            path.resolve(__dirname, "./haha.jpg"),
            buffer
        )
    })
    .then(() => {
        console.log("操作结束")
    })


fs.mkdir()
```

```javascript
const fs = require("node:fs/promises")
const path = require("node:path")

/*
    fs.readFile() 读取文件
    fs.appendFile() 创建新文件，或将数据添加到已有文件中
    fs.mkdir() 创建目录
    fs.rmdir() 删除目录
    fs.rm() 删除文件
    fs.rename() 重命名 (剪切)
    fs.copyFile() 复制文件（复制）
*/

/*
    mkdir可以接收一个 配置对象作为第二个参数，
        通过该对象可以对方法的功能进行配置
            recursive 默认值为false
                - 设置true以后，会自动创建不存在的上一级目录
*/
// fs.mkdir(path.resolve(__dirname, "./hello/abc"), { recursive: true })
//     .then(r => {
//         console.log("操作成功~")
//     })
//     .catch(err => {
//         console.log("创建失败", err)
//     })

// fs.rmdir(path.resolve(__dirname, "./hello"), { recursive: true })
//     .then(r => {
//         console.log("删除成功")
//     })

fs.rename(
    path.resolve(__dirname, "../an.jpg"),
    path.resolve(__dirname, "./an.jpg")
).then(r => {
    console.log("重命名成功")
})
```

## 包管理器

### ==npm==

```javascript
/* 
    package.json
        - package.json是包的描述文件
        - node中通过该文件对项目进行描述
        - 每一个node项目必须有package.json

    命令
        npm init 初始化项目，创建package.json文件（需要回答问题）
        npm init -y 初始化项目，创建package.json文件（所有值都采用默认值）
        npm install 包名 将指定包下载到当前项目中
            install时发生了什么？
                ① 将包下载当前项目的node_modules目录下
                ② 会在package.json的dependencies属性中添加一个新属性
                    "lodash": "^4.17.21"
                ③ 会自动添加package-lock.json文件
                    帮助加速npm下载的，不用动他

                

        npm install 自动安装所有依赖

        npm install 包名 -g 全局安装
            - 全局安装是将包安装到计算机中
            - 全局安装的通常都是一些工具

        npm uninstall 包名 卸载   
        
        https://docs.npmjs.com/cli/v8/commands
*/

/* 
    引入从npm下载的包时，不需要书写路径，直接写包名即可
*/
const _ = require("lodash")
// console.log(_)

```

```javascript
/* 
    package.json
        scripts:
            - 可以自定义一些命令
            - 定义以后可以直接通过npm来执行这些命令
            - start 和 test 可以直接通过 npm start npm test执行
            - 其他命令需要通过npm run xxx 执行

    npm镜像
        - npm的仓管的服务器位于国外，有时候并不是那么的好使
        - 为了解决这个问题，可以在npm中配置一个镜像服务器
        - 镜像的配置：
            ① 在系统中安装cnpm（我自己不太推荐大家使用）
                npm install -g cnpm --registry=https://registry.npmmirror.com
            ② 彻底修改npm仓库地址（我个人习惯的使用方式）
                npm set registry https://registry.npmmirror.com
                - 还原到原版仓库
                npm config delete registry
*/
```

## http协议

```javascript
HTTP协议
    - 网络基础
    - 网络的服务器基于请求和响应的
     

        https:// 协议名  http ftp ...

        lilichao.com 域名 domain
            整个网络中存在着无数个服务器，每一个我服务器都有它自己的唯一标识
                这个标识被称为 ip地址 192.168.1.17，但是ip地址不方便记忆
                域名就相当于是ip地址的别名

        /hello/index.html
            网站资源路径

    1.当在浏览器中输入地址以后发生了什么？
        https://lilichao.com/hello/index.html

        ① DNS解析，获取网站的ip地址
        ② 浏览器需要和服务器建立连接（tcp/ip）（三次握手）
        ③ 向服务器发送请求（http协议）
        ④ 服务器处理请求，并返回响应（http协议）
        ⑤ 浏览器将响应的页面渲染
        ⑥ 断开和服务器的连接（四次挥手）

    2. 客户端如何和服务器建立（断开）连接
        - 通过三次握手和四次挥手
            - 三次握手（建立连接）
                - 三次握手是客户端和服务器建立连接的过程
                    1. 客户端向服务器发送连接请求
                                    SYN
                    2. 服务器收到连接请求，向客户端返回消息
                                    SYN ACK 
                    3. 客户端向服务器发送同意连接的信息
                                    ACK

            - 四次挥手（断开连接）
                    1. 客户端向服务器发送请求，通过之服务器数据发送完毕，请求断开来接
                                    FIN
                    2. 服务器向客户端返回数据，知道了
                                    ACK
                    3. 服务器向客户端返回数据，收完了，可以断开连接
                                    FIN ACK
                    4. 客户端向服务器发数据，可以断开了
                                    ACK

        请求和响应实际上就是一段数据，只是这段数据需要遵循一个特殊的格式，
            这个特殊的格式由HTTP协议来规定

TCP/IP 协议族（了解）
    - TCP/IP协议族中包含了一组协议
        这组协议规定了互联网中所有的通信的细节
    - 网络通信的过程由四层组成
        应用层
            - 软件的层面，浏览器 服务器都属于应用层
        传输层
            - 负责对数据进行拆分，把大数据拆分为一个一个小包
        网络层
            - 负责给数据包，添加信息
        数据链路层
            - 传输信息

    - HTTP协议就是应用层的协议，
        用来规定客户端和服务器间通信的报文格式的
    - 报文（message）
        - 浏览器和服务器之间通信是基于请求和响应的
            - 浏览器向服务器发送请求（request）
            - 服务器向浏览器返回响应（response）
            - 浏览器向服务器发送请求相当于浏览器给服务器写信
                服务器向浏览器返回响应，相当于服务器给浏览器回信
                这个信在HTTP协议中就被称为报文
            - 而HTTP协议就是对这个报文的格式进行规定

        - 服务器
            - 一个服务器的主要功能：
                1.可以接收到浏览器发送的请求报文
                2.可以向浏览器返回响应报文

        - 请求报文（request）
            - 客户端发送给服务器的报文称为请求报文
            - 请求报文的格式如下：
                请求首行
                请求头
                空行
                请求体

                请求首行
                    - 请求首行就是请求报文的第一行
                        GET /index.html?username=sunwukong HTTP/1.1
                        第一部分 get 表示请求的方式，get表示发送的是get请求
                            现在常用的方式就是get和post请求
                            get请求主要用来向服务器请求资源
                            post请求主要用来向服务器发送数据

                        第二部分 /index.html?username=sunwukong
                            表示请求资源的路径，
                                ? 后边的内容叫做查询字符串
                                查询字符串是一个名值对结构，一个名字对应一个值
                                    使用=连接，多个名值对之间使用&分割
                                    username=admin&password=123123
                                get请求通过查询字符串将数据发送给服务器
                                    由于查询字符串会在浏览器地址栏中直接显示
                                        所以，它安全性较差
                                        同时，由于url地址长度有限制，所以get请求无法发送较大的数据

                                post请求通过请求体来发送数据
                                    - 在chrome中通过 载荷 可以查看
                                    post请求通过请求体发送数据，无法在地址栏直接查看
                                        所以安全性较好
                                    请求体的大小没有限制，可以发送任意大小的数据

                                如果你需要向服务器发送数据，能用post尽量使用post

                        第三部分
                            HTTP/1.1 协议的版本

                请求头
                    - 请求头也是名值对结构，用来告诉服务器我们浏览器的信息
                    - 每一个请求头都有它的作用：
                        Accept 浏览器可以接受的文件类型
                        Accept-Encoding 浏览器允许的压缩的编码
                        User-Agent 用户代理，它是一段用来描述浏览器信息的字符串

                空行
                    - 用来分隔请求头和请求体

                请求体
                    - post请求通过请求体来发送数据


            网页、css、 js、图片这些资源会作为响应报文中的哪一部分发送

            响应报文：
                响应首行
                响应头
                空行
                响应体

                响应首行
                    HTTP/1.1 200 OK
                        200 响应状态码
                        ok 对响应状态码的描述
                        - 响应状态码的规则
                            1xx 请求处理中
                            2xx 表示成功
                            3xx 表示请求的重定向
                            4xx 表示客户端错误
                            5xx 表示服务器的错误

                响应头
                    - 响应头也是一个一个的名值对结构，用来告诉浏览器响应的信息
                    - Content-Type 用来描述响应体的类型
                    - Content-Length 用来描述响应体大小
                    Content-Type: text/html; charset=UTF-8
                    Content-Length: 2017
                空行
                    - 空行用来分隔响应头和响应体

                响应体
                    - 响应体就是服务器返回给客户端的内容

                <!DOCTYPE html>
                    <html lang="zh">
                        <head>
                            <meta charset="UTF-8" />
                            <meta http-equiv="X-UA-Compatible" content="IE=edge" />
                            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
                            <title>Document</title>
                        </head>
                        <body>
                            <form method="post" action="./02_target.html">
                                <input type="text" name="username" />
                                <input type="password" name="password" />
                                <button>提交</button>
                            </form>
                    </body>
                    </html>


            响应报文：
                HTTP/1.1 200 OK
                Vary: Origin
                Access-Control-Allow-Credentials: true
                Accept-Ranges: bytes
                Cache-Control: public, max-age=0
                Last-Modified: Thu, 27 Oct 2022 11:31:54 GMT
                ETag: W/"20c-18419368696"
                Content-Type: text/html; charset=UTF-8
                Content-Length: 2017
                Date: Thu, 27 Oct 2022 11:52:29 GMT
                Connection: keep-alive
                Keep-Alive: timeout=5


            请求报文：  
            GET /07_http%E5%8D%8F%E8%AE%AE/01_http%E5%8D%8F%E8%AE%AE.html?username=sunwukong HTTP/1.1
            Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
            Accept-Encoding: gzip, deflate, br
            Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,la;q=0.7
            Cache-Control: max-age=0
            Connection: keep-alive
            Host: 127.0.0.1:5500
            If-Modified-Since: Thu, 27 Oct 2022 11:10:28 GMT
            If-None-Match: W/"1b8-1841922e832"
            Referer: http://127.0.0.1:5500/07_http%E5%8D%8F%E8%AE%AE/01_http%E5%8D%8F%E8%AE%AE.html?username=sunwukong
            Sec-Fetch-Dest: document
            Sec-Fetch-Mode: navigate
            Sec-Fetch-Site: same-origin
            Upgrade-Insecure-Requests: 1
            User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.0.0 Safari/537.36
            sec-ch-ua: "Chromium";v="106", "Google Chrome";v="106", "Not;A=Brand";v="99"
            sec-ch-ua-mobile: ?0
            sec-ch-ua-platform: "Windows"
```

## ==express==

### 简介

```javascript
/* 
    express 是node中的服务器软件
        通过express可以快速的在node中搭建一个web服务器
    - 使用步骤：
        1. 创建并初始化项目
            npm init -y
        2. 安装express
            npm add express
        3. 创建index.js 并编写代码
*/

// 引入express
const express = require("express")

// 获取服务器的实例（对象）
const app = express()

/* 
    如果希望服务器可以正常访问，则需要为服务器设置路由，
        路由可以根据不同的请求方式和请求地址来处理用户的请求
    
        app.METHOD(...)
            METHOD 可以是 get 或 post ...

    中间件
        - 在express我们使用app.use来定义一个中间件
            中间件作用和路由很像，用法很像
            但是路由不区分请求的方式，只看路径

        - 和路由的区别
            1.会匹配所有请求
            2.路径设置父目录
*/

// next() 是回调函数的第三个参数，它是一个函数，调用函数后，可以触发后续的中间件
// next() 不能在响应处理完毕后调用
app.use((req, res, next) => {
    console.log("111", Date.now())
    // res.send("<h1>111</h1>")

    next() // 放行，我不管了~~
})

app.use((req, res, next) => {
    console.log("222", Date.now())
    // res.send("<h1>222</h1>")
    next()
})

app.use((req, res) => {
    console.log("333", Date.now())
    res.send("<h1>333</h1>")
})

// http://localhost:3000
// 路由的回调函数执行时，会接收到三个参数
// 第一个 request 第二个 response
// app.get("/hello", (req, res) => {
//     console.log("有人访问我了~")
//     // 在路由中，应该做两件事
//     // 读取用户的请求（request）
//     // req 表示的是用户的请求信息，通过req可以获取用户传递数据
//     // console.log(req.url)

//     // 根据用户的请求返回响应（response）
//     // res 表示的服务器发送给客户端的响应信息
//     //  可以通过res来向客户端返回数据
//     // sendStatus() 向客户端发送响应状态吗
//     // status() 用来设置响应状态吗，但是并不发送
//     // send() 设置并发送响应体
//     // res.sendStatus(404)
//     // res.status(200)
//     res.send("<h1>这是我的第一个服务器</h1>")
// })

// 启动服务器
// app.listen(端口号) 用来启动服务器
// 服务器启动后，我们便可以通过3000端口来访问了
// 协议名://ip地址:端口号/路径
// http://localhost:3000
// http://127.0.0.1:3000
app.listen(3000, () => {
    console.log("服务器已经启动~")
})
```

