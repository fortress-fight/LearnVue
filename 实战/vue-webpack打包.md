webpack 是将各种文件进行打包

开发时的webpack

- package.json

```
"scripts": {
  "dev": "node build/dev-server.js",
  "build": "node build/build.js",
  "lint": "eslint --ext .js,.vue src"
},
```

 首先看 webpack run dev

 ```
 var opn = require('opn')
 var path = require('path') // node 路径
 var express = require('express') // node 框架
 var webpack = require('webpack') // 引入 webpack
 var proxyMiddleware = require('http-proxy-middleware') // http 代理
 var webpackConfig = require('./webpack.dev.conf') // webpack 相关配置
 ```

 - webpack dev confi.js

```
var utils = require('./utils') // 合并工具方法
var webpack = require('webpack')
var config = require('../config') // 配置
var merge = require('webpack-merge') // 用于合并配置文件
var baseWebpackConfig = require('./webpack.base.conf') // wepack 共享配置
var HtmlWebpackPlugin = require('html-webpack-plugin') // webpack 操作html 插件
var FriendlyErrorsPlugin = require('friendly-errors-webpack-plugin')
```


```
resolve: {
  extensions: ['.js', '.vue', '.json'], // 用于自动补全
  modules: [
    resolve('src'),
    resolve('node_modules')
  ],
  alias: { // 路径别名
    'vue$': 'vue/dist/vue.common.js',
    'src': resolve('src'),
    'assets': resolve('src/assets'),
    'components': resolve('src/components')
  }
```
