# Vue核心

## 简介

介绍与描述

- 动态构建用户界面的渐进式JavaScript框架

### Vue的特点

1. 遵循MVVM模式
2. 编码简洁，体积小，运行效率高，适合移动/PC端开发
3. 它本身只关注UI，可以引入其它[第三方库](https://so.csdn.net/so/search?q=第三方库&spm=1001.2101.3001.7020)开发项目

### 其他JS框架的关联

1. 借鉴 Angular 的模板和数据绑定技术
2. 借鉴 React 的组件化和虚拟DOM技术

### Vue周边库

- vue-cli：vue脚手架
- vue-resource
- axios
- vue-router：路由
- vuex：状态管理
- element-ui：基于vue的UI组件库（PC端）

## 1.初识Vue

```javascript
    <script>
        Vue.config.productionTip=false//阻止vue在启动时生成生产提示
        // 创建Vue实例
        new Vue({
            el:'#root',//el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串
            data:{//data中用于存储数据，数据供el所指定的容器去使用，值我们先写成一个对象
                name:'尚硅谷'
            }
        })
    </script>

注意：
想让Vue工作，就必须创建一个Vue实例，且要传入一个配置对象
root容器里的代码依然符合html规范，只不过混入了一些特殊的Vue语法
root容器里的代码被称为Vue模板
Vue实例与容器是一一对应的
真实开发中只有一个Vue实例，并且会配合着组件一起使用
{{xxx}}中的xxx要写js表达式，且xxx可以自动读取到data中的所有属性
一旦data中的数据发生变化，那么模板中用到该数据的地方也会自动更新

```

## 2.模板语法

```html
<body>
    <div id="root">
        <h1>插值语法</h1>
        <h3>你好，{{name}}</h3>
        <hr>
        <h1>指令语法</h1>
        <a v-bind:href="url">点我去尚硅谷学习1</a>
        <a :href="url">点我去尚硅谷学习2</a>
    </div>
    <script>
        Vue.config.productionTip = false//阻止vue在启动时生成生产提示
        new Vue({
            el: "#root",
            data: {
                name: 'jack',
                url: "http://www.atguigu.com",
            }
        })
    </script>
</body>
    <!-- 
    总结：
    Vue模板语法包括两大类：
    
    插值语法：
    功能：用于解析标签体内容
    写法：{{xxx}}，xxx是js表达式，且可以直接读取到data中的所有区域

    指令语法：
    功能：用于解析标签（包括：标签属性、标签体内容、绑定事件…）
    举例：<a v-bind:href="xxx">或简写为<a :href="xxx">，xxx同样要写js表达式，
        且可以直接读取到data中的所有属性
        备注：Vue中有很多的指令，且形式都是v-???，此处我们只是拿v-bind举个例子
    -->
```

## 3.数据绑定

```html
<body>
    <div id="root">
        单向绑定数据：<input type="text" v-bind:value="name"><br>
        双向绑定数据：<input type="text" v-model:value="name">
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                name:'尚硅谷',
            }
        })
    </script>
</body>
    <!-- 总结：
    Vue中有2种数据绑定的方式：
    
    1.单向绑定（v-bind）：数据只能从data流向页面
    2.双向绑定（v-model）：数据不仅能从data流向页面，还可以从页面流向data
    
    备注：
        1.双向绑定一般都应用在表单类元素上（如：<input>、<select>、<textarea>等）
        2.v-model:value可以简写为v-model，因为v-model默认收集的就是value值
    -->
```

## 4.el与data的两种写法

```html
<body>
    <div id="root">
        <h1>你好，{{name}}</h1>
    </div>
    <script>
        Vue.config.productionTip = false//阻止vue在启动时生成生产提示
        // el的两种写法
        // const v=new Vue({
        //     //el:'#root',第一种写法
        //     data:{
        //         name:'尚硅谷',
        //     }
        // })
        // console.log(v);
        // v.$mount('#root')//第二种写法
        // data的两种写法
        const v=new Vue({
            el:'#root',
            // data的第一种写法：对象式
            // data:{
            //     name:'尚硅谷',
            // }
            // data的第二种写法：函数式
            data:function(){
                return{
                    name:'尚硅谷',
                }
            }
        })
    </script>
</body>
<!-- 
    el有2种写法：
        创建Vue实例对象的时候配置el属性
        先创建Vue实例，随后再通过vm.$mount('#root')指定el的值

    data有2种写法：
        对象式
        函数式
        如何选择：目前哪种写法都可以，以后学到组件时，data必须使用函数，否则会报错
    由Vue管理的函数，一定不要写箭头函数，否则this就不再是Vue实例了
    -->
```

## 5. [MVVM](https://so.csdn.net/so/search?q=MVVM&spm=1001.2101.3001.7020)模型

![img](https://img-blog.csdnimg.cn/img_convert/ac43527b06defd324a702aea4f7940a0.png)

- MVVM模型：
  - M：模型（Model），data中的数据
  - V：视图（View），模板代码
  - VM：视图模型（ViewModel），Vue实例

- data中所有的属性，最后都出现在了vm身上
- vm身上所有的属性 及 Vue原型身上所有的属性，在Vue模板中都可以直接使用

## ==6. Vue中的数据代理==

![img](https://img-blog.csdnimg.cn/img_convert/1fbebd52e39fa5a97210ee65d0a58069.png)

```
总结：

Vue中的数据代理通过vm对象来代理data对象中属性的操作（读/写）
Vue中数据代理的好处：更加方便的操作data中的数据
基本原理：
通过object.defineProperty()把data对象中所有属性添加到vm上。
为每一个添加到vm上的属性，都指定一个getter/setter。
在getter/setter内部去操作（读/写）data中对应的属性。
```

## ==7. 事件处理==

1. 事件处理

```html
<body>
    <div id="root">
        <h2>欢迎来到{{name}}学习</h2>
        <!-- <button v-on:click="showInfo">点我提示信息</button> -->
        <button @click="showInfo1">点我提示信息1</button>
        <button @click="showInfo2($event,666)">点我提示信息2</button>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                name:'尚硅谷',
            },
            methods:{
                showInfo1(){
                    alert('同学你好！')
                },
                showInfo2(event,number){
                    console.log(event,number)
                }
            }
        })
    </script>
</body>
<!-- 
    总结：

    1.使用v-on:xxx或@xxx绑定事件，其中xxx是事件名
    2.事件的回调需要配置在methods对象中，最终会在vm上
    3.methods中配置的函数，==不要用箭头函数！==否则this就不是vm了
    4.methods中配置的函数，都是被Vue所管理的函数，this的指向是vm或组件实例对象
    5.@click="demo和@click="demo($event)"效果一致，但后者可以传参
    -->
```

2. #### 事件修饰符

```html
<body>
    <div id="root">
        <h2>欢迎来到{{name}}学习</h2>
        <!-- 阻止默认事件 -->
        <a href="http://www.atguigu.com" @click.prevent="showInfo">点我提示信息</a>
        <!-- 阻止事件冒泡 -->
        <div class="demo1" @click="showInfo">
            <button @click.stop="showInfo">点我提示信息</button>
        </div>
        <!-- 事件只是触发一次 -->
        <button @click.once="showInfo">点我提示信息</button>
    
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                name:'尚硅谷',
            },
            methods:{
                showInfo(e){
                    alert('同学你好！')
                },

            }
        })
    </script>
</body>
<!-- 
    总结：

    Vue中的事件修饰符：
    
    1.prevent：阻止默认事件（常用）
    2.stop：阻止事件冒泡（常用）
    3.once：事件只触发一次（常用）
    4.capture：使用事件的捕获模式
    5.self：只有event.target是当前操作的元素时才触发事件
    6.passive：事件的默认行为立即执行，无需等待事件回调执行完毕

    修饰符可以连续写，比如可以这么用：@click.prevent.stop="showInfo"
    -->
```

3. #### 键盘事件

```html
<body>
    <div id="root">
        <h2>hello,{{name}}</h2>
        <input type="text" placeholder="按下回车提示输入" @keyup.enter="showInfo">
    </div>
    <script>
        Vue.config.productionTip=false//阻止vue在启动时生成生产提示
        // 创建Vue实例
        new Vue({
            el:'#root',//el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串
            data:{//data中用于存储数据，数据供el所指定的容器去使用，值我们先写成一个对象
                name:'尚硅谷'
            },
            methods:{
                showInfo(e){
                    // if(e.keyCode!==13) return
                    console.log(e.target.value)
                }
            }
        })
    </script>
</body>
    <!-- 总结：

    键盘上的每个按键都有自己的名称和编码，例如：Enter（13）。而Vue还对一些常用按键起了别名方便使用
    
    Vue中常用的按键别名：
    
    回车：enter
    删除：delete (捕获“删除”和“退格”键)
    退出：esc
    空格：space
    换行：tab (特殊，必须配合keydown去使用)
    上：up
    下：down
    左：left
    右：right

    注意：
        1.系统修饰键（用法特殊）：ctrl、alt、shift、meta
            配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发
            配合keydown使用：正常触发事件
        2.可以使用keyCode去指定具体的按键，比如：@keydown.13="showInfo"，但不推荐这样使用
        3.Vue.config.keyCodes.自定义键名 = 键码，可以自定义按键别名
    -->
```

## ==8.计算属性==

```html
<body>
    <div id="root">
        姓：<input type="text" v-model="firstName"><br><br>
        名：<input type="text" v-model="lastName"><br><br>
        姓名：<span>{{fullName}}</span>
    </div>
    <script>
        Vue.config.productionTip = false 

        new Vue({
            el:'#root', 
            data:{ 
                firstName:'张',
                lastName:'三'
            },
            computed:{
                fullName:{
                    get(){
                        return this.firstName + '-' + this.lastName
                    },
                    set(value){
						const arr = value.split('-')//以"-"进行分割
						this.firstName = arr[0]
						this.lastName = arr[1]
                    }
                }
               // 简写:只是考虑读取的时候才能简写，要修改就不能简写
               // fullName(){
               //     console.log('get被调用了!')
               //     return this.firstName+'-'+this.lastName
               // }
            }
        })
    </script>
</body>
    <!-- 总结：

    计算属性：
        定义：要用的属性不存在，需要通过已有属性计算得来。
        原理：底层借助了Objcet.defineproperty()方法提供的getter和setter。
    
    get函数什么时候执行？
        初次读取时会执行一次
        当依赖的数据发生改变时会被再次调用
        优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便
    
    备注：
        计算属性最终会出现在vm上，直接读取使用即可
        如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变
        如果计算属性确定不考虑修改，可以使用计算属性的简写形式
    -->
```

## 9.监视属性

```html
<body>
    <div id="root">
        <h2>今天天气好{{info}}!</h2>
        <button @click="changeWeather">点击切换天气</button>
    </div>

    <script>
        Vue.config.productionTip = false 

        new Vue({
            el:'#root', 
            data:{ 
                isHot:true,
            },
            computed:{
                info(){
                    return this.isHot ? '炎热' : '凉爽' 
                }
            },
            methods:{
				changeWeather(){
					this.isHot = !this.isHot//简单地取反
				}
			},
            watch:{
                isHot:{//监视的数据
                    immediate:true, //immediate:初始化时让handler调用一下
                    //handler什么时候调用？当isHot发生改变时
                    handler(newValue,oldValue){
						console.log('isHot被修改了',newValue,oldValue)
					}
                }
            }
        })
    </script>
</body>
<!-- 
    总结：

    监视属性watch：
    
        当被监视的属性变化时，回调函数自动调用，进行相关操作
        监视的属性必须存在，才能进行监视
    
    监视有两种写法：

        创建Vue时传入watch配置
        通过vm.$watch监视 -->
```

1. #### 深度监视

```html
<body>
    <div id="root">
        <h1>今天天气很{{info}}</h1>
        <button @click="changeWeather">切换天气</button>
        <hr>
        <h3>a的值是:{{number.a}}</h3>
        <button @click="number.a++">点一下让a+1</button>
        <hr>
        <h3>b的值是:{{number.b}}</h3>
        <button @click="number.b++">点一下让b+1</button>
    </div>

    <script>
        new Vue({
            el: '#root',
            data: {
                isHot: true,
                numbers: {
                    a: 1,
                    b: 1
                }
            },
            computed: {
                info() {
                    return this.isHot ? '炎热' : '凉爽'
                }
            },
            methods: {
                changeWeather() {
                    this.isHot = !this.isHot
                }
            },
            watch: {
                isHot: {
                    //immediate:true,//初始化时让handler调用一下
                    // handler什么时候调用？
                    // 当isHot发生改变时调用
                    handler(newValue, oldValue) {
                        console.log("isHot被修改了", newValue, oldValue)
                    }
                },
                // 监视多级结构中某个属性的变化
                // 'number.a':{
                //     handler(){
                //         console.log('a被改变了')
                //     }
                // }
                // 监视多级结构中所有属性的变化
                numbers: {
                    deep: true,//numbers中的数据a/b变化
                    handler() {
                        console.log('numbers被改变了')
                    }
                }
            }
        })

    </script>
</body>
<!-- 
    总结：

    深度监视：

        Vue中的watch默认不监测对象内部值的改变（一层）
        在watch中配置deep:true可以监测对象内部值的改变（多层）
    
    备注：

        Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以
        使用watch时根据监视数据的具体结构，决定是否采用深度监视 -->
```

2.监视属性简写

```html
<body>
    <div id="root">
    <h1>今天天气很{{info}}</h1>
    <button @click="changeWeather">切换天气</button>
    </div>
    
    <script>
        new Vue({
            el:'#root',
            data:{
                isHot:true,
            },
            computed: {
                info(){
                    return this.isHot ? '炎热':'凉爽'
                }
            },
            methods: {
                changeWeather(){
                    this.isHot=!this.isHot
                }
            },
            watch:{
                // 完整写法
                //isHot:{
                    //immediate:true,//初始化时让handler调用一下
                    //deep:true,深度监视
                    // handler什么时候调用？
                    // 当isHot发生改变时调用
                    // handler(newValue,oldValue){
                    //     console.log("isHot被修改了",newValue,oldValue)
                    // }
                //},
                // 简写；当不需要immediate和deep的时候可以简写
                isHot(newValue,oldValue){
                    console.log("isHot被修改了",newValue,oldValue)
                }
            }
        })

        //第二种写法
        
        // vm.$watch('isHot',{
        //     immediate:true,//初始化时让handler调用一下
        //     // handler什么时候调用？
        //     // 当isHot发生改变时调用
        //     handler(newValue,oldValue){
        //     console.log("isHot被修改了",newValue,oldValue)
        //     }
        // })
        
        // 简写
        // vm.$watch('isHot',function(newValue,oldValue){
        //     console.log("isHot被修改了",newValue,oldValue)
        // })
        
        
    </script>
</body>
```

3.computed和watch

```html
<!-- 总结：

    computed和watch之间的区别：
    
        computed能完成的功能，watch都可以完成
        watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作
    
    两个重要的小原则：
    
        所有被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象
        所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数等、Promise的回调函数），
            最好写成箭头函数，这样this的指向才是vm 或 组件实例对象。
-->
<body>
    <div id="root">
    <h1>今天天气很{{info}}</h1>
    <button @click="changeWeather">切换天气</button>
    </div>
    
    <script>
        new Vue({
            el:'#root',
            data:{
                isHot:true,
            },
            computed: {
                info(){
                    return this.isHot ? '炎热':'凉爽'
                }
            },
            methods: {
                changeWeather(){
                    this.isHot=!this.isHot
                }
            },
            watch:{
                // 完整写法
                //isHot:{
                    //immediate:true,//初始化时让handler调用一下
                    //deep:true,深度监视
                    // handler什么时候调用？
                    // 当isHot发生改变时调用
                    // handler(newValue,oldValue){
                    //     console.log("isHot被修改了",newValue,oldValue)
                    // }
                //},
                // 简写；当不需要immediate和deep的时候可以简写
                isHot(newValue,oldValue){
                    console.log("isHot被修改了",newValue,oldValue)
                }
            }
        })

        //第二种写法
        
        // vm.$watch('isHot',{
        //     immediate:true,//初始化时让handler调用一下
        //     // handler什么时候调用？
        //     // 当isHot发生改变时调用
        //     handler(newValue,oldValue){
        //     console.log("isHot被修改了",newValue,oldValue)
        //     }
        // })
        
        // 简写
        // vm.$watch('isHot',function(newValue,oldValue){
        //     console.log("isHot被修改了",newValue,oldValue)
        // })
        
        
    </script>
</body>
```

## 10.绑定样式

```html
    <style>
        .basic{
            width: 400px;
            height: 100px;
            border: 1px solid black;
        }
        .happy{
            border: 4px solid red;;
            background-color: rgba(255, 255, 0, 0.644);
            background: linear-gradient(30deg,yellow,pink,orange,yellow);
        }
        .sad{
            border: 4px dashed rgb(2, 197, 2);
            background-color: gray;
        }
        .normal{
            background-color: skyblue;
        }
    
        .atguigu1{
            background-color: yellowgreen;
        }
        .atguigu2{
            font-size: 30px;
            text-shadow:2px 2px 10px red;
        }
        .atguigu3{
            border-radius: 20px;
        }
    </style>
</head>
<body>
    <div id="root">
        <!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
        <div class="basic" :class="mood" @click="changeMood">{{name}}</div><br><br><br>

        <!-- 绑定class样式--数组写法，适用于：要绑定的个数不确定，名字也不确定 -->
        <div class="basic" :class="classArr" >{{name}}</div><br><br>

        <!-- 绑定class样式--对象写法，适用于：要绑定的个数确定，名字也确定，但要动态决定用不用 -->
        <div class="basic" :class="classObj" >{{name}}</div><br><br>

        <!-- 绑定style样式--对象写法-->  
        <div class="basic" :style="styleObj">{{name}}</div>
    </div>

    <script>
        new Vue({
            el:'#root',
            data:{
                name:'尚硅谷',
                mood:'normal',
                classArr:['atguigu1','atguigu2','atguigu3'],
                classObj:{
                    atguigu1:false,
                    atguigu2:false
                },
                styleObj:{
                    fontSize:'40px',
                    color:'red',
                    backgroundColor:'#bfa'
                },
            },
            methods: {
                changeMood(){
                    const arr=['happy','sad','normal']
                    const index=Math.floor(Math.random()*3)
                    this.mood=arr[index]
                }
            },
        })
    </script>
</body>
<!-- 
    总结：

    1.class样式：
    
        写法：class="xxx"，xxx可以是字符串、对象、数组
    
        字符串写法适用于：类名不确定，要动态获取
    
        对象写法适用于：要绑定多个样式，个数不确定，名字也不确定
    
        数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用
    
    2.style样式：
    
        :style="{fontSize: xxx}"其中xxx是动态值
        :style="[a,b]"其中a、b是样式对象
    -->
```

## 11.条件渲染if

```html
<body>
    <div id="root">

        <h2>当前的n值是:{{n}}</h2>
        <button @click="n++">点我n+1</button>

        <!-- 使用v-show做条件渲染  面中的结构即节点不会改变-->
        <!-- <h2 v-show="false">欢迎来到{{name}}</h2> -->
        <!-- <h2 v-show="1 === 1">欢迎来到{{name}}</h2> -->

        <!-- 使用v-if做条件渲染  页面中的结构即节点也会改变，删除或增加-->
        <!-- <h2 v-if="false">欢迎来到{{name}}</h2> -->
        <!-- <h2 v-if="1 === 1">欢迎来到{{name}}</h2> -->

        <!-- 使用v-else和v-else-if -->
        <!-- <div v-if="n===1">Angular</div>
        <div v-else-if="n===2">React</div>
        <div v-else-if="n===3">Vue</div>
        <div v-else>哈哈</div> -->

        <!-- v-if和template的配合使用 -->
        <template v-if="n===1">
            <h2>你好1</h2>
            <h2>你好2</h2>
            <h2>你好3</h2>
        </template>


    </div>

    <script>
        new Vue({
            el: '#root',
            data: {
                name: '尚硅谷',
                n: 0,
            },

        })
    </script>
</body>
<!-- 
    总结：

    1.v-if：

    写法：

        v-if="表达式"
        v-else-if="表达式"
        v-else
        适用于：切换频率较低的场景  

    特点：不展示的DOM元素直接被移除

    注意：v-if可以和v-else-if、v-else一起使用，但要求结构不能被打断

    2.v-show：

        写法：v-show="表达式" 
        适用于：切换频率较高的场景 
        特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉

        使用v-if的时，元素可能无法获取到，而使用v-show一定可以获取到 -->
```

## ==12.列表渲染for==

1. 基本列表

```html
<body>
    <div id="root">
        <!-- 遍历数组 -->
        <h2>人员列表</h2>
        <ul>
            <li v-for="(p,index) in persons" :key="index">
                {{p.name}}-{{p.age}}-{{index}}
            </li>
        </ul>

        <!-- 遍历对象 -->
        <h2>汽车信息</h2>
        <ul>
            <li v-for="(value,k) in car" :key="k">
                {{k}}-{{value}}
            </li>
        </ul>

        <!-- 遍历字符串 少用-->
        <h2>字符串字母</h2>
        <ul>
            <li v-for="(char,k) in str" :key="k">
                {{k}}-{{char}}
            </li>
        </ul>

        <!-- 遍历指定次数 少用-->
        <h2>测试遍历指定次数</h2>
        <ul>
            <li v-for="(number,index) in 5" :key="index">
                {{number}}-{{index}}
            </li>
        </ul>

    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                persons:[
                    {id:'001',name:'张三',age:18},
                    {id:'002',name:'李四',age:19},
                    {id:'003',name:'王五',age:15},
                ],
                car:{
                    name:'奥迪A8',
                    price:'70w',
                    color:'black',
                },
                str:'hallo'
            }
        })
    </script>
</body>
<!-- 
    总结：

    v-for指令：

    1.用于展示列表数据
    2.语法：<li v-for="(item, index) in xxx" :key="yyy">，其中key可以是index，也可以是遍历对象的唯一标识
    3.可遍历：数组、对象、字符串（用的少）、指定次数（用的少） -->
```

2. key的原理

```html
<body>
    <div id="root">
        <!-- 遍历数组 -->
        <h2>人员列表</h2>
        <button @click.once="add">添加一个老刘</button>
        <ul>
            <li v-for="(p,index) in persons" :key="p.id">
                {{p.name}}-{{p.age}}-{{index}}
                <input type="text">
            </li>
        </ul>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                persons:[
                    {id:'001',name:'张三',age:18},
                    {id:'002',name:'李四',age:19},
                    {id:'003',name:'王五',age:15},
                ],
            },
            methods: {
                add(){
                    const p={id:'004',name:'老刘',age:25}
                    this.persons.unshift(p)
                }
            },
        })
    </script>
</body>
<!-- 
    总结：

    v-for指令：

    1.用于展示列表数据
    2.语法：<li v-for="(item, index) in xxx" :key="yyy">，
            其中key可以是index，也可以是遍历对象的唯一标识

    3.可遍历：数组、对象、字符串（用的少）、指定次数（用的少）


    1.虚拟DOM中key的作用：key是虚拟DOM中对象的标识，
        当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】，
            随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：

    2.对比规则：
    
        1.旧虚拟DOM中找到了与新虚拟DOM相同的key：
            1.若虚拟DOM中内容没变, 直接使用之前的真实DOM
            2.若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM

        2.旧虚拟DOM中未找到与新虚拟DOM相同的key：创建新的真实DOM，随后渲染到到页面
    
    3.用index作为key可能会引发的问题：
    
        1.若对数据进行逆序添加、逆序删除等破坏顺序操作：
            会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低

        2.若结构中还包含输入类的DOM：会产生错误DOM更新 ==> 界面有问题

    4.开发中如何选择key?
    
        1.最好使用每条数据的唯一标识作为key，比如id、手机号、身份证号、学号等唯一值
        2.如果不存在对数据的逆序添加、逆序删除等破坏顺序的操作，仅用于渲染列表，使用index作为key是没有问题的
-->
```

3.列表过滤

```html
<body>
    <div id="root">
        <!-- 遍历数组 -->
        <h2>人员列表</h2>
        <input type="text" placeholder="请输入名字" v-model="keyWord">
        <ul>
            <li v-for="(p,index) in filPersons" :key="index">
                {{p.name}}-{{p.age}}-{{p.sex}}
            </li>
        </ul>
    </div>
    <script>
        // 用watch实现
        // #region
        // new Vue({
        //     el:'#root',
        //     data:{
        //         keyWord:'',
        //         persons:[
        //             {id:'001',name:'马冬梅',age:18,sex:'女'},
        //             {id:'002',name:'周冬雨',age:19,sex:'女'},
        //             {id:'003',name:'周杰伦',age:15,sex:'男'},
        //             {id:'004',name:'温兆伦',age:35,sex:'男'}
        //         ],
        //         filPersons:[]
        //     },
        //     watch:{
        //         keyWord:{
        //             immediate:true,
        //             handler(val){
        //             this.filPersons=this.persons.filter((p)=>{
        //                 return p.name.indexOf(val) !==-1
        //             })
        //         }
        //         }
        //     }
        // })
        //#endregion
    
        // 用computed实现
        new Vue({
            el:'#root',
            data:{
                keyWord:'',
                persons:[
                    {id:'001',name:'马冬梅',age:18,sex:'女'},
                    {id:'002',name:'周冬雨',age:19,sex:'女'},
                    {id:'003',name:'周杰伦',age:15,sex:'男'},
                    {id:'004',name:'温兆伦',age:35,sex:'男'}
                ]
            },
            computed:{
                filPersons(){
                        return this.persons.filter((p)=>{
                        return p.name.indexOf(this.keyWord) !==-1
                    })
                }
            }
        })
    </script>
</body>
```

4. 列表排序

```html
<body>
    <div id="root">
        <!-- 遍历数组 -->
        <h2>人员列表</h2>
        <input type="text" placeholder="请输入名字" v-model="keyWord">
        <button @click="sortType=2">年龄升序</button>
        <button @click="sortType=1">年龄降序</button>
        <button @click="sortType=0">原顺序</button>
        <ul>
            <li v-for="(p,index) in filPersons" :key="p.id">
                {{p.name}}-{{p.age}}-{{p.sex}} 
            </li>
        </ul>
    </div>
    <script>
        // 用computed实现
        new Vue({
            el:'#root',
            data:{
                keyWord:'',
                sortType:0,//0代表原顺序，1代表降序，2代表升序
                persons:[
                    {id:'001',name:'马冬梅',age:25,sex:'女'},
                    {id:'002',name:'周冬雨',age:19,sex:'女'},
                    {id:'003',name:'周杰伦',age:15,sex:'男'},
                    {id:'004',name:'温兆伦',age:35,sex:'男'}
                ]
            },
            computed:{
                filPersons(){
                        const arr= this.persons.filter((p)=>{
                        return p.name.indexOf(this.keyWord) !==-1
                    })
                    // 判断一下是否需要排序
                    if(this.sortType){
                        arr.sort((p1,p2)=>{
                            return this.sortType === 1? p2.age-p1.age : p1.age-p2.age
                        })
                    }
                    return arr
                }
            }
        })
    </script>
</body>

```

```html
<!-- 
总结：

1.Vue监视数据的原理：

    vue会监视data中所有层次的数据

2.如何监测对象中的数据？
通过setter实现监视，且要在new Vue时就传入要监测的数据

    1.对象中后追加的属性，Vue默认不做响应式处理
    2.如需给后添加的属性做响应式，请使用如下API：
        Vue.set(target,propertyName/index,value)
        vm.$set(target,propertyName/index,value)

3.如何监测数组中的数据？
通过包裹数组更新元素的方法实现，本质就是做了两件事：

    1.调用原生对应的方法对数组进行更新
    2.重新解析模板，进而更新页面

4在Vue修改数组中的某个元素一定要用如下方法：

    1.使用这些API：push()、pop()、shift()、unshift()、splice()、sort()、reverse()
    2.Vue.set() 或 vm.$set()

特别注意：Vue.set() 和 vm.$set() 不能给vm 或 vm的根数据对象（data等） 添加属性
-->
```

## 13.收集表单数据

```html
    <!-- 
    总结：

    收集表单数据：
    
    若：<input type="text"/>，则v-model收集的是value值，用户输入的内容就是value值
    若：<input type="radio"/>，则v-model收集的是value值，且要给标签配置value属性
    若：<input type="checkbox"/>
    没有配置value属性，那么收集的是checked属性（勾选 or 未勾选，是布尔值）
    配置了value属性：
    v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
    v-model的初始值是数组，那么收集的就是value组成的数组
    v-model的三个修饰符：
    
    1.lazy：失去焦点后再收集数据
    2.number：输入字符串转为有效的数字
    3.trim：输入首尾空格过滤
    -->
```

## 14.过滤器

```html
<body>
    <div id="root">
        <h2>显示格式化后的时间</h2>
        <!-- 计算属性实现 -->
        <h3>现在是:{{fmtTime}}</h3>
        <!-- 方法实现 -->
        <h3>现在是:{{getFmtTime()}}</h3>
        <!-- 过滤器实现 -->
        <h3>现在是:{{time | timeFormater}}</h3>
        <!-- 过滤器传参实现 -->
        <h3>现在是:{{time | timeFormater('YYYY年MM月DD日')}}</h3>
        <!-- 多个过滤器实现 -->
        <h3>现在是:{{time | timeFormater('YYYY年MM月DD日') | mySlice}}</h3>

        <h3 :x="msg | mySlice">尚硅谷</h3>

    </div>

    <div id="root2">
        <h2>{{msg | mySlice}}</h2>
    </div>
    <script>
        // 全局的过滤器
        Vue.filter("mySlice", function (value) {
            // 截取前4个字符
            return value.slice(0, 4);
        })

        new Vue({
            el: '#root',
            data: {
                time: 1666062971472, //时间戳
                msg: '你好,尚硅谷！hi！',
            },
            methods: {
                getFmtTime() {
                    return dayjs(this.time).format('YYYY-MM-DD HH:mm:ss');
                },
            },
            computed: {
                fmtTime() {
                    return dayjs(this.time).format('YYYY-MM-DD HH:mm:ss');
                },
            },
            // 过滤器
            filters: {
                timeFormater(value, str = 'YYYY-MM-DD HH:mm:ss') {
                    return dayjs(value).format(str);
                },
                mySlice(value){
                    // 截取前4个字符
                    return value.slice(0,4);
                }
            }
        })

        new Vue({
            el: '#root2',
            data: {
                msg: 'hello word!'
            }
        })
    </script>
</body>
    <!-- 
    总结：

    过滤器：
    
    定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）。
    
    语法：
    
    注册过滤器：Vue.filter(name,callback) 或 new Vue{filters:{}}
    
    使用过滤器：{{ xxx | 过滤器名}} 或 v-bind:属性 = "xxx | 过滤器名"
    
    备注：
    
    1.过滤器可以接收额外参数，多个过滤器也可以串联
    2.并没有改变原本的数据，而是产生新的对应的数据
    -->
```

## 15.内置指令总结

```html
    <!-- 总结：

    之前学过的指令：
    
    v-bind：单向绑定解析表达式，可简写为 :
    v-model：双向数据绑定
    v-for：遍历数组 / 对象 / 字符串
    v-on：绑定事件监听，可简写为 @
    v-if：条件渲染（动态控制节点是否存存在）
    v-else：条件渲染（动态控制节点是否存存在）
    v-show：条件渲染 (动态控制节点是否展示)
    
    v-text指令：
        作用：向其所在的节点中渲染文本内容
        与插值语法的区别：v-text会替换掉节点中的内容，{{xx}}则不会。

    v-html指令：

        1.作用：向指定节点中渲染包含html结构的内容
        
        2.与插值语法的区别：
        
            1.v-html会替换掉节点中所有的内容，{{xx}}则不会
            2.v-html可以识别html结构

        3.严重注意：v-html有安全性问题！！！
        
            1.在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击
            2.一定要在可信的内容上使用v-html，永远不要用在用户提交的内容上！！！

    v-cloak指令（没有值）：

            1.本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性
            2.使用css配合v-cloak可以解决网速慢时页面展示出{{xxx}}的问题

    v-once指令：

            1.v-once所在节点在初次动态渲染后，就视为静态内容了
            2.以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能

    v-pre指令：

            1.跳过其所在节点的编译过程。
            2.可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译

    -->
```

## 16.自定义指令

```html
    <!-- 
    总结：

    1.自定义指令定义语法：

        1.局部指令：
    
        1.new Vue({															
            directives:{指令名:配置对象}   
        }) 		
    
        2.new Vue({															
            directives:{指令名:回调函数}   
        }) 	
    
    2.全局指令：
    
        1.Vue.directive(指令名,配置对象)
        2.Vue.directive(指令名,回调函数)
        
    例如：
    
    Vue.directive('fbind',{
        //指令与元素成功绑定时（一上来）
        bind(element,binding){
            element.value = binding.value
        },
        //指令所在元素被插入页面时
        inserted(element,binding){
            element.focus()
        },
        //指令所在的模板被重新解析时
        update(element,binding){
            element.value = binding.value
        }
    })
    
    2.配置对象中常用的3个回调函数：
    
        1.bind(element,binding)：指令与元素成功绑定时调用
        2.inserted(element,binding)：指令所在元素被插入页面时调用
        3.update(element,binding)：指令所在模板结构被重新解析时调用

    3.备注：
    
        1.指令定义时不加“v-”，但使用时要加“v-”
    
        2.指令名如果是多个单词，要使用kebab-case命名方式，不要用camelCase命名
    
    new Vue({
        el:'#root',
        data:{
            n:1
        },
        directives:{
            'big-number'(element,binding){
                element.innerText = binding.value * 10
            }
        }
    })
-->
<body>
    <div id="root">
        <h2>当前的n值是:<span v-text="n"></span></h2>
        <h2>放大10倍后的n值是:<span v-big="n"></span></h2>
        <button @click="n++">点击n+1</button>
        <hr>
        <input type="text" v-fbind:value="n">
    </div>
    <script>
        //  需求1：定义一个v-big指令，和v-text功能类似，但会把绑定的数值放大10倍。
        //  需求2：定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点。
        new Vue({
            el: '#root',
            data: {
                n: 1
            },
            directives: {
                //big函数何时会被调用？1.指令与元素成功绑定时（一上来） 2.指令所在的模板被重新解析时
                big(element, binding) {
                    element.innerText = binding.value * 10;
                },
                fbind: {
                    //指令与元素成功绑定时（一上来）就调用
                    bind(element, binding) {
                        element.value = binding.value
                    },
                    //指令所在元素被插入页面时调用
                    inserted(element, binding) {
                        element.focus()
                    },
                    //指令所在的模板被重新解析时调用
                    update(element, binding) {
                        // element.value=binding.value
                    }
                }
            }
        })
    </script>
</body>
```

## 17.生命周期

![img](https://img-blog.csdnimg.cn/img_convert/934db3f17c6daded6578bdf1a769d9dc.png)

```html
    <!-- 
    生命周期：
        又名：生命周期回调函数、生命周期函数、生命周期钩子
        是什么：Vue在关键时刻帮我们调用的一些特殊名称的函数
        生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的
        生命周期函数中的this指向是vm 或 组件实例对象 -->

<!-- 
    总结：

        常用的生命周期钩子：
            mounted：发送ajax请求、启动定时器、绑定自定义事件、订阅消息等初始化操作
            beforeDestroy：清除定时器、解绑自定义事件、取消订阅消息等收尾工作

        关于销毁Vue实例：

            销毁后借助Vue开发者工具看不到任何信息
            销毁后自定义事件会失效，但原生DOM事件依然有效
            一般不会在beforeDestroy操作数据，因为即便操作数据，也不会再触发更新流程了 -->
```

#  Vue组件化编程

## 1.非单文件组件

```html
<body>
    <div id="root">
        <hello></hello>
        <!-- 第三步：编写组件标签 -->
        <school></school>
        <hr>
        <student></student>
    </div>
    <div id="root2">
        <hello></hello>
    </div>
    <script>
        // 第一步：创建school组件
        const school = Vue.extend({
            template: `
                <div>
                    <h2>学校名称:{{schoolName}}</h2>
                    <h2>学校地址:{{address}}</h2>
                    <button @click='showName'>点我提示学校名<button>
                </div>
            `,
            // el: '#root',//组件定义时一定不要写el配置项，
            // 因为最终所有的组件都要被一个vm管理，由vm决定服务哪个容器
            data() {
                return {
                    schoolName: '尚硅谷',
                    address: '广州'
                }
            },
            methods: {
                showName(){
                    alert(this.schoolName)
                }
            },
        })

        // 创建student组件
        const student = Vue.extend({
            template: `
                <div>
                    <h2>学生姓名:{{studentName}}</h2>
                    <h2>学生年龄:{{age}}</h2>
                </div>
            `,
            // el: '#root',//组件定义时一定不要写el配置项，
            // 因为最终所有的组件都要被一个vm管理，由vm决定服务哪个容器
            data() {
                return {
                    studentName: '张三',
                    age: 18
                }
            }
        })

        // 创建hello组件
        const hello=Vue.extend({
            template:`
                <div>
                    <h2>你好呀！{{name}}</h2>
                </div>
            `,
            data(){
                return{
                    name:'Tom'
                }
            }
        })

        // 全局注册hello组件
        Vue.component('hello',hello)

        //创建vm
        new Vue({
            el: '#root',
            // 第二步：注册组件（局部注册）
            components: {
                school,
                student
            }
        });
        //创建vm
        new Vue({
            el: '#root2',
        })
    </script>
</body>
<!-- 
总结：

Vue中使用组件的三大步骤：

    1.定义组件(创建组件)
    2.注册组件
    3.使用组件(写组件标签)

如何定义一个组件？
    使用Vue.extend(options)创建，其中options和new Vue(options)时传入的options几乎一样，但也有点区别：
        el不要写，为什么？
            最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器
        data必须写成函数，为什么？
            避免组件被复用时，数据存在引用关系
        备注：使用template可以配置组件结构
        
如何注册组件？
    1.局部注册：new Vue的时候传入components选项
    2.全局注册：Vue.component('组件名',组件)

编写组件标签：<school></school> -->
```

```html
<!-- 
总结：

关于组件名：
    一个单词组成：
        第一种写法（首字母小写）：school
        第二种写法（首字母大写）：School
    多个单词组成：
        第一种写法（kebab-case命名）：my-school
        第二种写法（CamelCase命名）：MySchool （需要Vue脚手架支持）
    备注：
        组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行
        可以使用name配置项指定组件在开发者工具中呈现的名字

关于组件标签：
    第一种写法：<school></school>
    第二种写法：<school/>
    备注：不使用脚手架时，<school/>会导致后续组件不能渲染

一个简写方式：const school = Vue.extend(options)可简写为：const school = options -->
```

```html
<!-- 
关于VueComponent：

    1.school组件本质是一个名为VueComponent的构造函数，且不是程序员定义的，是Vue.extend生成的

    2.我们只需要写<school/>或<school></school>，
        Vue解析时会帮我们创建school组件的实例对象，即Vue帮我们执行的：new VueComponent(options)

    3.特别注意：每次调用Vue.extend，返回的都是一个全新的VueComponent！

    4.关于this指向：

        1.组件配置中：data函数、methods中的函数、watch中的函数、computed中的函数 
            它们的this均是VueComponent实例对象

        2.new Vue(options)配置中：data函数、methods中的函数、watch中的函数、computed中的函数 
            它们的this均是Vue实例对象

    5.VueComponent的实例对象，以后简称vc（也可称之为：组件实例对象）
        Vue的实例对象，以后简称vm

        只有在本笔记中VueComponent的实例对象才简称为vc -->
```

```html
<!-- 
    一个重要的内置关系：VueComponent.prototype.__proto__ === Vue.prototype
    为什么要有这个关系：让组件实例对象（vc）可以访问到 Vue 原型上的属性、方法 -->
```

## 2.单文件组件

脚手架
