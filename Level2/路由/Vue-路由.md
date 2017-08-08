# Learn Vue

## 1. Vue - 路由

### 1.1 基本介绍

对于大多数单页面应用，都推荐使用官方支持的vue-router库。更多细节可以看vue-router文档。

- 安装
源自文档：

 1. 直接下载 / CDN
https://unpkg.com/vue-router/dist/vue-router.js
Unpkg.com 提供了基于 NPM 的 CDN 链接。上面的链接会一直指向在 NPM 发布的最新版本。你也可以像 https://unpkg.com/vue-router@2.0.0/dist/vue-router.js 这样指定 版本号 或者 Tag。
在 Vue 后面加载 vue-router，它会自动安装的：
```
<script src="/path/to/vue.js"></script>
<script src="/path/to/vue-router.js"></script>
```
 2. NPM
`npm install vue-router`
如果在一个模块化工程中使用它，必须要通过 Vue.use() 明确地安装路由功能：
```
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)
```
如果使用全局的 script 标签，则无须如此（手动安装）。

 3. 构建开发版
如果你想使用最新的开发版，就得从 GitHub 上直接 clone，然后自己 build 一个 vue-router。
```
git clone https://github.com/vuejs/vue-router.git node_modules/vue-router
cd node_modules/vue-router
npm install
npm run build
```

### 1.2 开始

用 Vue.js + vue-router 创建单页应用，是非常简单的。使用 Vue.js 时，我们就已经把组件组合成一个应用了，当你要把 vue-router 加进来，只需要配置组件和路由映射，然后告诉 vue-router 在哪里渲染它们。下面是个基本例子：

[dome1](./learnVue-router/index.html)

具体步骤：
1. 导入Vue和VueRouter，调用 Vue.use(VueRouter)
2. 定义（路由）组件。（由一个或多个包含path和component属性的对象，组成的数组）
3. 创建 router 实例，然后传 `routes` 配置（将定义的路由组件传入）
4. 创建和挂载根实例。
