# Vue3快速上手

## 简介-Vue3变化

### 1.性能的提升

- 打包大小减少41%

- 初次渲染快55%, 更新渲染快133%

- 内存减少54%

  ......

### 2.源码的升级

- 使用Proxy代替defineProperty实现响应式

- 重写虚拟DOM的实现和Tree-Shaking

  ......

### 3.拥抱TypeScript

- Vue3可以更好的支持TypeScript

### 4.新的特性

1. Composition API（组合API）

   - setup配置
   - ref与reactive
   - watch与watchEffect
   - provide与inject
   - ......

2. 新的内置组件

   - Fragment 
   - Teleport
   - Suspense

3. 其他改变

   新的生命周期钩子

   data 选项应始终被声明为一个函数

   移除keyCode支持作为 v-on 的修饰符

   

   ## 创建Vue3.0工程

   ### 1.使用 vue-cli 创建

   官方文档：https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create

   ```bash
   ## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
   vue --version
   ## 安装或者升级你的@vue/cli
   npm install -g @vue/cli
   ## 创建
   vue create vue_test
   ## 启动
   cd vue_test
   npm run serve
   ```

   ### 2.使用 vite 创建

   官方文档：https://v3.cn.vuejs.org/guide/installation.html#vite

   vite官网：https://vitejs.cn

   - 什么是vite？—— 新一代前端构建工具。
   - 优势如下：
     - 开发环境中，无需打包操作，可快速的冷启动。
     - 轻量快速的热重载（HMR）。
     - 真正的按需编译，不再等待整个应用编译完成。

   ```bash
   ## 创建工程
   npm init vite-app <project-name>
   ## 进入工程目录
   cd <project-name>
   ## 安装依赖
   npm install
   ## 运行
   npm run dev
   ```

   ## 分析工程结构

   ```js
   
   // 引入的不再是Vue构造函数了，引入的是一个名为createApp的工厂函数
   旧
   //import Vue from 'vue'
   //import App from './App.vue'
   //new Vue({
   //  render: h => h(App),
   //}).$mount('#app')
   
   新
   import { createApp } from 'vue'
   import App from './App.vue'
   
   // 创建应用实例对象-
   const app=createApp(App)
   
   // 挂载
   app.mount('#app')
   ```

   注： <!-- Vue3组件中的模板结构可以没有根标签 -->

## 常用 （组合式）Composition API

