# Ajax

## 简介

- Ajax全称为Asynchronous Javascript And XML，即异步JS和XML
- 通过Ajax可以在浏览器中向服务器发送异步请求，最大的优势：**无刷新获取数据**
- AJAX不是新的编程语言，而是一种将现有的标准组合在一起使用的新方式

可以使用 AJAX 最主要的两个特性做下列事：

- 在不重新加载页面的情况下发送请求给服务器。
- 接受并使用从服务器发来的数据。

## XML简介

- XML：可扩展标记语言
- XML：被设计用来传输和存储数据
- XML和HTML类似，不同点：HTML中都是预定义标签，XML中没有预定义标签，全是自定义标签，用来表示一些数据
- 现在已被JSON取代

##  AJAX 的特点

###  AJAX的优点

1. 可以无刷新页面与服务端进行通信
2. 允许你根据用户事件来更新部分页面内容

###  AJAX 的缺点

1. 没有浏览历史，不能回退
2. 存在跨域问题（同源）
3. SEO不友好（爬虫获取不到信息）



## 准备工作

运行server.js文件，自己使用node搭建一个简单服务器

![image-20230225214645229](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230225214645229.png)

```javascript
//server.js文件如下

//1.引入express
const { response } = require('express');
const express = require('express');

//2.创建应用对象
const app = express();

//3.创建路由规则
//request是对请求报文的封装
//response是对响应报文的封装
app.get('/server', (request, response) => {
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*')

    //设置响应体
    response.send('hello AJAX -2');
});

app.all('/server', (request, response) => {
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*')
    // 响应头
    response.setHeader('Access-Control-Allow-Headers', '*');
    //设置响应体
    response.send('hello AJAX POST');
});

app.all('/json-server', (request, response) => {
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    // 响应头
    response.setHeader('Access-Control-Allow-Headers', '*');
    // 响应一个数据
    const data={
        name:'atguigu'
    };
    // 对对象进行一个字符串转换
    let str=JSON.stringify(data);
    //设置响应体
    response.send(str);
});

// 延时响应
app.all('/delay', (request, response)=>{
    // 设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');

    response.setHeader('Access-Control-Allow-Headers', '*');
    setTimeout(() =>{
        // 设置响应体
        response.send('延时响应');
    },3000)
});

// JQuery 服务
app.all('/JQuery-server', (request, response)=>{
    // 设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Headers', '*');
    const data={name:'尚硅谷'};
    // response.send('Hello jQuery AJAX');
    response.send(JSON.stringify(data));
});

// axios 服务
app.all('/axios-server', (request, response)=>{
    // 设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Headers', '*');
    const data={name:'尚硅谷'};
    response.send(JSON.stringify(data));
});

// fetch 服务
app.all('/fetch-server', (request, response)=>{
    // 设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Headers', '*');
    const data={name:'尚硅谷'};
    response.send(JSON.stringify(data));
});

//4.监听端口启动服务
app.listen(8000, () => {
    console.log("服务已经启动，8000 端口监听中......");
});
```

## 原生Ajax

### GET请求

```javascript
    <script>
        //获取button对象
        const btn=document.getElementsByTagName('button')[0];
        const result=document.getElementById("result");
        //绑定事件
        btn.onclick=function(){
            //1.创建对象
            const xhr=new XMLHttpRequest();
            //2.初始化，设置请求的类型和url
            xhr.open('GET', 'http://127.0.0.1:8000/server?a=100&b=200&c=300');
            //3.发送
            xhr.send();
            //4.事件绑定    处理服务端返回的结果
            //on 表示当。。。。的时候
            //readystate 是xhr对象中的属性，表示状态0 1 2 3 4
            //change 改变
            xhr.onreadystatechange=function(){
                //判断(服务端返回了所有的结果)
                if(xhr.readyState===4){
                    //判断响应状态码 200 404 403 401 500
                    //2xx成功
                    if(xhr.status>=200&&xhr.status<300){
                        //处理结果  行  头  空行   体
                        //响应
                        // console.log(xhr.status);//状态码
                        // console.log(xhr.statusText);//响应状态字符串
                        // console.log(xhr.getAllResponseHeaders());//所有响应头
                        // console.log(xhr.response);//响应体
                        //设置result的文本
                        result.innerHTML=xhr.response;
                    }else{
                        
                    }
                }
            }
        };
    </script>
```

![image-20230225213539713](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230225213539713.png)

### POST请求

```javascript
    <script>
        // 获取元素对象
        const result=document.getElementById("result");
        // 绑定事件
        result.addEventListener("mouseover", function(){
            // 1.创建对象
            const xhr=new XMLHttpRequest();
            // 2.初始化，设置请求的类型和url
            xhr.open("POST","http://127.0.0.1:8000/server");
            // 设置请求头
            xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
            // 3.发送(可以设置请求体)
            // xhr.send('a=100&b=200&c=300');
            // xhr.send('a:100&b:200&c:300');
            xhr.send('124354');
            //4.事件绑定    处理服务端返回的结果
            xhr.onreadystatechange=function(){
                //判断
                if(xhr.readyState===4){
                    if(xhr.status>=200&&xhr.status<300){
                        // 创建返回结果
                        result.innerHTML=xhr.response;
                    }
                }
            };
        });
    </script>
```

![image-20230225214859544](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230225214859544.png)

### JSON响应

```javascript
    <script>
        const result=document.getElementById("result");
        // 绑定键盘按下事件
        window.onkeydown = function () {
            // 发送请求
            const xhr = new XMLHttpRequest();
            // 设置响应体数据的类型
            xhr.responseType='json';
            // 初始化
            xhr.open("GET", 'http://127.0.0.1:8000/json-server');
            // 发送
            xhr.send();
            // 事件绑定
            xhr.onreadystatechange = function () {
                //判断(服务端返回了所有的结果)
                if (xhr.readyState === 4) {
                    //判断响应状态码 200 404 403 401 500
                    //2xx成功
                    if (xhr.status >= 200 && xhr.status < 300) {
                        //处理结果  行  头  空行   体
                        
                        // console.log(xhr.response);
                        // result.innerHTML=xhr.response;
                        // 1.手动对数据转化
                        // let data=JSON.parse(xhr.response);
                        // console.log(data);
                        // result.innerHTML=data.name;
                        // 2.自动转换
                        console.log(xhr.response);
                        result.innerHTML=xhr.response.name;
                    }
                }
            }
        }
    </script>
```

![image-20230225215406651](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230225215406651.png)

### 请求超时与网络异常

```javascript
<body>
    <button>点击发送请求</button>
    <div id="result"></div>
    <script>
        const btn=document.getElementsByTagName('button')[0];
        const result=document.querySelector('#result');

        btn.addEventListener('click', function(){
            const xhr=new XMLHttpRequest();
            // 超时设置2s设置
            xhr.timeout=2000;
            
            // 超时回调
            xhr.ontimeout=function(){
                alert("网络异常,请稍后重试")
            };
            // 网络异常回调
            xhr.onerror=function(){
                alert("你的网络好像出了问题!");
            };

            xhr.open('GET', 'http://127.0.0.1:8000/delay');
            xhr.send();
            xhr.onreadystatechange=function(){
                if(xhr.readyState===4){
                    if(xhr.status>=200&&xhr.status<300){
                        result.innerHTML=xhr.response;
                    }
                }
            }
        });
    </script>
</body>
```

### 取消请求

```javascript
<body>
    <button>点击发送</button>
    <button>点击取消</button>
    <script>
        // 获取元素对象
        const btns=document.querySelectorAll("button");
        let x=null;

        btns[0].onclick=function(){
            x=new XMLHttpRequest();
            x.open("GET",'http://127.0.0.1:8000/delay');
            x.send();
        };

        // abort 取消请求
        btns[1].onclick=function(){
            x.abort();
        };
    </script>
</body>
```

### 重复请求问题

```javascript
<body>
    <button>点击发送</button>
    <script>
        // 获取元素对象
        const btns=document.querySelectorAll("button");
        let x=null;
        // 标识变量
        let isSending=false;//是否正在发送AJAX请求


        btns[0].onclick=function(){
            // 判断标识变量
            if(isSending){
                x.abort();//如果正在发送，则取消该请求，创建一个新的请求
            }
            x=new XMLHttpRequest();
            //修改标识变量值
            isSending=true;
            x.open("GET",'http://127.0.0.1:8000/delay');
            x.send();
            x.onreadystatechange=function(){
                if(x.readyState===4){
                    isSending=false;
                }
            }
            
        };

        // abort() 取消请求
        btns[1].onclick=function(){
            x.abort();
        };
    </script>
```

## JQuery 发送 AJAX 请求

准备工作：引入jQuery

```javascript
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.1/jquery.min.js"></script>
```

```javascript
<body>
    <div class="container">
        <h2 class="page-header">JQuery发送AJAX 请求</h2>
        <button class="btn btn-primary">GET</button>
        <button class="btn btn-danger">POST</button>
        <button class="btn btn-info">通用型方法ajax</button>
    </div>
    <script>
        $('button').eq(0).click(function(){
            $.get('http://127.0.0.1:8000/jquery-server', {a:100,b:200}, function(data){
                console.log(data);
            },'json');
        });
        $('button').eq(1).click(function(){
            $.post('http://127.0.0.1:8000/jquery-server', {a:100,b:200}, function(data){
                console.log(data);
            })
        });

        $('button').eq(2).click(function(){
            $.ajax({
                // url
                url:'http://127.0.0.1:8000/JQuery-server',
                // 参数
                data:{a:100,b:200},
                // 请求类型
                type:'GET',
                // 响应体结果
                dataType:'json',
                // 成功的回调
                success:function(data){
                    console.log(data);
                },
                // 超时时间
                timeout:2000,
                // 失败的回调
                error:function(){
                    console.log("出错了！");
                },
                headers:{
                    c:300,d:400
                }
            });
        });
    </script>
</body>
```

![image-20230225220205421](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230225220205421.png)

## Axios 发送 AJAX请求

准备工作：引入Axios 

```javascript
    <script src="https://cdn.bootcdn.net/ajax/libs/axios/1.1.2/axios.min.js"></script>
```

```javascript
<body>
    <button>GET</button>
    <button>POST</button>
    <button>AJAX</button>

    <script>
		//获取按钮对象
        const btns = document.querySelectorAll('button');

        // 配置baseURL
        axios.defaults.baseURL = 'http://127.0.0.1:8000';

        btns[0].onclick = function () {
            // GET请求
            axios.get('/axios-server', {
                // url参数
                params: {
                    id: 100,
                    vip: 7
                },
                // 请求头信息
                headers: {
                    name: 'atguigu',
                    age: 20
                }
            }).then(value => {
                console.log(value);
            })
        },
            btns[1].onclick = function () {
                // POST请求
                axios.post('/axios-server',
                    // 请求体
                    {
                        username: 'admin',
                        password: 'admin'
                    },
                    {
                        // url参数
                        params: {
                            id: 200,
                            vip: 9
                        },
                        // 请求头信息
                        headers: {
                            height: 180,
                            weight: 180
                        }
                    }).then(value => {
                        console.log(value);
                    })
            },
            btns[2].onclick = function () {
                axios({
                    // 请求方法
                    method: 'POST',
                    //url
                    url: '/axios-server',
                    // url参数
                    params: {
                        vip: 10,
                        level: 30
                    },
                    // 头信息
                    headers: {
                        a: 100,
                        b: 200
                    },
                    //请求体参数
                    data: {
                        username: 'admin',
                        password: 'admin'
                    }
                }).then(response=>{
                    //响应状态码
                    console.log(response.status);
                    //响应状态字符串
                    console.log(response.statusText);
                    //响应头信息
                    console.log(response.headers);
                    //响应体
                    console.log(response.data);
                })
            }
    </script>
</body>
```

![image-20230225220546576](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230225220546576.png)

## fetch 发送 AJAX请求

```javascript
<body>
    <button>AJAX请求</button>
    <script>
        const btn=document.querySelector('button');

        btn.onclick=function(){
            fetch('http://127.0.0.1:8000/fetch-server?vip=10',{
                // 请求方法
                method:'POST',
                // 请求头
                headers:{
                    name:'atguigu'
                },
                //请求体
                body:'username=admin&password=admin'

            }).then(response=>{
                // return response.text();
                return response.json();
            }).then(response=>{
                console.log(response);
        });
    }
    </script>
</body>
```

## 跨域

### 同源策略

同源策略（Same-Origin Policy）最早由 Netscape 公司提出，是浏览器的一种安全策略。

同源：协议、域名、端口号 必须完全相同

违背同源策略就是跨域



准备工作：运行server.js文件

```javascript
const { request } = require('express');
const express = require('express');

const app = express();

app.get('/home', (request, response) => {
    // 设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Headers', '*');
    //响应一个页面
    response.sendFile(__dirname + '/index.html');
});
app.all('/data', (request, response) => {
    // 设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Headers', '*');
    
    response.send("用户数据");
});
app.listen(9000, () => {
    console.log("服务器启动成功");
});
```

```javascript
<body>
    <H2>首页</H2>
    <button>data</button>
    <script>
        const btn = document.querySelector('button');
        btn.onclick = function() {
            const x = new XMLHttpRequest();
            //这里因为是满足同源策略的，所以url可以简写
            x.open("GET", "http://127.0.0.1:9000/data");
            //发送
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
</body>
```

![image-20230225221311129](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230225221311129.png)

### 如何解决跨域

#### JSONP

1. JSONP是什么

   JSONP (JSON with Padding)，是一个非官方的跨域解决方案，纯粹凭借程序员的聪明才智开发出来，只支持get请求

2. JSONP 怎么工作的？

   在网页有一些标签天生具有跨域能力，比如：img, link, iframe, script

   JSONP就是利用**script**标签的跨域能力来发送请求的

3. JSONP的使用

   - 动态的创建一个script标签

   ```
   var script = document.createElement("script");
   ```

   - 设置script的src，设置回调函数

   ```
   script.src = "http://locallhost:3000/textAJAX?callback=abc"
   ```

#### CORS

推荐阅读：

- http://www.ruanyifeng.com/blog/2016/04/cors.html
- https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS

1. CORS是什么？

   CORS (Cross-Origin Resource Sharing), 跨域资源共享。CORS 是官方的跨域解决方案，它的特点是不需要在客户端做任何特殊的操作，完全在服务器中进行处理，支持 get 和 post 等请求。跨域资源共享标准新增了一组 HTTP 首部字段（响应头），允许服务器声明哪些源站通过浏览器有权限访问哪些资源

2. CORS怎么工作的？

   CORS 是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应以后就会对响应放行。

3. CORS 的使用

   主要是服务端的设置：

   ```
   rounter.get("/testAJAX",function(req, res){
   
   })
   ```

#### 代理服务器

在vue中出现跨域问题，可以自己使用node.js来配置代理服务器解决

```javascript
//  cli3     vue.config.js
  devServer: {
    proxy:{
      "/api": {
          target: "http://www.xiongmaoyouxuan.com", // 需要代理的域名
           ws: false, // 是否启用websockets
          changeOrigin: true, //开启代理：在本地会创建一个虚拟服务端，然后发送请求的数据，并同时接收请求							的数据，这样服务端和服务端进行数据的交互就不会有跨域问题
          pathRewrite: {  //重写匹配的字段，如果不需要在请求路径上，重写为""
              "^/api": "/api"
          }
      },
   
  },
  }

  //cli2的放在config文件夹中的index.js  中
    dev: {
    proxyTable:{
      "/api": {
          target: "http://www.xiongmaoyouxuan.com", // 需要代理的域名
           ws: false, // 是否启用websockets
          changeOrigin: true, //开启代理：在本地会创建一个虚拟服务端，然后发送请求的数据，并同时接收请求							的数据，这样服务端和服务端进行数据的交互就不会有跨域问题
          pathRewrite: {  //重写请求路径上匹配到的字段，如果不需要在请求路径上，重写为""
              "^/api": "/api"，
              "demo.json": "index.json"
          }
      },
  },
  }

```

```javascript
//在创建axios的时候，beseURL这样配置
const ajax = axios.create({
    baseURL:"/api",
    timeout: 6000,//请求超时时间
})

//创建的请求
export function getData() { //get
  return request({
    url: '/search/home',
    method: 'GET'
  })
}
```

这样匹配到这个字段时就会代理到target去，将target添加到/前面，在根据pathRewrite,然后是你请求的ajax
意思大概就是，你的beseURL加上你请求的url满足proxy的匹配，那你的代理就是没问题的
同时处理的也是匹配到的部分。
这样虚拟服务器请求的的就是

> http://www.xiongmaoyouxuan.com/api/search/home

但你本地看见的请求地址可能是这样的

![image-20230225223009840](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230225223009840.png)
