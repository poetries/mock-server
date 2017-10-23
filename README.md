mock-server
---

build a mock-server 


Usage
---

```javascript
# 安装依赖
npm install

# 先执行npm run gulp
npm run gulp

# 再执行npm run dev
# 就可以搭建一个随改随变的 mock-server 环境
# 以后每次当我们需要添加修改mock数据使都不需要重启mock服务
# 然后在浏览器中打开http://localhost:8081/api/comment/get.action自定义的接口
npm run dev
```

原理
---

- 在项目根目录新建 `mock`文件夹，新建 `mock/db.js` 作为 `mock` 数据源，`mock/server.js`作为 `mock` 服务，`mock/routes.js`重写路由表
- 利用 `mock.js` 生成 `mock` 数据，可以尽可能的还原真实数据

**搭建主要思路**

- 以 `json-server` 作为 `mock` 服务器， `mock.js` 生成`mock` 数据，利用 `gulp + nodemon + browser-sync` 监听`mock`文件的改动重启 `node` 服务，刷新浏览器，以此达到一种相对完美的 `mock-server`要求

**端口代理**

> 通过 `Webpack` 配置 `proxy` 代理

```javascrpit
module.exports = {
  
  devServer: {  
    //其实很简单，只要配置这个参数就可以了  
    proxy: {  
      '/api/': {  
        target: 'http://localhost:3000',
  	    changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    }
  } 
}
```

- 接着在代码里进行 `ajax`请求就可以写成，以 `axios `为例子

```javascript
function getComments () {
  axios.get('api/comment/get.action', {}).then((res) => {
    console.log(res.data)
  })
}
```
