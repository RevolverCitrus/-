# Axios

## 什么是axios?

Axios 是一个基于 promise 的 HTTP 库，简单的讲就是可以发送get、post请求。
说到get、post，大家应该第一时间想到的就是Jquery吧，毕竟前几年Jquery比较火的时候，大家都在用他。但是由于Vue、React等框架的出现，Jquery也不是那么吃香了。

## Axios特性

1、可以在浏览器中发送 XMLHttpRequests

2、可以在 node.js 发送 http 请求

3、支持 Promise API

4、拦截请求和响应

5、转换请求数据和响应数据

6、能够取消请求

7、自动转换 JSON 数据

8、客户端支持保护安全免受 XSRF 攻击

## 引入axios

```javascript
<script src="https://cdn.bootcdn.net/ajax/libs/axios/1.1.2/axios.min.js"></script>
```

## axios的基本使用

```javascript
<body>
    <div class="container">
        <h1 class="page-header">基本使用</h1>
        <button class="btn btn-primary">发送 GET 请求</button>
        <button class="btn btn-warning">发送 POST 请求</button>
        <button class="btn btn-success">发送 PUT 请求</button>
        <button class="btn btn-danger">发送 DELETE 请求</button>
    </div>
    <script>
        // 获取按钮
        const btns=document.querySelectorAll('button')

        // 第一个
        btns[0].onclick=function(){
            // 发送AJAX请求
            axios({
                // 请求类型
                method:'GET',
                // URL
                url:'http://localhost:3000/posts/1',
            }).then(response=>{
                console.log(response);
            })
        },

        // 第二个
        // 添加一篇新的文章
        btns[1].onclick=function(){
            // 发送AJAX请求
            axios({
                // 请求类型
                method:'POST',
                // URL
                url:'http://localhost:3000/posts',
                // 设置请求体
                data:{
                    title:'你好！欢迎来到尚硅谷！',
                    author:'zs'
                }
            }).then(response=>{
                console.log(response);
            })
        },

        //第三个
        // 更新数据 
        btns[2].onclick=function(){
            // 发送AJAX请求
            axios({
                // 请求类型
                method:'PUT',
                // URL
                url:'http://localhost:3000/posts/2',
                // 设置请求体
                data:{
                    title:'你好！欢迎来到尚硅谷！',
                    author:'ls'
                }
            }).then(response=>{
                console.log(response);
            })
        }

        //第四个
        // 删除数据 
        btns[3].onclick=function(){
            // 发送AJAX请求
            axios({
                // 请求类型
                method:'delete',
                // URL
                url:'http://localhost:3000/posts/2',
            }).then(response=>{
                console.log(response);
            })
        }
    </script>
</body>
```

## axios的其他使用

```javascript
<body>
    <div class="container">
        <h1 class="page-header">基本使用</h1>
        <button class="btn btn-primary">发送 GET 请求</button>
        <button class="btn btn-warning">发送 POST 请求</button>
        <button class="btn btn-success">发送 PUT 请求</button>
        <button class="btn btn-danger">发送 DELETE 请求</button>
    </div>
    
    <script>
        // 获取按钮
        const btns=document.querySelectorAll('button')

        // 发送GET请求
        btns[0].onclick=function(){
            // axios()
            axios.request({
                method:'GET',
                url:'http://localhost:3000/comments'
            }).then(response=>{
                console.log(response);
            })
        }

        // 发送POST请求
        btns[1].onclick=function(){
            // axios()
            axios.post(
                'http://localhost:3000/comments',
                {
                    'body':'喜大普奔',
                    'postId':2
                }).then(response=>{
                    console.log(response);
                })
        }
    </script>
</body>
```

## axios默认配置

```javascript
<body>
    <div class="container">
        <h1 class="page-header">基本使用</h1>
        <button class="btn btn-primary">发送 GET 请求</button>
        <button class="btn btn-warning">发送 POST 请求</button>
        <button class="btn btn-success">发送 PUT 请求</button>
        <button class="btn btn-danger">发送 DELETE 请求</button>
    </div>
    <script>
        // 获取按钮
        const btns=document.querySelectorAll('button')
        
        // 默认配置
        axios.defaults.method='GET';//设置默认的请求类型为GET
        axios.defaults.baseURL='http://localhost:3000'//设置基础URL

        // 第一个
        btns[0].onclick=function(){
            // 发送AJAX请求
            axios({
                // URL
                url:'/posts',
            }).then(response=>{
                console.log(response);
            })
        }
    </script>
</body>
```

## axios创建实例对象

```javascript
<body>
    <div class="container">
        <h1 class="page-header">基本使用</h1>
        <button class="btn btn-primary">发送 GET 请求</button>
        <button class="btn btn-warning">发送 POST 请求</button>
    </div>
    <script>
        // 获取按钮
        const btns=document.querySelectorAll('button')
        
        // 创建实例对象 https://api.apiopen.top/api/sentences
        const duanzi=axios.create({
            baseURL:'https://api.apiopen.top',
            timeout:2000
        });

        // 这里 duanzi与axios对象的功能几乎是一样的
        // duanzi({
        //     url:'/api/sentences',
        // }).then(response=>{
        //         console.log(response);
        // })

        duanzi.get('/api/sentences').then(response=>{
            console.log(response.data);
        })
    </script>
</body>
```

## 拦截器

```javascript
<body>
    <script>
        // 添加请求拦截器1    config配置对象,可以对其进行更改，response同理
        axios.interceptors.request.use(function (config) {
            console.log('请求拦截器，成功 -1号');
            // 在发送请求之前做些什么
            // 修改config中的参数
            config.params={a:100};

            return config;
            // throw '参数出了问题';
        }, function (error) {
            console.log('请求拦截器，失败 -1号');
            // 对请求错误做些什么
            return Promise.reject(error);
        });

        // 添加请求拦截器2
        axios.interceptors.request.use(function (config) {
            console.log('请求拦截器，成功 -2号');
            // 在发送请求之前做些什么
            // 修改config中的参数
            config.timeout=2000;

            return config;
            // throw '参数出了问题';
        }, function (error) {
            console.log('请求拦截器，失败 -2号');
            // 对请求错误做些什么
            return Promise.reject(error);
        });

        // 添加响应拦截器1
        axios.interceptors.response.use(function (response) {
            console.log('响应拦截器，成功 -1号');
            // 对响应数据做点什么
            return response;
        }, function (error) {
            console.log('响应拦截器，失败 -1号');
            // 对响应错误做点什么
            return Promise.reject(error);
        });

        // 添加响应拦截器2
        axios.interceptors.response.use(function (response) {
            console.log('响应拦截器，成功 -2号');
            // 对响应数据做点什么
            return response;
        }, function (error) {
            console.log('响应拦截器，失败 -2号');
            // 对响应错误做点什么
            return Promise.reject(error);
        });


        axios({
                // 请求类型
                method:'GET',
                // URL
                url:'http://localhost:3000/posts',
            }).then(response=>{
                console.log('自定义回调处理成功的结果')
                // console.log(response);
            }).catch(reason=>{
                console.log('自定义失败回调')
            })
    </script>
</body>
```

## 取消请求

```javascript
<body>
    <div class="container">
        <h1 class="page-header">axios取消请求</h1>
        <button class="btn btn-primary">发送请求</button>
        <button class="btn btn-warning">取消请求</button>
    </div>
    <script>
        // 获取按钮
        const btns = document.querySelectorAll('button')

        // 2.声明全局变量
        let cancel=null;

        // 第一个
        btns[0].onclick=function(){
            // 检测上一次请求是否已经完成
            if(cancel !==null){
                // 取消上一次请求
                cancel();
            }
            // 发送AJAX请求
            axios({
                // 请求类型
                method:'GET',
                // URL
                url:'http://localhost:3000/posts',
                
                // 1.添加配置对象的属性
                cancelToken: new axios.CancelToken(function(c){
                    // 3.将c的值赋值给cancel
                    cancel=c;
                })
            }).then(response=>{
                console.log(response);
                // 初始化cancel的值
                cancel=null;
            })
        };
        
        // 绑定第二个事件，取消请求
        btns[1].onclick=function(){
            cancel();
        };
    </script>
</body>
```

