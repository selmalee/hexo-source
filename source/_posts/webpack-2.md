---
title: 简单总结vue-cli中的webpack配置
date: 2017-07-09 11:38:01
tags: [Webpack, Vue, 自动化构建工具]
categories:
 - 学习笔记
---
## webpack简介
随着移动互联的浪潮到来，Webapp模式、SPA受到越来越多的关注。如何在开发环境中组织好碎片化、多格式的代码和资源，并保证他们在浏览器端快速、优雅地加载和更新需要一个模块化的系统。
[Webpack](http://webpack.github.io/)就是这样的模块化系统。它有以下几个特点：
 - 分块传输，按需进行懒加载；
 - 各种格式的资源都是JavaScript模块，可以用`require`加载；
 - 对代码进行静态分析，分析各模块的类型和依赖关系并交给适合的加载器处理。
 <!-- more -->

具体地说，Webpack的配置主要包括这几个方面：
 - 入口：指定我们代码的入口点。它的值是一个字符串或者一个对象（多个入口）。
 - 加载器：因为Webpack本身只能处理JavaScript模块，如果要处理其他类型的文件，就需要使用 loader(加载器)进行转换。加载器将资源转换成JavaScript模块，在打包之前提供预处理。
 - 插件：提供加载器无法做到的其他扩展功能。
 - 出口：包括打包后的文件放置的路径及它们的文件名称。

## vue-cli版本
2.8.1
（没有使用ESLint，没有安装单元测试，e2e测试）
## 相关的配置文件目录
``` bash
├── ...
├── build
│   ├── build.js
│   ├── check-versions.js
│   ├── dev-client.js
│   ├── dev-server.js
│   ├── utils.js
│   ├── vue-loader.conf.js
│   ├── webpack.base.conf.js
│   ├── webpack.dev.conf.js
│   └── webpack.prod.conf.js
├── config
│   ├── dev.env.js
│   ├── index.js
│   └── prod.env.js
├── ...
├── package.json
└── ...
```
## package.json
``` js
{
    // ...
    "scripts": {
        // 开发环境
        "dev": "node build/dev-server.js",
        // 生产环境
        "build": "node build/build.js"
    },
    // ...
    "devDependencies": {
        // ...
        // 配置分离，将配置定义在config/下，然后合并成最终的配置，以避免webpack.config.js臃肿
        "webpack-merge": "^2.6.1"
    },
    // ...
}
```
## 基础配置
### config/index.js
这个文件里是webpack的基础配置，描述了开发环境和生产环境的两个配置。
``` js
// see http://vuejs-templates.github.io/webpack for documentation.
var path = require('path')

module.exports = {
  // 生产环境
  build: {
    env: require('./prod.env'), // 编译环境
    index: path.resolve(__dirname, '../dist/index.html'), //  输入的文件
    assetsRoot: path.resolve(__dirname, '../dist'),  // 本地系统上的路径，输出的目标文件夹
    assetsSubDirectory: 'static', // 输出的二级文件夹(自动生成)
    assetsPublicPath: '/', // 发布资源的根目录，可配置为资源服务器url
    productionSourceMap: true, // 记录文件压缩前后的位置信息
    // 压缩 gzip模式下需要压缩的文件的扩展名
    // Gzip off by default as many popular static hosts such as
    // Surge or Netlify already gzip all static assets for you.
    // Before setting to `true`, make sure to:
    // npm install --save-dev compression-webpack-plugin
    productionGzip: false,
    productionGzipExtensions: ['js', 'css'],
    // Run the build command with an extra argument to
    // View the bundle analyzer report after build finishes:
    // `npm run build --report`
    // Set to `true` or `false` to always turn it on or off
    bundleAnalyzerReport: process.env.npm_config_report
  },
  // 开发环境
  dev: {
    env: require('./dev.env'),
    port: 8080, // 监听的端口
    autoOpenBrowser: true, // 自动打开浏览器
    assetsSubDirectory: 'static', // 输出的二级文件夹(自动生成)
    assetsPublicPath: '/', // 发布资源的根目录，可配置为资源服务器url
    // 如设置"/api": "http://localhost:3000"，则请求到 /api/users 现在会被代理到请求 http://localhost:3000/api/users
    // 需要proxyTable代理的接口(可跨域)
    proxyTable: {},
    // 记录文件压缩前后的位置信息
    // CSS Sourcemaps off by default because relative paths are "buggy"
    // with this option, according to the CSS-Loader README
    // (https://github.com/webpack/css-loader#sourcemaps)
    // In our experience, they generally work as expected,
    // just be aware of this issue when enabling this option.
    cssSourceMap: false
  }
}

```
### build/webpack.base.conf.js
这个文件是开发环境和生产环境都会用到的基础配置
dev-server.js里引入了webpack.dev.conf.js，build.js里引入了webpack.prod.conf.js，而这两个文件(webpack.dev.conf.js和webpack.prod.conf.js)里都引入了webpack.base.conf.js
``` js
var path = require('path')
var utils = require('./utils')
var config = require('../config')
var vueLoaderConfig = require('./vue-loader.conf')

function resolve (dir) {
  return path.join(__dirname, '..', dir)
}

module.exports = {
  // [[ 1. 配置webpack编译入口 ]]
  entry: {
    app: './src/main.js'
  },
  // [[ 2. 配置webpack输出路径和命名规则 ]]
  output: {
    path: config.build.assetsRoot,
    filename: '[name].js',
    publicPath: process.env.NODE_ENV === 'production'
      ? config.build.assetsPublicPath
      : config.dev.assetsPublicPath
  },
  // [[ 3. 配置模块resolve规则 ]]
  resolve: {
    extensions: ['.js', '.vue', '.json'],
    // 创建路径别名
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src')
    }
  },
  // [[ 4. 配置不同类型模块的处理规则 ]]
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test')]
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      }
    ]
  }
}

```
## 开发环境
### build/dev-server.js
执行`npm run dev`首先运行dev-server.js这个文件。
``` js
// [[ 1. 检查node和npm的版本 ]]
require('./check-versions')()

// [[ 2. 引入相关插件和配置 ]]
// 获取配置，获取配置中设置的环境变量
var config = require('../config')
if (!process.env.NODE_ENV) {
  process.env.NODE_ENV = JSON.parse(config.dev.env.NODE_ENV)
}

var opn = require('opn') // 用于打开默认浏览器打开localhost:端口
var path = require('path')
var express = require('express')
var webpack = require('webpack')
var proxyMiddleware = require('http-proxy-middleware') // 一个express中间件，用于将http请求代理到其他服务器
var webpackConfig = require('./webpack.dev.conf')

// 设置监听端口
var port = process.env.PORT || config.dev.port
// 设置是否自动打开浏览器
var autoOpenBrowser = !!config.dev.autoOpenBrowser
// 定义HTTP代理表
// https://github.com/chimurai/http-proxy-middleware
var proxyTable = config.dev.proxyTable

// [[ 3. 创建express服务器和webpack编译器 ]]
var app = express()
var compiler = webpack(webpackConfig)

// [[ 4. 配置开发中间件和热重载中间件 ]]
var devMiddleware = require('webpack-dev-middleware')(compiler, {
  publicPath: webpackConfig.output.publicPath,
  quiet: true
})

var hotMiddleware = require('webpack-hot-middleware')(compiler, {
  log: () => {}
})
// 当html-webpack-plugin模版文件发生改变，强制页面重载
compiler.plugin('compilation', function (compilation) {
  compilation.plugin('html-webpack-plugin-after-emit', function (data, cb) {
    hotMiddleware.publish({ action: 'reload' })
    cb()
  })
})

// [[ 5. 挂载代理服务和中间件 ]]
Object.keys(proxyTable).forEach(function (context) {
  var options = proxyTable[context]
  if (typeof options === 'string') {
    options = { target: options }
  }
  app.use(proxyMiddleware(options.filter || context, options))
})

// 重定向不存在的URL
app.use(require('connect-history-api-fallback')())

// 使用开发中间件，即把webpack编译后输出到内存中的文件资源挂到express服务器上
app.use(devMiddleware)

// 挂载热重载
app.use(hotMiddleware)

// [[ 6. 配置静态资源 ]]
var staticPath = path.posix.join(config.dev.assetsPublicPath, config.dev.assetsSubDirectory)
app.use(staticPath, express.static('./static'))

// [[ 7. 启动服务器监听特定端口]]
var uri = 'http://localhost:' + port

var _resolve
var readyPromise = new Promise(resolve => {
  _resolve = resolve
})

// [[ 8. 打开默认浏览器打开localhost:端口 ]]
console.log('> Starting dev server...')
devMiddleware.waitUntilValid(() => {
  console.log('> Listening at ' + uri + '\n')
  // when env is testing, don't need open it
  if (autoOpenBrowser && process.env.NODE_ENV !== 'testing') {
    opn(uri)
  }
  _resolve()
})

// 监听端口
var server = app.listen(port)

module.exports = {
  ready: readyPromise,
  close: () => {
    server.close()
  }
}

```
### 其他配置文件
 - build/webpack.dev.conf.js：在webpack.base.conf的基础上完善了开发环境中需要的配置，如合并配置文件、更友好地输出webpack的警告信息等
 - build/utils.js：配置静态资源路径，pei z样式的加载器cssLoaders和styleLoaders
 - build/vue.loader.conf.js：配置了css加载器和编译css之后自动添加前缀(autoprefixer)

## 生产环境
### build/build.js
执行`npm run build`首先运行build.js这个文件。
``` js
// [[ 1. 检查NodeJS和npm的版本 ]]
require('./check-versions')()

process.env.NODE_ENV = 'production'

var ora = require('ora') // 用于开启loading动画
var rm = require('rimraf')
var path = require('path')
var chalk = require('chalk')
var webpack = require('webpack')
var config = require('../config')
var webpackConfig = require('./webpack.prod.conf')

// [[ 2. 开启loading动画 ]]
var spinner = ora('building for production...')
spinner.start()

// [[ 3. 删除创建打包的目标文件夹]]
rm(path.join(config.build.assetsRoot, config.build.assetsSubDirectory), err => {
  if (err) throw err
  // [[ 4. webpack编译 ]]
  webpack(webpackConfig, function (err, stats) {
    spinner.stop()
    if (err) throw err
    // 输出相关信息
    process.stdout.write(stats.toString({
      colors: true,
      modules: false,
      children: false,
      chunks: false,
      chunkModules: false
    }) + '\n\n')

    console.log(chalk.cyan('  Build complete.\n'))
    console.log(chalk.yellow(
      '  Tip: built files are meant to be served over an HTTP server.\n' +
      '  Opening index.html over file:// won\'t work.\n'
    ))
  })
})
```
### 其他配置文件
 - build/webpack.prod.conf.js：在webpack.base.conf的基础上完善了生产环境中需要的配置，如合并基础的webpack配置、配置webpack的输出、丑化压缩代码、抽离css文件等

## 参考
 - [webpack中文指南](https://zhaoda.gitbooks.io/webpack/content/)
 - [vue-cli的webpack模板项目配置文件分析](https://github.com/hehongwei44/my-blog/issues/205)

## 题外话
评论系统换成了最近颇受关注的[Gitment](https://imsun.net/posts/gitment-introduction/) 目前看来很赞^ ^
