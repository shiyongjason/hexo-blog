---
title: webpack
date: 2023-04-25 13:45:13
tags: webpack
---

## 1： 常见配置
 1、文件打包的出口和入口
 2、webpack css 配置
 3、webpack如何打包图片，静态资源等
 4、转化js
 5、性能
 
 
 
 ## 安装
 
```
npm i webpack webpack-cli -D
(开发 生产不要下载)
```

 
![image](/img/webpack1.png)
 
-  入口文件 src/
-  出口 dist/

## package.json

- scripts

```
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "watch": "webpack --watch",
    "dev": "webpack-dev-server"
```

## webpack 配置 入 出口


```
module.exports = {
  entry : './src/index.js'  // 配置打包入口文件
  output : { // 配置打包完成的出口文件 路径 
    path : path.resolve(__dirname , './dist/'),
    filename : 'building.js'
  }
}
```

优化输出的文件名称，为了缓存 hash，自动引入到html 里面

```
npm i -D html-webpack-plugin
```

### 多入口文件

> 多入口文件如何开发


```
plugins:[
      new HtmlWebpackPlugin({
        template:path.resolve(__dirname,'../public/index.html'),
        filename:'index.html',
        chunks:['main'] // 与入口文件对应的模块名
      }),
      new HtmlWebpackPlugin({
        template:path.resolve(__dirname,'../public/header.html'),
        filename:'header.html',
        chunks:['header'] // 与入口文件对应的模块名
      }),
    ]

```

如图
![image](/img/webpack2.png)


> 每次执行npm run build 会发现dist文件夹里会残留上次打包的文件，这里我们推荐一个plugin来帮我们在打包输出前清空文件夹clean-webpack-plugin, 

```
const {CleanWebpackPlugin} = require('clean-webpack-plugin')

plugins:[
   new CleanWebpackPlugin(),
]

```

### css 解析

> 要一些loader来解析我们的css文件

```
npm i -D style-loader css-loader
或者
npm i -D less less-loader

```
![image](/img/webpack22.png)
![image](/img/webpack222.png)

> 实现css3代码自动补全，也可以运用到sass、less

1、 display: flex;
``` css
display: -webkit-box;
display: -ms-flexbox;
display: flex;
```

> 这时候我们发现css通过style标签的方式添加到了html文件中，但是如果样式文件很多，全部添加到html中，难免显得混乱。这时候我们想用把css拆分出来用外链的形式引入css文件怎么做呢？

```
npm i -D mini-css-extract-plugin
```

```
rules:[
      {
          test:/\.css$/,
          use: ['vue-style-loader','css-loader',{
            loader:'postcss-loader',
            options:{
              plugins:[require('autoprefixer')]
            }
          }]
        },
        {
          test:/\.less$/,
          use: ['vue-style-loader','css-loader',{
            loader:'postcss-loader',
            options:{
              plugins:[require('autoprefixer')]
            }
          },'less-loader']
        }

      ]
```



> 拆分多个css 【mini-css-extract-plugin会将所有的css样式合并为一个css文件】

```
<!--npm i -D extract-text-webpack-plugin@next-->
```

如图
![image](/img/webpack3.png)

###  打包 图片、字体、媒体、等文件

1、file-loader
2、url-loader

如图
![image](/img/webpack4.png)

### babel 转义 js

> 为了使我们的js代码兼容更多的环境我们需要安装依赖

```
npm i -D babel-loader @babel/preset-env @babel/core
```

```
// webpack.config.js
module.exports = {
    // 省略其它配置 ...
    module:{
        rules:[
          {
            test:/\.js$/,
            use:{
              loader:'babel-loader',
              options:{
                presets:['@babel/preset-env']
              }
            },
            exclude:/node_modules/
          },
       ]
    }
}
```
如图
![image](/img/webpack5.png)

es6/7/8 语法转成es5，但是对新api并不会转换 例如(promise、Generator、Set、Maps、Proxy等)

```
npm i @babel/polyfill
```

## 性能
### 缩小文件的搜索范围(配置include exclude alias noParse extensions)

### 使用HappyPack开启多进程Loader转换

```
npm i -D happypack
```

### 使用webpack-parallel-uglify-plugin 增强代码压缩

```
npm i -D webpack-parallel-uglify-plugin
```

### 抽离第三方模块

### 配置缓存

我们每次执行构建都会把所有的文件都重复编译一遍，这样的重复工作是否可以被缓存下来呢，答案是可以的，目前大部分 loader 都提供了cache 配置项。比如在 babel-loader 中，可以通过设置cacheDirectory 来开启缓存，babel-loader?cacheDirectory=true 就会将每次的编译结果写进硬盘文件（默认是在项目根目录下的node_modules/.cache/babel-loader目录内，当然你也可以自定义）

### 引入webpack-bundle-analyzer分析打包后的文件