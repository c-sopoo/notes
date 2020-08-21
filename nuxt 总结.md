## nuxt 总结

[官网]: https://zh.nuxtjs.org/guide

Nuxt.js 是一个基于 Vue.js 的通用应用框架。

通过对客户端/服务端基础架构的抽象组织，Nuxt.js 主要关注的是应用的 **UI 渲染**。

我们的目标是创建一个灵活的应用框架，你可以基于它初始化新项目的基础结构代码，或者在已有 Node.js 项目中使用 Nuxt.js。

Nuxt.js 预设了利用 Vue.js 开发**服务端渲染**的应用所需要的各种配置。

### 1. 安装

```bash
$ npx create-nuxt-app <项目名>   确保安装了npx
```

或者

```bash
$ yarn create nuxt-app <项目名>
```

2. ### 配置

   ```js
   // 全局axios
   yarn add @nuxtjs/axios -S
   // nuxt.config.js
   export default {
       modules: [
           '@nuxtjs/axios'
       ]
   }
   
   // 在plugins文件夹下，新建request.js
   // 注入context中
   // 内置axios
   // http://www.axios-js.com/zh-cn/docs/nuxtjs-axios.html
   export default function({$axios}) {// 
       // 基础路径
       $axios.default.baseURL = 'XXX'
       // 响应拦截
       $axios.onResponse(response => {})
       // 请求拦截
       $axios.onRequest(config => {})
       // 错误拦截
       $axios.onError(err => {})
   }
   // nuxt.config.js
   export default {
       pulgins: [
           {src: '@/plugins/request'}
       ]
   }
   
   // 将方法或变量全局注入
   //plugins文件夹下新建my-inject.js
   export default (({app}, inject) => {
       inject('myInject', () => {console.log('全局注入')})
   })
   // nuxt.config.js
   export default {
       plugins:['@/plugins/my-inject.js']
   }
   // 在context中使用
   asyncData(context) {
       context.app.$myInject()
   }
   // vue实例中使用,通过this调用
   created() {
       this.$myInject()
   }
   
   // 代理
   yarn add @nuxtjs/proxy -D
   // nuxt.config.js 注册模块
   export default {
       modules: [
           '@nuxt/proxy'
       ],
       axios: {
           proxy: true
       },
       proxy: {
           '/api/': {
             target: 'http://192.168.31.187',
             pathRewrite: {
               '^/api/': '/',
               changeOrigin: true
             }
           }
       }
   }
   
   
   // 全局css
   // nuxt.config.js
   export default {
       css: [
           '@/路径'
       ]
   }
   
   // 自定义路由
   // nuxt.config.js
   export default {
       router: {
           extendRoutes (routes, resolve) {
               routes.push({
                   name: 'xx',
                   path: '你要跳转的路径',
                   component: resolve(__dirname, 'pages/xx')
               })
           }
       }
   }
   
   // 加载进度条
   // nuxt.config.js
   export default {
       loading: {// 更多配置查看官方文档
           color: 'purple'
       }
   }
   
   //cookie 插件
   yarn add cookie-universal-nuxt -S
   // nuxt.config.js
   export default {
       modules: [
           'cookie-universal-nuxt'
       ]
   }
   // 使用 vue实例中通过this.$cookies
   // context 中通过context.app.$cookies
   // 更多方法访问 https://www.npmjs.com/package/cookie-universal-nuxt
   
   // 环境配置
   yarn add cross-env -D
   // package.json
   "scripts": {
       "dev": "cross-env BASE_URL=http://192.168.31.187/ NODE_ENV=development nuxt",
       "build": "cross-env BASE_URL=https://xx线上域名 NODE_ENV=production nuxt build",
       "start": "cross-env BASE_URL=https:// NODE_ENV=production nuxt start",
       "generate": "cross-env BASE_URL=https:// NODE_ENV=production nuxt generate"
     },
     // nuxt.config.js
     export default {
        env: {
           BASE_URL: process.env.BASE_URL,
       	NODE_ENV: process.env.NODE_ENV
        }
     }
   
   // 自定义端口号
   package.json 中添加
   "config": {
    "nuxt": {
         "host": "0.0.0.0",
         "port": "9999" // 自定义修改
       }
     }
   
   
   // 线上环境去console
   yarn add babel-plugin-transform-remove-console -D
   // nuxt.config.js
   
   const myPlugins = []
   if (process.env.NODE_ENV === 'production') {
     // 添加transform-remove-console插件，以实现删除项目中的所有的 console 代码
     // 可配置选项  ["transform-remove-console", { "exclude": [ "error", "warn"] }] 不包括error 和 warn
     // 或者直接 'transform-remove-console'
     myPlugins.push(["transform-remove-console", { "exclude": [ "error", "warn"] }])
   }
   export default {
       build: {
           babel: {
               'pulgins': myPlugins
           },
           // 配置打包后路径,根据情况写,默认/_nuxt/
           publicPath: ''
       }
   }
   
   
   ```
   
   