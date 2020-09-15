# Default starter for Gridsome

https://www.gridsome.cn/docs/

## 1. 前置准备

```shell
# 项目新建前置依赖
npm config set sharp_binary_host "https://npm.taobao.org/mirrors/sharp/"
npm config set sharp_libvips_binary_host "https://npm.taobao.org/mirrors/sharp-libvips/"
npm config set sharp_dist_base_url "https://npm.taobao.org/mirrors/sharp-libvips/"

# 项目新建前置依赖 
npm install -g node-gyp # -- node-gyp list   node-gyp install
# 然后必须以管理员身份运行powershell或者cmd否则以下指令会报错
npm install --global --production windows-build-tools

# 注意检查比较node版本要求
```



## 2. 全局安装依赖

```shell
npm install --global @gridsome/cli
```



## 3. 新建项目说明

```shell
gridsome create my-gridsome-site
# 依赖安装不成功，考虑方式: 1. cnpm install  2. 直接解压node_modules压缩文件
gridsome develop
```



## 4. introduction 

```javascript
// https://www.gridsome.cn/docs/
```

### 1. Collections

```js
// gridsome.server.js ---- https://gridsome.org/docs/collections/
const axios = require('axios')

module.exports = function (api) {
  api.loadSource(async actions => {
    const collection = actions.addCollection('Post')

    const { data } = await axios.get('https://cnodejs.org/api/v1/topics?limit=10')

    for (const item of data.data || []) {
      collection.addNode({
        id: item.id,
        title: item.title,
        content: item.content
      })
    }
  })
}

// https://gridsome.org/docs/querying-data/
```



### 2.Templates

```javascript
// https://gridsome.org/docs/templates/

// gridsome.config.js
module.exports = {
  templates: {
    Post: [
      {
         path: '/posts/:id',
         component: './src/templates/Post.vue'
      }
    ]
  }
}

// http://localhost:8080/posts/5ee1ee83b703280f0bcb922a
```



## 5. plugins

```shell
# https://gridsome.org/docs/data-store-api/
# https://www.gridsome.cn/plugins/

# ---- plugin - A:
npm install --save @gridsome/source-strapi # 预取数据，集成数据
# 实践成功方式:
cnpm install --save @gridsome/source-strapi

# ---- plugin - B: B-C组合使用
cnpm install --save @gridsome/source-filesystem # 将文件转换为 GraphQL 数据层可获取的内容
cnpm install --save markdown-it

# ---- plugin - C: B-C组合使用
cnpm install --save @gridsome/transformer-remark # Gridsome 用于转换 Markdown 的转换器
```



## 6.安装依赖

```shell
cnpm install --save axios
```



## 7.发布上线

### 1.区分环境自定义变量

```shell
# https://www.gridsome.cn/docs/environment-variables/

# .env.production
GRIDSOME_API_URL=https://api.example.com

# .env.development
GRIDSOME_API_URL=https://api.example.com

# all the variables are available on the server, only variables prefixed with GRIDSOME_ are available in the browser for security reasons; 即只有GRIDSOME_前缀的变量在客户端浏览器上课访问;
```



### 2.自定义环境变量用法

```javascript
// 1.客户端-直接使用, 在<script></script>中可直接使用, 但在<template></template>中不能直接使用
export default {
  data () {
    return {
      items: []
    }
  },
  async mounted () {
    const res = await axios.get(process.env.GRIDSOME_API_URL)
    this.items = res.data
  }
}

// 挂载到Vue上、或通过Vue.mixin-混入到Vue实例中 ---- main.js
export default function (Vue, { router, head, isClient }) {
  Vue.mixin({
      data () {
          return {
              GRIDSOME_API_URL: process.env.GRIDSOME_API_URL
          }
      }
  })
    
  // Set default layout as a global component
  Vue.component('Layout', DefaultLayout)
}
```



### 3.通过vercel发布上线

https://vercel.com/

#### 1.根据内容自动构建发布

1. 通过[git-integration](https://vercel.com/longxiaobai/static-gridsome-site/settings/git-integration)设置Deploy Hooks，复制生成网址；
2. 在strapi项目-设置-Webhooks，添加新的 webhook；

#### 2. 协议不匹配

https协议与http协议不匹配，发布上线需注意；