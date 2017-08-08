# 慕课 饿了吗

## 使用vue-cli

node -- 检测node版本
npm install -g vue-cli -- 安装vue-cli
vue list -- 显示vue-cli支持的模板

通过vue init webpack sell 创建模板

eslint -- 代码风格检测器

安装完成以后 执行
 cd sell
 npm install
 npm run dev


目录介绍：
bulid 和 config -- webpack 相关

- src -- 开发代码都放在这个文件夹

- static -- 存放第三方静态资源的

- .babelrc -- babel 编译的配置

```
{
  "presets": [ // 预设
    ["es2015", { "modules": false }], // babel 安装需要预先安装的插件
    "stage-2"  // 2015 草案 -- 共有四个阶段，0-3，stage-2 就代表使用2-3
  ],
  "plugins": ["transform-runtime"], // 三方插件，扩展方法
  "comments": false,  // false 表示转换后的代码 取消注释
  "env": { //
    "test": {
      "plugins": [ "istanbul" ]
    }
  }
}
```

- editorconfig -- 编辑器风格

```
root = true

[*]
charset = utf-8
indent_style = space // 缩进风格
indent_size = 2  // 缩进大小
end_of_line = lf // 换行风格
insert_final_newline = true // 末尾添加新行
trim_trailing_whitespace = true // 会自动移出行尾的空格
```

- eslintignore

```
build/*.js
config/*.js
```

对这里的文件忽略 eslint 检查

- eslintrc -- eslint 配置文件

```
'rules': {
  // allow paren-less arrow functions
  'arrow-parens': 0,
  // allow async-await
  'generator-star-spacing': 0,
  // allow debugger during development
  'no-debugger': process.env.NODE_ENV === 'production' ? 2 : 0
}
```
在这里设置为0 就不做限制；
` process.env.NODE_ENV === 'production'`
判断当前环境是不是工作环境

- gitignore

```
.DS_Store
node_modules/
dist/
npm-debug.log
```
这些文件或者目录不会提交到git上

- package.json -- 项目配置文件

```
"scripts": {
  "dev": "node build/dev-server.js",
  "build": "node build/build.js",
  "lint": "eslint --ext .js,.vue src"
},
```

可以执行的命令 例如 npm run dev

- dependence -- 生产环境的依赖

- devDependece -- 编译环境的依赖

- index -- 入口文件

文件的入口js 是 src 中的main.js

- main.js

```
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue' //  接受到Vue
import App from './App' // App.vue 省略了后缀
import router from './router'

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  template: '<App/>',
  components: { App }
})
```

- App.vue

每一个组件都分为3个部分，
template ||  

```
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <hello></hello>
  </div>
</template>
```

这个template就是模板，其中还引入了 hello 模块

注意：在 如果需要在一个模块中使用 hello 模块，就必须
先通过`import Hello from './components/Hello'`引入然后通过 components 注册；

```
export default {
  name: 'app',
  components: {
    Hello
  }
}
```

`export default {} `就是将信息导出，


补充：
postcss -- 补充css 兼容，被vue-loader 依赖
