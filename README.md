# 目录

1. [版本说明](#banben)
2. [目录结构](#banben)
3. [初始化项目](#banben)
4. [webpack](#banben)
5. [react](#banben)
6. [配置loader](#banben)
7. [引入babel](#banben)
7. [使用HtmlWebpackPlugin](#banben)

# 版本说明
由于构建相关例如webpack，babel等更新的较快，所以本教程以下面各种模块的版本号为主，切勿轻易修改或更新版本。
```javascript
  "dependencies": {
    "axios": "^0.18.0",
    "babel-plugin-transform-async-to-generator": "^6.24.1",
    "babel-plugin-transform-runtime": "^6.23.0",
    "copy-webpack-plugin": "^4.5.1",
    "css-loader": "^0.28.11",
    "file-loader": "^1.1.11",
    "html-webpack-include-assets-plugin": "^1.0.4",
    "node-sass": "^4.7.2",
    "react": "^16.2.0",
    "react-dom": "^16.2.0",
    "react-redux": "^5.0.7",
    "react-router-dom": "^4.2.2",
    "react-router-redux": "^5.0.0-alpha.9",
    "redux": "^3.7.2",
    "redux-thunk": "^2.2.0",
    "sass-loader": "^6.0.7",
    "sass-resources-loader": "^1.3.3",
    "script-ext-html-webpack-plugin": "^2.0.1",
    "style-loader": "^0.20.3",
    "url-loader": "^1.0.1",
    "babel-core": "^6.26.0",
    "babel-eslint": "^8.2.3",
    "babel-loader": "^7.1.4",
    "babel-plugin-react-hot-loader": "^3.0.0-beta.6",
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-react": "^6.24.1",
    "babel-preset-stage-0": "^6.24.1",
    "clean-webpack-plugin": "^0.1.19",
    "eslint": "^4.19.1",
    "eslint-loader": "^2.0.0",
    "eslint-plugin-react": "^7.7.0",
    "extract-text-webpack-plugin": "^3.0.2",
    "html-webpack-inline-source-plugin": "0.0.10",
    "html-webpack-plugin": "^3.2.0",
    "react-hot-loader": "^4.0.0",
    "redux-logger": "^3.0.6",
    "webpack": "^4.3.0",
    "webpack-cli": "^2.0.12",
    "webpack-dev-server": "^3.1.1",
    "write-file-webpack-plugin": "^4.2.0"
  }
```
# 目录结构
开发和发布版本的配置文件是分开的，多入口页面的目录结构。
```
react-family/
    |
    |──dist/                                    * 发布版本构建输出路径
    |
    |──dev/                                     * 调试版本构建输出路径
    |
    |──src/                                     * 工具函数
    |     |
    |     |—— component/                        * 各页面公用组件
    |     |
    |     |—— page/                             * 页面代码
    |     |      |—— index/                     * 页面代码
    |     |      |        |—— Main/             * 组件代码
    |     |      |        |       |—— Main.jsx  * 组件jsx
    |     |      |        |       |—— Main.scss * 组件css
    |     |      |
    |     |      |—— detail/                    * 页面代码
    |     |
    |     |—— static/                           * 静态文件js，css
    |
    |
    |──webpack.config.build.js                  * 发布版本使用的webpack配置文件
    |──webpack.config.dev.js                    * 调试版本使用的webpack配置文件
    |──.eslint                                  * eslint配置文件
    |__.babelrc                                 * babel配置文件
```

# 初始化项目
1, 创建文件夹
```bash
mkdir react-family-bucket
```
2, 初始化npm
```bash
cd react-family-bucket
npm init
```
如果有特殊需要，可以填入自己的配置，一路回车下来，会生成一个`package.json`，里面是你项目的基本信息，后面的npm依赖安装也会配置在这里。
# webpack
1, 安装webpack
```bash
npm install webpack --save
or
npm install webpack --g
```
`--save`是将当前webpack安装到react-family-bucket下的`/node_modules`。
`--g`是将当前webpack安装到全局下面，可以在node的安装目录下找到全局的`/node_modules`。

2，配置webopack配置文件

```bash
touch webpack.config.dev.js
```
新建一个app.js
```bash
touch app.js
```
写入基本的webpack配置，可以参考[文档](https://webpack.js.org/)：
```javascript
const path = require('path');
const srcRoot = './src';
module.exports = {

    // 输入配置
    entry: [
      './app.js'
    ],,

    // 输出配置
    output: {
        path: path.resolve(__dirname, './dev'),

        filename: 'bundle.min.js'
    },

};
```
3, 执行webpack命令
如果是全局安装：
```bash
webpack --config webpack.config.dev.js
```
如果是当前目录安装：
```bash
./node_modules/.bin/webpack --config webpack.config.dev.js
```
执行命令之后，会发现需要安装`webpack-cli`，（webpack4之后需要安装这个）
```bash
npm install webpack-cli --save
```

去除`WARNING in configuration`警告,在webpack.config.dev.js增加一个配置即可：
```javascript
...
mode: 'development'
...
```
执行成功之后会在dev下面生成bundle.min.js正常。
# react
1. 安装react
```bash
npm install react react-dom --save
```
2. 创建page目录和index页面文件：
```bash
mkdir src
mkdir page
cd page
```
创建index
```bash
mkdir index
cd index & touch index.js & touch index.html
```
index.js
```javascript
import ReactDom from 'react-dom';
import Main from './Main/Main.jsx';

ReactDom.render(<Main />, document.getElementById('root'));
```
index.html
```html
<!DOCTYPE html>
<html>
<head>
    <title>index</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">

</head>
<body>
<div id="root"></div>
</body>
</html>
```
3. 创建Main组件
```javascript

import React from 'react';

class Main extends React.Component {

    constructor(props) {
        super(props);

    }

    render() {

        return (<div>Main</div>);
    }
}

export default Main;
```
* `export` 和 `export default`区别：

export可以有多个
```javascript
xx.js
export const test1 = 'a'
export function test2() {}
yy.js
import { test1, test2 } from 'xx.js';
```
export default只能有1个
```javascript
xx.js
let test1 = 'a';
export default test1;
yy.js
import test1 from 'xx.js';
```
* `export` 和 `module.exports`
```javascript
let exports = module.exports;
```
4. 修改webpack配置文件
# 配置loader

1. css-loader，sass-loader，style-loader处理样式文件需要这些loader
```bash
npm install css-loader sass-loader style-loader file-loader --save
```
配置：
```javascript
    module: {
        // 加载器配置
        rules: [
            { test: /\.css$/, use: ['style-loader', 'css-loader'], include: path.resolve(srcRoot)},
            { test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'], include: path.resolve(srcRoot)}
        ]
    },
```
2. url-loader处理处理静态文件
```bash
npm install url-loader --save
```
配置：
```
    module: {
        // 加载器配置
        rules: [
            { test: /\.(png|jpg|jpeg)$/, use: 'url-loader?limit=8192&name=images/[name].[hash].[ext]', include: path.resolve(srcRoot)}
        ]
    },
```
`limit:`表示超过多少就使用base64来代替，单位是byte<br>
`name:`可以设置图片的路径，名称和是否使用hash 具体[参考](https://github.com/webpack-contrib/url-loader)

# 引入babel

[bebel](https://babeljs.io/)是用来解析es6语法或者是es7语法分解析器，让开发者能够使用新的es语法，同时支持jsx，vue等多种框架。
1. 安装babel
```bash
npm install babel-core babel-loader --save
```
配置：
```
    module: {
        // 加载器配置
        rules: [
            { test: /\.(js|jsx)$/, use: [{loader:'babel-loader'}] ,include: path.resolve(srcRoot)},
        ]
    },
```
babel配置文件：`.babelrc`
```bash
touch .babelrc
```
配置：
```javascript
{
	"presets": [
		"es2015",
		"react",
		"stage-0"
	],
	"plugins": []
}
```
babel支持自定义的预设(presets)或插件(plugins),只有配置了这两个才能让babel生效，单独的安装babel是无意义的
`presets`：代表babel支持那种语法(就是你用那种语法写)，优先级是从下往上，`state-0|1|2|..`代表有很多没有列入标准的语法,[参考](https://babeljs.io/docs/en/babel-preset-stage-0.html)<br>
`plugins`:代表babel解析的时候使用哪些插件，作用和presets类似，优先级是从上往下
依次安装：
```bash
npm install babel-preset-es2015 babel-preset-react babel-preset-stage-0 --save
```
# 使用HtmlWebpackPlugin
记得我们之前新建的index.html么 我们执行构建命令之后并没有将index.html打包到dev目录下 我们需要[HtmlWebpackPlugin](https://github.com/jantimon/html-webpack-plugin)来将我们output的js和html结合起来

```bash
tnpm install html-webpack-plugin --save
```
配置：
```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
...
plugins: [
    new HtmlWebpackPlugin({
        filename: path.resolve(devPath, 'index.html'),
        template: path.resolve(srcRoot, './page/index/index.html'),
    })
]
```
`filename`:可以设置html输出的路径和文件名<br>
`template`:可以设置已哪个html文件为模版
更多参数配置可以[参考](https://github.com/jantimon/html-webpack-plugin)
## License

[GNU GPLv3](LICENSE)

