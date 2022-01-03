---
title: seo优化
date: 2020-09-03 14:02:11
updated:
tags: Vue
categories: seo
keywords: seo,vue
description: 预渲染 prerender-spa-plugin 结合 vue-meta-info
top_img:
cover:
comments:
toc:
toc_number:
auto_open:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
---
SPA项目爬虫无法爬取项目的静态文本内容
## seo优化 预渲染和服务端渲染
+ 服务端渲染

   将vue及对应库运行在服务端,web Server Frame实际上作为代理服务器去访问接口服务器来拉去数据,将拉去到的数据作为vue组件的初始状态.
   组件中的beforeCreate和created生命周期会在服务端调用,初始化对应的组件后,vue启用虚拟DOM形成初始化的HTML字符串.返回客户端,实现前后端同构应用.
   [服务端渲染](https://segmentfault.com/a/1190000018577041?utm_source=sf-related)
+ 预渲染

  对特定的路由构建生成静态HTML,在构建过程中本地启动一个 chrome的无头浏览器访问预渲染路由,再将渲染的html输出到dist目录建立与路由对应的目录
  
  不适用于个性化内容(依据每个人的页面)

  经常变化的内容(数据不是实时更新)

  预渲染的路由很多(严重拖慢构建进程)
## 预渲染
+ 安装 prerender-spa-plugin
  ```
    npm install prerender-spa-plugin --save
  ```
+ 配置

  搜索引擎对于#后面的内容不收录,要把hash模式改成history模式
  

  在router文件的index.js
  ```
  export default new Router({
      mode: 'history',
      base: '/',//如果项目直接放的根目录, 那么是没有问题的,如果是子目录,那么就会一片空白了.这个vue官方有解释,需要加一个base
    routes: []
  })
  ```
  在vue.config.js中
  ```
  const PrerenderSPAPlugin = require('prerender-spa-plugin')
  const Renderer = PrerenderSPAPlugin.PuppeteerRenderer
  ...
  ...
  configureWebpack: {
    // provide the app's title in webpack's name field, so that
    // it can be accessed in index.html to inject the correct title.
    name: name,
    resolve: {
      alias: {
        '@': resolve('src')
        // 'jQuery': 'jquery'
      }
    },
    plugins: [
      // new webpack.ProvidePlugin({
      //   $: 'jquery',
      //   jQuery: 'jquery',
      //   'windows.jQuery': 'jquery'
      // })
      **process.env.NODE_ENV !== 'development' ? new PrerenderSPAPlugin({
        // 生成文件的路径，也可以与webpakc打包的一致。
        // 这个目录只能有一级，如果目录层次大于一级，在生成的时候不会有任何错误提示，在预渲染的时候只会卡着不动。
        staticDir: resolve('dist'),
        // outputDir: path.join(__dirname, './'),
        // 对应自己的路由文件，比如a有参数，就需要写成 /a/param1。
        routes: routerPath,// ['/','/a','/b']
        // 这个很重要，如果没有配置这段，也不会进行预编译
        renderer: new Renderer({
          inject: { // 默认挂在window.__PRERENDER_INJECTED对象上，可以通过window.__PRERENDER_INJECTED.foo在预渲染页面取值
            foo: 'bar'
          },
          headless: false,
          // 在 main.js 中 document.dispatchEvent(new Event('render-event'))，两者的事件名称要对应上。
          renderAfterDocumentEvent: 'render-event' // 等到事件触发去渲染，此处我理解为是Puppeteer获取页面的时机
        })
      }) : () => {}** 
    ]
  },
  ```
  在main.js中
  ```
    new Vue({
    el: '#app',
    router,
    store,
    **// 添加到这里,这里的render-event和vue.config.js里面的
    renderAfterDocumentEvent配置名称一致
    mounted() {
      document.dispatchEvent(new Event('render-event'))
    }**,
    render: h => h(App)
  })
  ```
  打完包后,dist目录里就会多出来对应预渲染路由的静态页面
+ 配合 vue-meta-info 使用 改变每个预渲染路由的 meta

  安装
  ```
  npm install vue-meta-info --save
  ```
  main.js中
  ```
  import MetaInfo from 'vue-meta-info'

  Vue.use(MetaInfo)
  ```
  组件中静态用法
  ```
  export default {
    metaInfo: {
      title: '职级课程', // set a title
      meta: [
        { // set meta
          name: 'keywords',
          content: ''
        },
        { // set meta
          name: 'description',
          content: ''
        }
      ],
      link: [
        { // set link
          rel: 'assets',
          href: ''
        }
      ]
    },
    components: {},
    props: {},
    data() {
      return {
      }
    }
  }
  ```
  组件中动态用法
  ```
  export default {
    name: 'name',
    metaInfo () {
      return {
        title: this.pageName
      }
    },
    data () {
      return {
        pageName: 'loading'
      }
    },
    mounted () {
      setTimeout(() => {
        this.pageName = 'pageName'
      }, 2000)
    }
  }
  ```




