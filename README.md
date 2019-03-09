一、Usage
> ▶ git clone https://github.com/cosyer/es6-env.git
> ▶ cd es6-env
> ▶ yarn / npm install
> ▶ yarn dev / npm run dev

二、搭建过程
1、初始化文件目录

```
npm init -y
mkdir src
touch src/index.js
yarn add webpack webpack-cli -D
touch webpack.dev.config.js
```

```js
// webpack.dev.config.js文件
module.exports = {
  entry: "./src/index.js",
  output: {
    path: __dirname,
    filename: "./dist/bundle.js"
  }
}
```

package.json文件加一条script

```
"dev": "webpack --config ./webpack.dev.config.js --mode development"
```

2、自动打包插入js文件，热更新

```
yarn add webpack-dev-server html-webpack-plugin -D
touch index.html
```

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: "./src/index.js",
  output: {
    path: __dirname,
    filename: "./release/bundle.js"
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html'
    })
  ],
  devServer: {
    // 根目录
    contentBase: path.join(__dirname, './release'),
    // 自动打开浏览器
    open: true,
    port: 9124
  }
}
```

package.json文件改script

```
"dev": "webpack-dev-server --config ./webpack.dev.config.js --mode development"
```

3、babel编译ES6代码
此处因为babel升级到7的原因,所以babel-core6版本匹配,babel-loader@7要指定版本

```
yarn add -D babel-core babel-loader@7 babel-polyfill babel-preset-es2015 babel-preset-latest
touch .babelrc
```

```js
// .babelrc文件
{
  "presets": ["es2015", "latest"],
  "plugins": []
}
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
```

```js
module.exports = {
  entry: "./src/index.js",
  output: {
    path: __dirname,
    filename: "./dist/bundle.js"
  },
  plugins: [
    // bundle.js文件会被自动插入到模板文件中
    new HtmlWebpackPlugin({
      template: './index.html'
    })
  ],
  devServer: {
    // 根目录
    contentBase: path.join(__dirname, './dist'),
    // 自动打开浏览器
    open: true,
    port: 9124
  },
  module: {
    rules: [{
      test: /\.js?$/,
      exclude: /(node_modules)/,
      loader: 'babel-loader'
    }]
  }
}
```