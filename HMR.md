### HMR 
`hot-module-replacement`热模块替换，当本地代码更新并保存后，**webpack**会对代码重新编译打包，并将新的模块发送到浏览器，浏览器会用新的模块替换旧的模块，实现页面无刷新的更新。

> 官网所说，HMR不适用于生产环境，只能在开发环境使用

- 优点：浏览器状态不会丢失，开发效率高

在开始HMR之前，还必须得了解`webpack-dev-server`

#### webpack-dev-server

使用`webpack-dev-server`后，会默认在localhost:8080下建立服务，将dist下的文件作为可访问资源,打开index.html文件，如果修改文件保存后，浏览器会自动刷新

- 安装

```
yarn add webpack-dev-server -D
```

- 配置

```
// webpack.config.js

module.exports = {
    devServer: {
        contentBase: './dist',
        host: 'localhost',
        port: 8080,
        open: true, // 自动打开页面
    },
}

// package.json

"scrpit": {
    "start": "webpack-dev-server --open"
}
```
- 使用yarn start运行


准备好`webpack-dev-server`后，开启**HMR**很简单了。我们只需在此基础上稍加更改。

```
// webpack.config.js

const webpack = require('webpack)

module.exports = {
    devServer: {
        hot: true
    },
    plugins: [
        new webpack.NamedChunksPlugin(),
        new webpack.HotModuleReplacementPlugin()
    ]
}

// package.json
"scrpit": {
    "start": "webpack-dev-server --hotOnly"
}

```

在我们的业务代码中

```
if(module.hot) {
    module.hot.accept('./printMe.js', function() {
        console.log('accept the apdate from printMe.js')
        document.body.removeChild(element)
        element = component()
        document.body.appendChild(element)
    })
}
```

这样我们对`printMe.js`作修改时，就会按我们的预期正常运行，而不会丢失状态。