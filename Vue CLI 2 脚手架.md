# Vue CLI 2 脚手架



## 使用Vue CLI脚手架

3.1. 初始化脚手架

​    3.1.1. 说明
​        Vue 脚手架是 Vue 官方提供的标准化开发工具（开发平台）
​        最新的版本是 4.x
​        文档：Vue CLI
​    3.1.2. 具体步骤
​        如果下载缓慢请配置 npm 淘宝镜像：npm config set registry http://registry.npm.taobao.org
​        全局安装@vue/cli：npm install -g @vue/cli
​        切换到你要创建项目的目录，然后使用命令创建项目：vue create xxxx
​        选择使用vue的版本
​        启动项目：npm run serve
​        暂停项目：Ctrl+C

​    Vue 脚手架隐藏了所有 webpack 相关的配置，若想查看具体的 webpakc 配置，请执行：
​    vue inspect > output.js

## 脚手架文件结构
.文件目录
├── node_modules 
├── public
│   ├── favicon.ico: 页签图标
│   └── index.html: 主页面
├── src
│   ├── assets: 存放静态资源
│   │   └── logo.png
│   │── component: 存放组件
│   │   └── HelloWorld.vue
│   │── App.vue: 汇总所有组件
│   └── main.js: 入口文件
├── .gitignore: git版本管制忽略的配置
├── babel.config.js: babel的配置文件
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件
└── package-lock.json: 包版本控制文件

## 关于不同版本的函数

​    vue.js 与 vue.runtime.xxx.js的区别：
​        1.vue.js 是完整版的 Vue，包含：核心功能+模板解析器
​        2.vue.runtime.xxx.js 是运行版的 Vue，只包含核心功能，没有模板解析器

​    因为 vue.runtime.xxx.js 没有模板解析器，所以不能使用 template 配置项，
​        需要使用 render函数接收到的createElement 函数去指定具体内容



## 修改默认配置
​    1.vue.config.js 是一个可选的配置文件，如果项目的（和 package.json 同级的）根目录中存在这个文件，
​        那么它会被 @vue/cli-service 自动加载
​    2.使用 vue.config.js 可以对脚手架进行个性化定制，详见配置参考 | Vue CLI



## ref属性

1. 被用来给元素或子组件注册引用信息（==id的替代者==）
  ​    2.应用在html标签上获取的是真实DOM元素，应用在组件标签上获取的是组件实例对象（vc）
  ​    3.使用方式：
  ​        打标识：<h1 ref="xxx"></h1> 或 <School ref="xxx"></School>
  ​        获取：this.$refs.xxx

```vue
//在App.vue中
<template>
    <div>
        <h1 v-text="msg" ref="title"></h1>
        <button @click="showDOM" ref="btn">点我输出上方的DOM元素</button>
        <school ref="sch"/>
    </div>
</template>

<script>
import School from './components/School.vue'
export default {
    name:'App',
    components: { School },
    data(){
        return{
            msg:'欢迎来到尚硅谷学习！'
        }
    },
    methods: {
        showDOM(){
            console.log(this.$refs.title)   //真实DOM元素
            console.log(this.$refs.btn)     //真实DOM元素
            console.log(this.$refs.sch)     //School组件的实例对象（vc）
        }
    },
}
</script>
```



## ==props配置项==

​    1.功能：让组件接收外部传过来的数据
​    2.传递数据：<Demo name="xxx"/>
​    3.接收数据：
​        第一种方式（只接收）：
​            props:['name']
​        第二种方式（限制数据类型）：
​            props:{name:String}
​        第三种方式（限制类型、限制必要性、指定默认值）：
​            props:{
​                name:{
​                    type:String, //类型
​                    required:true, //必要性
​                    default:'JOJO' //默认值
​                }
​            }
​    备注：props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，
​            若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据

```vue
//在App.vue中
<template>
    <div>
        //给子组件传递数据
        <student name='李四' sex='男' age="18"/>
    </div>
</template>

<script>
import Student from './components/Student.vue'
export default {
    name:'App',
    components: { Student },
}
</script>

//在子组件中接收数据
<template>
    <div>
        //展示父组件给子组件传递的数据
        <h2>学生名称：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>
        <h2>学生年龄：{{age}}</h2>
    </div>
</template>
<script>
    //简单声明接收
    props:['name','sex','age']
</script>
```



## mixin（混入）

​    1.功能：可以把多个组件共用的配置提取成一个混入对象
​    2.使用方式：
​        1.定义混入：
​            const mixin = {
​            data(){....},
​            methods:{....}
​            ....
​        }

```js
//在mixin.js中定义混入
export const hunhe={
    methods:{
        showName(){
            alert(this.name)
        }
    },
    mounted(){
        console.log('你好啊！')
    },
};

export const hunhe2={
    data(){
        return{
            x:100,
            y:200
        }
    }
}
```

​        2.使用混入：
​            全局混入：Vue.mixin(xxx)
​       

```js
//在main.js中
// 导入mixin
import {hunhe,hunhe2} from './mixin'
// 注册mixin
Vue.mixin(hunhe)
Vue.mixin(hunhe2)
```

​     局部混入：mixins:['xxx']

```vue
//在子组件中
<script>
// 引入一个hunhe
import { hunhe,hunhe2 } from "../mixin";
export default {
    name: "School-1",
    data() {
        return {
        };
    },
    mixins:[hunhe,hunhe2]
};
</script>
```

​    3.备注：
​        1.组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”，在发生冲突时以组件优先。
​        2.同名生命周期钩子将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子之前调用。

------



## 插件

​    1.功能：用于增强Vue

​    2.本质：包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据

​    3.定义插件：
​        plugin.install = function (Vue, options) {
​            // 1. 添加全局过滤器
​            Vue.filter(....)
​    
​            // 2. 添加全局指令
​            Vue.directive(....)
​    
​            // 3. 配置全局混入
​            Vue.mixin(....)
​    
​            // 4. 添加实例方法
​            Vue.prototype.$myMethod = function () {...}
​            Vue.prototype.$myProperty = xxxx
​        }

```js
//在plugins.js中
export default {
    install(Vue) {
        // 添加全局过滤器
        Vue.filter("mySlice", function (value) {
        // 截取前4个字符
            return value.slice(0, 4);
        });
        //  添加全局指令
        Vue.directive('fbind',{
            //指令与元素成功绑定时（一上来）
            bind(element,binding){
                element.value = binding.value
            },
            //指令所在元素被插入页面时
            inserted(element,binding){
                element.focus()
                console.log(binding)
            },
            //指令所在的模板被重新解析时
            update(element,binding){
                element.value = binding.value
            },
        });

        // 配置全局混入
        Vue.mixin({
            data(){
                return{
                    x:100,
                    y:200
                }
            },
        })

        //添加实例方法
        // 给Vue原型上添加一个方法（vm和vc就都能用了）
        Vue.prototype.hello=()=>{alert('你好呀！')}

    },
};
```

​    4.使用插件：Vue.use(plugin)

```js
//在main.js中
// 引入插件
import plugins from './plugins'
// 应用（使用）插件
Vue.use(plugins)
```




## scoped样式

作用：让样式在局部生效，防止冲突
​    写法：<style scoped>
​    scoped样式一般不会在App.vue中使用

```vue
//在子组件中
<style lang="less" scoped>
    .demo{
        background-color: orange;
        .atguigu{
            font-size: 40px;
        }
    }
</style>
```



## 总结TodoList案例

​    组件化编码流程：

​        1.拆分静态组件：组件要按照功能点拆分，命名不要与html元素冲突
​        2.实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用：
​            1.一个组件在用：放在组件自身即可
​            2.一些组件在用：放在他们共同的父组件上（状态提升）
​        3.实现交互：从绑定事件开始

​    props适用于：
​        1.父组件 ==> 子组件 通信
​        2.子组件 ==> 父组件 通信（要求父组件先给子组件一个函数）

```vue
//在App.vue中，先传递给子组件一个函数
<MyHeader :addTodo='addTodo'/>
//在MyHeader组件中接收
props:["addTodo"]
```

​    使用v-model时要切记：v-model绑定的值不能是props传过来的值，因为props是不可以修改的

​    props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做

------



## webStorage

​    1.存储内容大小一般支持5MB左右（不同浏览器可能还不一样）

​    2.浏览器端通过 Window.sessionStorage 和 Window.localStorage 属性来实现本地存储机制。

​    3.相关API：

​        1.xxxxxStorage.setItem('key', 'value');
​        该方法接受一个键和值作为参数，会把键值对添加到存储中，如果键名存在，则更新其对应的值。

​        2.xxxxxStorage.getItem('person');
​        该方法接受一个键名作为参数，返回键名对应的值。

​        3.xxxxxStorage.removeItem('key');
​        该方法接受一个键名作为参数，并把该键名从存储中删除。

​        4.xxxxxStorage.clear()
​        该方法会清空存储中的所有数据。

​    4.备注：
​        1.SessionStorage存储的内容会随着浏览器窗口关闭而消失。
​        2.LocalStorage存储的内容，需要手动清除才会消失。
​        3.xxxxxStorage.getItem(xxx)如果xxx对应的value获取不到，那么getItem的返回值是null。
​        4.JSON.parse(null)的结果依然是null。

------



## ==组件的自定义事件==

​    1.一种组件间通信的方式，适用于：==子组件== ===> 父组件

​    2.使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A(父)中给B（子）绑定自定义事件（事件的回调在A（父）中）。

​    3.绑定自定义事件：

​        1.第一种方式，在父组件中：<Demo @atguigu="test"/>或 <Demo v-on:atguigu="test"/>

​        2.第二种方式，在父组件中：
​            <Demo ref="demo"/>
​            ......
​            mounted(){
​            this.$refs.xxx.$on('atguigu',this.test)
​            }

​        3.若想让自定义事件只能触发一次，可以使用once修饰符，或$once方法。

​    4.触发自定义事件：this.$emit('atguigu',数据)

​    5.解绑自定义事件this.$off('atguigu')

​    6.组件上也可以绑定原生DOM事件，需要使用native修饰符。

​    7.注意：通过this.$refs.xxx.$on('atguigu',回调)绑定自定义事件时，回调要么配置在methods中，要么用箭头函数，否则this指向会出问题！

```vue
//在App.vue中        
<!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传递数据 (第一种写法，使用@或者v-on)-->
        <student @atguigu='getStudentName' @demo='m1'/>

        <!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传递数据 (第二种写法，使用ref)-->
        <student ref='student'/>
<script>
    mounted(){
        this.$refs.student.$on('atguigu',this.getStudentName)
        // this.$refs.student.$once('atguigu',this.getStudentName)
    }
</script>

//在子组件Student中
    <button @click="sendStudentName">把学生名给App</button>
	<button @click="unbind">解绑atguigu事件</button>
<script>
    methods: {
        sendStudentName(){
            // 触发Student组件实例身上的atguigu事件
            this.$emit('atguigu',this.name,66,55,88)
            // this.$emit('demo')
        },
        unbind(){
            this.$off('atguigu')//解绑一个自定义事件
            // this.$off(['atguigu','demo'])//解绑多个自定义事件
            // this.$off()//解绑所有的自定义事件
        }
    },
 </script>
```



------



## ==全局事件总线（GlobalEventBus）==

​    全局事件总线是一种==可以在任意组件间通信的方式==，本质上就是一个对象。它必须满足以下条件：
​        1. 所有的组件对象都必须能看见他 
​        2. 这个对象必须能够使用$on、$emit和$off方法去绑定、触发和解绑事件

​    1.一种组件间通信的方式，适用于任意组件间通信

​    2.安装全局事件总线：

​        new Vue({
​            ...
​            beforeCreate() {
​                Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
​            },
​            ...
​        }) 

​    3.使用事件总线：

​        1.接收数据：A组件想接收数据，则在A组件中给==$on绑定自定义事件==，事件的回调留在A组件自身

​        export default {
​            methods(){
​                demo(data){...}
​            }
​            ...
​            mounted() {
​                this.$bus.$on('xxx',this.demo)
​            }
​        }

​        2.提供数据：this.$bus.$emit('xxx',data)

​    4.最好在beforeDestroy钩子中，用$off去解绑当前组件所用到的事件

//完成school组件接收，student组件发送的通信

```js
//在main.js中
// 创建vm
new Vue({
    el:'#app',
    render:h=>h(App),
    // 安装全局事件总线
    beforeCreate() {
        Vue.prototype.$bus= this
    },
})
```

```vue
//在school组件中
<script>
export default {
    mounted(){
        console.log('School',this)
        this.$bus.$on('hello',(data)=>{
            console.log('School组件收到了数据',data)
        })
    },
    beforeDestroy(){
        this.$bus.$off('hello')
    }
};
</script>

```

```vue
//在student组件中
<button @click="sendStudentName">把学生名给School组件</button>
<script>
export default {
    methods:{
        sendStudentName(){
            this.$bus.$emit('hello',this.name)
        }
    }
}
</script>
```



------



## 消息订阅与发布（pubsub）

​    1.消息订阅与发布是一种组件间通信的方式，适用于任意组件间通信

​    2.使用步骤：

​        1.安装pubsub：npm i pubsub-js

​        2.引入：import pubsub from 'pubsub-js'

​        3.接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身

​        export default {
​            methods(){
​                demo(data){...}
​            }
​            ...
​            mounted() {
​                this.pid = pubsub.subscribe('xxx',this.demo)
​            }
​        }

```vue
<script>
//引入
import pubsub from 'pubsub-js'
export default {
    mounted(){
        // 订阅消息
        this.pubId=pubsub.subscribe('hello',(msgName,data)=>{
            console.log('有人发布了hello消息,hello消息的回调执行了',msgName,data)
        })
    },
    beforeDestroy(){
        // 消息的取消订阅，利用订阅ID取消订阅
        pubsub.unsubscribe(this.pubId)
    }
};
</script>
```

​        4.提供数据：pubsub.publish('xxx',data)

```vue
<script>
import pubsub from 'pubsub-js'
export default {
    methods:{
        sendStudentName(){
            pubsub.publish('hello',666)
        }
    }
}
</script>
```

​        5.最好在beforeDestroy钩子中，使用pubsub.unsubscribe(pid)取消订阅

------



## $nextTick

1. 语法：this.$nextTick(回调函数)
​    2.作用：在下一次 DOM 更新结束后执行其指定的回调
​    3.什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行

------



## Vue封装的过度与动画

​    1.作用：在插入、更新或移除 DOM元素时，在合适的时候给元素添加样式类名
​    2.图示：

​    3.写法：

​        1.准备好样式：
​            元素进入的样式：
​                v-enter：进入的起点
​                v-enter-active：进入过程中
​                v-enter-to：进入的终点
​            元素离开的样式：
​                v-leave：离开的起点
​                v-leave-active：离开过程中
​                v-leave-to：离开的终点
​        2.使用<transition>包裹要过度的元素，并配置name属性：
​            <transition name="hello">
​                <h1 v-show="isShow">你好啊！</h1>
​            </transition>
​        3.备注：若有多个元素需要过度，则需要使用：<transition-group>，且每个元素都要指定key值

------



# Vue中的Ajax

## vue脚手架配置代理服务器

​    方法一：在vue.config.js中添加如下配置：

devServer:{
​            proxy:"http://localhost:5000"
​        }

​    说明：

 1.优点：配置简单，请求资源时直接发给前端即可
​        2.缺点：不能配置多个代理，不能灵活的控制请求是否走代理
​        3.工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器 （优先匹配前端资源）

​    方法二：
​        devServer: {
​            proxy: {
​                '/api1': { // 匹配所有以 '/api1'开头的请求路径
​                    target: 'http://localhost:5000',// 代理目标的基础路径
​                    changeOrigin: true,
​                    pathRewrite: {'^/api1': ''}
​                },
​                '/api2': { // 匹配所有以 '/api2'开头的请求路径
​                    target: 'http://localhost:5001',// 代理目标的基础路径
​                    changeOrigin: true,
​                    pathRewrite: {'^/api2': ''}
​                }
​            }
​        }
​        // changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
​        // changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
​        说明：
​            1.优点：可以配置多个代理，且可以灵活的控制请求是否走代理
​            2.缺点：配置略微繁琐，请求资源时必须加前缀

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  // 开启代理服务器(方式一)
  // devServer: {
  //   proxy: 'http://localhost:5000'
  // },
  // 开启代理服务器(方式二)
  devServer: {
    proxy: {
      '/atguigu': {
        target: 'http://localhost:5000',
        pathRewrite:{'^/atguigu':''},
        ws: true,//websocket
        changeOrigin: true
      },
      '/demo': {
        target: 'http://localhost:5001',
        pathRewrite:{'^/demo':''},
        ws: true,//websocket
        changeOrigin: true
      },
    }
  }
})
```

------



## vue项目常用的两个Ajax库

​    1.axios：通用的Ajax请求库，官方推荐，效率高
​    2.vue-resource：vue插件库，vue 1.x使用广泛，官方已不维护

------



## 插槽

​    1.作用：让父组件可以向子组件指定位置插入html结构，也是一种组件间通信的方式，适用于==父组件 > 子组件

​    2.分类：默认插槽、具名插槽、作用域插槽

​    3.使用方式：

​        1.默认插槽：

​    父组件中：

```vue
//在父组件中引入插槽组件
import Category from './components/Category.vue'
使用：
      <Category>
        <div>html结构1</div>
      </Category>
  子组件中：定义插槽
      <template>
         <div>
           <slot>插槽默认内容...</slot>
         </div>
      </template>
```

​        2.具名插槽：

```vue
//在插槽子组件中定义带有name名字的插槽
<template>
    <div class="category">
        <!-- 定义一个插槽(制造一个凹槽，等着组件的使用者进行填充) -->
        <slot name="center"></slot>
        <slot name="footer"></slot>
    </div>
</template>
//在父组件中引入插槽组件
import Category from './components/Category.vue'
<template>
        <div class="container">
            <Category title="美食">
                //插入内容的时候要带上插槽的名字
                <img slot="center" src="https://s3.ax1x.com/2021/01/16/srJIq0.jop" alt="">
                <div slot="footer" class="foot">
                    <a  href="http://www.baidu.com">更多美食</a>
                </div>
            </Category>
        </div>
</template>
```

​        3.作用域插槽：

​            理解：数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定。（games数据在Category组件中，但使用数据所遍历出来的结构由App组件决定）

​     具体编码：
​            父组件中：

```vue
<template>
        <div class="container">
            
            <Category title="游戏">
                <template scope="atguigu">
                    <!-- 生成的是ul列表 -->
                    <ul>
                        <li v-for="(g,index) in atguigu.games" :key="index">{{g}}</li>
                    </ul>
                </template>
            </Category>
            <Category title="游戏">
                    <template scope="atguigu">
						<!-- 生成的是h4标题 -->
                        <h4 v-for="(g,index) in atguigu.games" :key="index">{{g}}</h4>
                    </template>
            </Category>
        </div>
</template>
```

​        插槽子组件中：

```vue
            
                <template>
                    <div class="category">
                        <slot :games="games"></slot>
                    </div>
                </template>
                <script>
                    export default {
                        name:'Category',
                        props:['title'],
                        //数据在子组件自身
                        data() {
                            return {
                                games:['红色警戒','穿越火线','劲舞团','超级玛丽']
                            }
                        },
                    }
                </script>
```
------



# ==Vuex==

![vuex](https://vuex.vuejs.org/vuex.png)

## Vuex的基本使用



```js
在main.js中
    // 引入store
    import store from './store/index'
    // 创建vm
    new Vue({
        el:'#app',
        render:h=>h(App),
        //挂载仓库
        store,
    })
```

   

```js
在store文件夹下创建index.js
// 该文件用于创建Vuex中最为核心的store
import Vue from 'vue'
// 引入Vuex
import Vuex from 'vuex'
// 使用Vuex插件
Vue.use(Vuex)
// 准备actions-用于响应组件中的动作
const actions={
    jiaOdd(context,value){
        if(context.state.sum%2){
            context.commit('JIA',value)
        }
    },
    jiaWait(context,value){
        setTimeout(()=>{
            context.commit('JIA',value)
        },500)
    },
}
// 准备mutations-用于操作数据（state）
const mutations={
    JIA(state,value){
        state.sum+=value
    },
    JIAN(state,value){
        state.sum-=value
    },
}
// 准备state-用于存储数据
const state={
    // 当前的和
    sum:0,
}
// 准备getters-用于将status中的数据进行加工
const getters={
    bigSum(state){
        return state.sum*10
    }
}

// 创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state,
    getters
})
```

```vue
//在组件中使用vuex
<template>
    <div>
        //读取仓库中的数据sum
        <h2>当前求和为:{{$store.state.sum}}</h2>
        //使用仓库中的bigSum方法队数据sum进行加工
        <h3>当前求和放大10倍为:{{$store.getters.bigSum}}</h3>
        <select v-model.number="n">
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
        </select>
        <button @click="increment">+</button>
        <button @click="decrement">-</button>
        <button @click="incrementOdd">当前求和为奇数再加</button>
        <button @click="incrementWait">等一等再加</button>
    </div>
</template>

<script>
export default {
    name:'Count-1',
    data(){
        return{
            // 用户选择的数字
            n:1
        }
    },
    methods: {
        increment(){
            //dispatch派发请求
            // this.$store.dispatch('jia',this.n)
            //commit提交请求
            this.$store.commit('JIA',this.n)
        },
        decrement(){
            // this.$store.dispatch('jian',this.n)
            this.$store.commit('JIAN',this.n)
        },
        incrementOdd(){
            this.$store.dispatch('jiaOdd',this.n)
        },
        incrementWait(){
            this.$store.dispatch('jiaWait',this.n)
        }
    },
}
</script>
```

1.初始化数据state，配置actions、mutations，操作文件store.js

​	//==actions==-用于响应组件中的动作

​	// ==mutations==-用于操作数据（state）

​	// ==state==-用于存储数据

​	// ==getters==-用于将status中的数据进行加工

2.组件中==读取vuex中的数据：====$store.state==.sum

3.组件中修改vuex中的数据：$store.dispatch('action中的方法名',数据) 或 $store.commit('mutations中的方法名',数据)

4.备注：==若没有网络请求或其他业务逻辑==，组件中也可以越过actions，即不写dispatch，直接编写commit

------



## ==map方法的使用==

​    1.mapState方法：用于帮助我们映射state中的数据为计算属性

​    2.mapGetters方法：用于帮助我们映射getters中的数据为计算属性


​    3.mapActions方法：用于帮助我们生成与actions对话的方法，即：包含$store.dispatch(xxx)的函数

​    4.mapMutations方法：用于帮助我们生成与mutations对话的方法，即：包含$store.commit(xxx)的函数

在count组件中

```vue
<template>
    <div>
        //简化
        <!-- <h2>当前求和为:{{$store.state.sum}}</h2>
        <h3>当前求和放大10倍为:{{$store.getters.bigSum}}</h3> -->
        <h2>当前求和为:{{sum}}</h2>
        <h3>当前求和放大10倍为:{{bigSum}}</h3>
        <h3>我在{{school}},学习{{subject}}</h3>
        <select v-model.number="n">
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
        </select>
        <button @click="increment(n)">+</button>
        <button @click="decrement(n)">-</button>
        <button @click="incrementOdd(n)">当前求和为奇数再加</button>
        <button @click="incrementWait(n)">等一等再加</button>
    </div>
</template>
<script>
import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'
export default {
    name:'Count-1',
    data(){
        return{
            // 用户选择的数字
            n:1
        }
    },
    methods: {
        // increment(){
        //     this.$store.commit('JIA',this.n)
        // },
        // decrement(){
        //     this.$store.commit('JIAN',this.n)
        // },

        // 借助mapMutations方法生成对应的方法，方法中会调用commit去联系mutations（对象写法）
        ...mapMutations({increment:'JIA',decrement:'JIAN'}),

        // --------------------------------------------
        // incrementOdd(){
        //         this.$store.dispatch('jiaOdd',this.n)
        // },
        // incrementWait(){
        //     this.$store.dispatch('jiaWait',this.n)
        // }

        // 借助mapActions方法生成对应的方法，方法中会调用dispatch去联系actions（对象写法）
        ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
    },
    computed:{

        // 借助mapState自动生成的计算属性，从state中读取数据，（对象写法）
        // ...mapState({he:'sum',xuexiao:'school',xueke:'subject'}),

        // 借助mapState自动生成的计算属性，从state中读取数据，（数组写法）
        ...mapState(['sum','school','subject']),

        // --------------------------------------------------------------
        
        // 借助mapGetters自动生成的计算属性，从getters中读取数据，（数组写法）
        ...mapGetters(['bigSum']),
    },
}
</script>
```

​    5.备注：mapActions与mapMutations使用时，若需要传递参数，则需要在模板中绑定事件时传递好参数，否则参数是事件对象

------



## 模块化+命名空间：

​    1.目的：让代码更好维护，让多种数据分类更加明确

​    2.修改store.js：

```js
// 求和功能相关的配置
const countOptions={
    //开启命名空间
    namespaced:true,
    actions:{
    },
    mutations:{
    },
    state:{
    }
}
// 人员管理相关的配置
const personOptions={
    //开启命名空间
    namespaced:true,
    actions:{
    },
    mutations:{
    },
    state:{
    },
    getters:{
    }
}

// 创建并暴露store
export default new Vuex.Store({
    modules:{
        countAbout:countOptions,
        personAbout:personOptions
    }
})
```

3.开启命名空间后，组件中读取state数据：

//方式一：自己直接读取
​        this.$store.state.personAbout.list

//方式二：借助mapState读取：
​        ...mapState('countAbout',['sum','school','subject']),

```vue
<script>
import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'
export default {
    methods: {
        // 借助mapMutations方法生成对应的方法，方法中会调用commit去联系mutations（对象写法）
        ...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
        // 借助mapActions方法生成对应的方法，方法中会调用dispatch去联系actions（对象写法）
        ...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
    },
    computed:{
        // 借助mapState自动生成的计算属性，从state中读取数据，（数组写法）
        ...mapState('countAbout',['sum','school','subject']),
        ...mapState('personAbout',['personList']),
        
        // 借助mapGetters自动生成的计算属性，从getters中读取数据，（数组写法）
        ...mapGetters('countAbout',['bigSum']),
    }
}
</script>
```

4.开启命名空间后，组件中读取getters数据：

//方式一：自己直接读取
​        this.$store.getters['personAbout/firstPersonName']
​        //方式二：借助mapGetters读取：
​        ...mapGetters('countAbout',['bigSum'])

5.开启命名空间后，组件中调用dispatch：

//方式一：自己直接dispatch
​        this.$store.dispatch('personAbout/addPersonWang',person)
​        //方式二：借助mapActions：
​        ...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})

6.开启命名空间后，组件中调用commit：

//方式一：自己直接commit
​        this.$store.commit('personAbout/ADD_PERSON',person)
​        //方式二：借助mapMutations：
​        ...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),

# ==路由器==

## Vue Router路由管理器
​    6.1 相关理解
​        6.1.1 vue-router的理解
​            vue 的一个插件库，专门用来实现SPA 应用
​        6.1.2 对SPA应用的理解
​            1.单页 Web 应用（single page web application，SPA）
​            2.整个应用只有一个完整的页面
​            3.点击页面中的导航链接不会刷新页面，只会做页面的局部更新
​            4.数据需要通过ajax请求获取


​        6.1.3 路由的理解
​            1.什么是路由?
​                1.一个路由就是一组映射关系（key - value）
​                2.key 为路径，value 可能是 function 或 componen

​            2.路由分类

​                后端路由：
​                    理解：value 是 function，用于处理客户端提交的请求
​                    工作过程：服务器接收到一个请求时，根据请求路径找到匹配的函数来处理请求，返回响应数据

​                前端路由：
​                    理解：value 是 component，用于展示页面内容
​                    工作过程：当浏览器的路径改变时，对应的组件就会显示

------



## 路由的基本使用

![image-20230306165906089](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230306165906089.png)

​    1.安装vue-router，命令：npm i vue-router

​    2.应用插件：Vue.use(VueRouter)

```js
在main.js中
// 引入Vue
import Vue from 'vue'
// 引入App
import App from './App.vue'
// 引入VueRouter
import VueRouter from 'vue-router'
// 引入路由器
import router from './router'
// 关闭Vue的生产提示
Vue.config.productionTip=false
// 应用插件VueRouter
Vue.use(VueRouter)
// 创建vm
new Vue({
    el:'#app',
    render:h=>h(App),
    router:router
})
```

​    3.编写router配置项：

```js
在router的index.js中
// 该文将专门创建整个应用的路由器
import VueRouter from "vue-router"
// 引入组件
import About from '../components/About.vue'
import Home from '../components/Home.vue'

// 创建并暴露一个路由器
export default new VueRouter({
    // 创建一个个路由
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home
        },
    ]
})
```

​    4.实现切换（active-class可配置高亮样式）：

​        <router-link active-class="active" to="/about">About</router-link>

​    5.指定展示位：<router-view></router-view>

```vue
<template>
    <div>
        <div class="row">
            <div class="col-xs-offset-2 col-xs-8">
                <div class="page-header">
                    <h2>Vue Router Demo</h2>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-xs-2 col-xs-offset-2">
                <div class="list-group">
                    <!-- 原始html中我们使用a标签进行页面的跳转 -->
                    <!-- <a class="list-group-item active" href="./about.html">About</a>
                    <a class="list-group-item" href="./home.html">Home</a> -->

                    <!-- Vue中借助router-link实现路由的切换 -->
                    <router-link class="list-group-item" active-class="active" to="/about">About</router-link>
                    <router-link class="list-group-item" active-class="active" to="/home">Home</router-link>
                </div>
            </div>
            <div class="col-xs-6">
                <div class="panel">
                    <div class="panel-body">
                        <!-- 指定组件的呈现位置 -->
                        <router-view></router-view>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
```

------



## 路由器的几个注意事项

1.路由组件通常存放在pages文件夹，一般组件通常存放在components文件夹
    	2.通过切换，“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载
    	3.每个组件都有自己的$route属性，里面存储着自己的路由信息
   	 4.整个应用只有一个router，可以通过组件的$router属性获取到

## 多级路由

![image-20230306171126030](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230306171126030.png)

​    1.配置路由规则，使用children配置项：

在router的index.js文件中

```js
// 该文将专门创建整个应用的路由器
import VueRouter from "vue-router"
// 引入组件
import About from '../pages/About.vue'
import Home from '../pages/Home.vue'
import News from '../pages/News.vue'
import Message from '../pages/Message.vue'
// 创建并暴露一个路由器
export default new VueRouter({
    // 创建一个个路由
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',
                    component:News
                },
                {
                    path:'message',
                    component:Message
                }
            ]
        },
    ]
})
```

​    2.跳转（要写完整路径）：<router-link to="/home/news">News</router-link>

------



## 路由的==query==参数

```vue
                <!-- 跳转路由并携带query参数 ，to的字符串写法-->
                <!-- <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{m.title}}</router-link>&nbsp;&nbsp; -->
                
                <!-- 跳转路由并携带query参数 ，to的对象写法-->
                <router-link :to="{
                    path:'/home/message/detail',
                    query:{
                        id:m.id,
                        title:m.title
                    }
                }">
                    {{m.title}}
                </router-link>
```

​    2.接收参数：

```vue
detail组件中
<template>
    <ul>
        <li>消息编号：{{$route.query.id}}</li>
        <li>消息标题：{{$route.query.title}}</li>
    </ul>
</template>
```

------



## 命名路由

​    1.作用：可以简化路由的跳转

​    2.如何使用：

```js
        1.给路由命名：
​        {
​            path:'/demo',
​            component:Demo,
​            children:[
​                {
​                    path:'test',
​                    component:Test,
​                    children:[
​                        {
​                            name:'hello' //给路由命名
​                            path:'welcome',
​                            component:Hello,
​                        }
​                    ]
​                }
​            ]
​        }
```



```vue
                <router-link :to="{
                    // 标准path写法
                    // path:'/home/message/detail',
                    // 使用name属性写法，可以简化path，只需要给路由规则起名字
                    name:'xiangqing',
                    query:{
                        id:m.id,
                        title:m.title
                    }
                }">
                    {{m.title}}
                </router-link>
```

​        2.简化跳转：

```vue
        <!--简化前，需要写完整的路径 -->
​        <router-link to="/demo/test/welcome">跳转</router-link>

​        <!--简化后，直接通过名字跳转 -->
​        <router-link :to="{name:'hello'}">跳转</router-link>

​        <!--简化写法配合传递参数 -->
​        <router-link 
​            :to="{
​                name:'hello',
​                query:{
​                    id:666,
​                    title:'你好'
​                }
​            }"
​        >跳转</router-link>
```



------



##  路由的==params==参数

​    1.配置路由，声明接收params参数：

```js
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',
                    component:News
                },
                {
                    component:Message,
                    children:[
                        {
                            name:'xiangqing',
                            path:'detail/:id/:title', //使用占位符声明接收params参数
                            component:Detail
                        }
                    ]
                }
            ]
        }
```


​    2.传递参数：

```vue
                <!-- 跳转路由并携带params参数 ，to的字符串写法-->
                <!-- <router-link :to="`/home/message/detail/${m.id}/${m.title}`">
                    {{m.title}}
                </router-link>&nbsp;&nbsp; -->
                
                <!-- 跳转路由并携带params参数 ，to的对象写法-->
                <router-link :to="{
                    name:'xiangqing',
                    params:{
                        id:m.id,
                        title:m.title
                    }
                }">
                    {{m.title}}
                </router-link>
```

​        ==特别注意==：路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置！

​    3.接收参数：

 $route.params.id
​        $route.params.title

------



## 路由的props配置

​    1.作用：让路由组件更方便的收到参数
​        {
​            name:'xiangqing',
​            path:'detail/:id',
​            component:Detail,

​            //第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件
​            // props:{a:900}

​            //第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
​            // props:true
​            
​            //第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
​            props(route){
​                return {
​                    id:route.query.id,
​                    title:route.query.title
​                }
​            }
​        }

```js
                {
                    path:'message',
                    component:Message,
                    children:[
                        {
                            name:'xiangqing',
                            path:'detail/:id/:title',//使用占位符声明接收params参数
                            component:Detail,
                            //props的第一种写法，值为对象，该对象中的所有key-value都会以props的形式传给Detail组件。
                            // props:{a:1,b:'hello'}

                            //props的第二种写法，值为布尔值，若布尔值为真，
                                // 就会把该路由组件收到的所有params参数，以props的形式传给Detail组件。
                            // props:true

                            //props的第三种写法，值为函数
                            props($route){
                                return {
                                    id:$route.params.id,
                                    title:$route.params.title
                                }
                            }
                        }
                    ]
                }
```

------



## 路由跳转的replace方法

​    1.作用：控制路由跳转时操作浏览器历史记录的模式
​    2.浏览器的历史记录有两种写入方式：push和replace，其中push是追加历史记录，replace是替换当前记录。路由跳转时候默认为push方式
​    3.开启replace模式：<router-link replace >News</router-link>

------



## ==编程式路由导航==

​    1.作用：不借助<router-link>实现路由跳转，让路由跳转更加灵活

​    2.具体编码：
​        this.$router.forward() //前进
​        this.$router.back() //后退
​        this.$router.go() //可前进也可后退,看传入的数字是正数(前进)还是负数(后退)

```vue
                <button @click="pushShow(m)">push查看</button>
                <button @click="replaceShow(m)">replace查看</button>
<script>
    export default {
        methods:{
            pushShow(m){
                this.$router.push({
                    name:'xiangqing',
                    params:{
                        id:m.id,
                        title:m.title
                    }
                })
            },
            replaceShow(m){
                this.$router.replace({
                    name:'xiangqing',
                    params:{
                        id:m.id,
                        title:m.title
                    }
                })
            }
        }
    };
</script>
```

------



## ==缓存路由组件==

​    1.作用：让不展示的路由组件保持挂载，不被销毁

​    2.具体编码：

```vue
            //缓存多个路由组件
            <!-- <keep-alive :include="['News-1','Message-1']"></keep-alive> -->
            //缓存一个路由组件
            <keep-alive include="News-1">//组件名
                <router-view></router-view>
            </keep-alive>
```



------



## 两个新的生命周期钩子 

activated和deactivated

1.activated和deactivated是路由组件所独有的两个钩子，用于捕获路由组件的激活状态
    	2.具体使用：
        	activated路由组件被激活时触发
        	deactivated路由组件失活时触发

![image-20230306183108516](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230306183108516.png)

------



## ==路由守卫==

​    1.作用：对路由进行权限控制

​    2.分类：全局守卫、独享守卫、组件内守卫

​    3.全局守卫：

在router中的index.js中

```js
// 全局前置路由守卫--初始化的时候被调用，每次路由切换之前被调用
router.beforeEach((to,from,next)=>{
    if(to.meta.isAuth){//判断是否需要授权
        if(localStorage.getItem('school')==='atguigu'){
        next()
        }else{
            alert('学校名不对，无权限查看！')
        }
    }else{
        next()
    }
}),

// 全局后置路由守卫--初始化的时候被调用，每次路由切换之后被调用
router.afterEach((to)=>{
    document.title=to.meta.title || '硅谷系统'
})
```

​    4.独享守卫：

```js
        {
            name:'zhuye',
            path:'/home',
            component:Home,
            meta:{title:'主页'},
            children:[
                {
                    name:'xinwen',
                    path:'news',
                    component:News,
                    meta:{isAuth:true,title:'新闻'},
                    // 独享路由守卫
                    beforeEnter:(to,next)=>{
                        if(to.meta.isAuth){//判断是否需要授权
                                    if(localStorage.getItem('school')==='atguigu'){
                                    next()
                                    }else{
                                        alert('学校名不对，无权限查看！')
                                    }
                                }else{
                                    next()
                                }
                    }
                },
            ]
        }
```

​    5.组件内守卫：

```vue
<script>
    export default {
        name:'About-1',
        //通过路由规则，离开该组件时被调用
        beforeRouteEnter (to, from, next) {
            console.log('About--beforeRouteEnter',to,from)
            if(localStorage.getItem('school')==='atguigu'){
                next()
            }else{
                alert('学校名不对，无权限查看！')
            }
        },
        //通过路由规则，离开该组件时被调用
        beforeRouteLeave (to, from, next) {
            console.log('About--beforeRouteLeave',to,from)
            next()
        }
    }
</script>
```

------



## 路由器的两种工作模式

​    1.对于一个url来说，什么是hash值？—— #及其后面的内容就是hash值

​    2.hash值不会包含在 HTTP 请求中，即：hash值不会带给服务器

​    3.hash模式：
​        地址中永远带着#号，不美观
​        若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法
​        兼容性较好

​    4.history模式：
​        地址干净，美观
​        兼容性和hash模式相比略差
​        应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题

```js
// 创建并暴露一个路由器
const router= new VueRouter({
    mode:'history',
    // 创建一个个路由
    routes:[
        ...
    ]
```

