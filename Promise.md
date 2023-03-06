# Promise

## 简介

（一）相关知识及特点

1. Promise是一门Es6引进的js进行异步编码方面的新解决方法

2. 语法上：Promise是一个构造函数

  功能上：Promise对象用来封装一个异步操作并可以获取其成功/失败的结果值

3. ==优点==：

​		① 指定回调函数的方式更加灵活（甚至可以在异步任务结束后指定多个）

​		② 支持链式调用，可以解决回调地狱问题

a. ==回调地狱==：回调函数嵌套调用，外部回调函数异步执行的结果是嵌套的回调执行的条件

b. 回调地狱特点：不便于阅读，不便于异常处理

c. 解决方案：Promise链式调用

## 初认Promise

### 初体验

```javascript
<body>
    <div class="container">
        <h1 class="header">Promise初体验</h1>
        <button>点击抽奖</button>
    </div>
</body>
<script>
    //生成随机数 30%中奖概率
    //点击按钮 1s后显示是否中奖
    //若中奖，则弹出 恭喜中奖，获得吴世勋亲笔签名及周边大礼包
    //若未中奖，则弹出 再接再厉ovo

    //生成随机数
    function rand(m, n) {
        return Math.ceil(Math.random() * (n - m + 1)) + m - 1;
    }

    const btn = document.querySelector("button");
    btn.addEventListener("click", () => {
        //Promise 可包裹一个异步操作
        //Promise 形式
        //参数为函数类型的一个值，分别有两个形参 resolve、reject
        //resolve 解决 reject拒绝  都为函数类型的数据
        //当异步任务成功时，调用resolve；当异步任务失败时，调用reject
        const p = new Promise((resolve, reject) => {
            setTimeout(() => {
                //获取从1-100的随机数
                let num = rand(1, 100);
                if (num <= 30) {
                    resolve(num);
                    //调用resolve之后，会将promise对象（即p）的状态设置为成功
                } else {
                    reject(num);
                    //调用resolve之后，会将promise对象（即p）的状态设置为失败
                }
            }, 1000);
        });

        //实现resolve和reject
        //调用then方法，接受两个参数，参数类型都为函数类型的数据
        //第一个参数为成功是的回调，第二个参数为失败时的回调
        p.then(
            (value) => {
                alert(
                    "您的号码为：" +
                    value +
                    "，恭喜中奖，获得吴世勋亲笔签名及周边大礼包"
                );
            },
            (reason) => {
                alert("您的号码为：" + reason + "，未能中奖再接再厉ovo");
            }
        );
    });
</script>
```

### 读取 conten.txt文件

```javascript
//引入 fs 回调函数形式
const fs = require("fs");

//Promise形式
let p = new Promise((resolve, reject) => {
    fs.readFile("content.txt", (err, data) => {
    //readFile 读取文件是一个异步操作
    //如果失败（出错则调用reject）
    if (err) reject(err);
    //如果成功（成功则调用resolve）
    resolve(data);
    });
});
//调用then
p.then(
    (value) => {
    console.log(value.toString());
    },
    (reason) => {
    console.log(reason);
    }
);
```

### Promise 封装 AJAX 操作

```javascript
<body>
    <div class="container">
        <h1>Promise 封装 AJAX 操作</h1>
        <button>点击发送AJAX</button>
    </div>
</body>
<script>
    //接口地址 https://api.apiopen.top/getJoke
    //Promise 形式
    const btn = document.querySelector("button");
    //绑定事件
    btn.addEventListener("click", () => {
        const p = new Promise((resolve, reject) => {
            //1.创建对象
            const xhr = new XMLHttpRequest();
            //2.初始化
            xhr.open("GET", "https://api.apiopen.top/getJoke");
            //3.发送
            xhr.send();
            //4.绑定事件处理响应结果
            xhr.onreadystatechange = () => {
                //判断响应
                if (xhr.readyState === 4) {
                    if (xhr.status >= 200 && xhr.status < 300) {
                        //控制台输出响应体
                        resolve(xhr.response);
                    } else {
                        //控制台输出响应状态码
                        reject(xhr.status);
                    }
                }
            };
        });
        //调用then方法
        p.then(
            (value) => {
                console.log(value);
            },
            (reason) => {
                console.warn(reason);
            }
        );
    });
</script>
```

### Promise封装练习

```javascript
/**
 * 封装一个函数 mineReadFile 读取文件内容
 * 参数：path 文件路径
 * 返回：promise对象
 */
function mineReadFile(path) {
    return new Promise((resolve, reject) => {
    //读取文件
    require("fs").readFile(path, (err, data) => {
      //出现错误则调用reject
        if (err) reject(err);
      //成功则调用resolve
        resolve(data);
    });
    });
}

mineReadFile("./content.txt").then(
    (value) => {
    //输出文件内容
    console.log(value.toString());
    },
    (reason) => {
    //输出错误问题
    console.log(reason);
    }
);

```

### util.promisify方法

```javascript
// 实践五：util.promisify方法进行promise风格转化

// 1）该方法将基于回调的函数转换为基于 Promise 的函数。
    //这使您可以将 Promise 链和 async/await 与基于回调的 API 结合使用

//引入 util模块
const util = require("util");
//引入 fs模块
const fs = require("fs");
//返回一个新的函数
let mineReadFile = util.promisify(fs.readFile);

mineReadFile("content.txt").then(
    (value) => {
    console.log(value.toString());
    },
    (reason) => {
    console.log(reason);
    }
);

```

## ES6-ES11中的Promise

### Promise基本语法

```javascript
    <script>
        // 实例化Promise对象
        const p=new Promise(function(resolve,reject){
            setTimeout(function(){
                // 
                // let data='数据库中的用户数据';
                // // resolve
                // resolve(data);

                let err='数据读取失败';
                reject(err);
            },1000);
        });

        // 调用promise对象的then方法
        p.then(function(value){
            console.log(value);
        },function(reason){
            console.log(reason);
        })
    </script>
```

### Promise读取文件

```javascript
// 1.引入fs的模块
const fs=require('fs')

// 2.调用方法读取文件
// fs.readFile('./resources/为学.md',(err,data)=>{
//     // 如果失败，则抛出错误
//     if(err) throw err;
//     // 如果没有出错，则输出内容
//     console.log(data.toString());

// });

// 3.使用Promise封装
const p=new Promise(function(resolve,reject){
    fs.readFile('./resources/为学.md',(err,data)=>{
        // 判断如果失败
        if(err) reject(err);
        // 如果成功
        resolve(data);
    });
});


p.then(function(value){
    console.log(value.toString());
},function(reason){
    console.log('读取失败！！');
});


```

### catch方法

```javascript
<script>
    //catch()函数只有一个回调函数,意味着如果Promise对象状态为失败就会调用catch()方法并且调用回调函数
    const p = new Promise((resolve, reject) => {
        setTimeout(()=>{
            reject('出错啦')
        },1000)
    })

    p.catch(reason => {
        console.log(reason)
    })
</script>
```

### ==使用Promise链式调用 ，解决了回调地狱的问题==

```javascript
const fs=require('fs')

//使用fs.readFile,会产生地狱回调的问题
// fs.readFile('./resources/为学.md',(err,data1)=>{
//     fs.readFile('./resources/行宫.md',(err,data2)=>{
//         fs.readFile('./resources/鹿柴.md',(err,data3)=>{
//             let result=data1+'\r\n'+data2+'\r\n'+data3;
//             console.log(result);
//         });
//     });
// });

// 使用Promise链式调用 ，解决了回调地狱的问题
// 使用promise实现
const p=new Promise(function(resolve,reject){
    fs.readFile('./resources/为学.md',(err,data)=>{
        resolve(data);
    });
});

p.then(function(value){
    return new Promise((resolve,reject)=>{
    fs.readFile('./resources/行宫.md',(err,data)=>{
        resolve([value,data]);
    });
    })
}).then(value=>{
    return new Promise((resolve,reject)=>{
        fs.readFile('./resources/鹿柴.md',(err,data)=>{
            // 压入
            value.push(data);
            resolve(value);
        });
        })
}).then(value=>{
    console.log(value.join('\r\n'));
})
```

