# 目录

1. [版本说明](#banben)
2. [目录结构](#banben)
3. [初始化项目](#banben)
4. [webpack](#banben)
5. [react](#banben)
6. [配置loader(sass,jsx)](#banben)
7. [引入babel](#banben)
8. [使用HtmlWebpackPlugin](#banben)
9. [redux](#banben)
10. [使用webpack-dev-server](#banben)
11. [多入口页面配置](#banben)
12. [如何理解`entry point(bundle)`,`chunk`,`module`](#banben)
13. [多入口页面html配置](#banben)
14. [模块热替换（Hot Module Replacement）](#banben)
15. [使用ESLint](#banben)
16. [使用react-router](#banben)
16. [使用redux-thunk](#banben)
17. [使用axios和async/await](#banben)


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
1, 安装[webpack](https://webpack.js.org/)
```bash
npm install webpack --save
or
npm install webpack --g
```
`--save`是将当前webpack安装到react-family-bucket下的`/node_modules`。<br>
`--g`是将当前webpack安装到全局下面，可以在node的安装目录下找到全局的`/node_modules`。

2，配置webopack配置文件

```bash
touch webpack.config.dev.js
```
新建一个app.js
```bash
touch app.js
```
写入基本的webpack配置，可以[参考这里](https://webpack.js.org/)：
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
在package.json中添加执行命令：
```json
  "scripts": {
    "dev": "./node_modules/.bin/webpack --config webpack.config.dev.js",
  },
```
执行`npm run dev`命令之后，会发现需要安装`webpack-cli`，（webpack4之后需要安装这个）
```bash
npm install webpack-cli --save
```

去除`WARNING in configuration`警告,在webpack.config.dev.js增加一个配置即可：
```javascript
...
mode: 'development'
...
```
成功之后会在dev下面生成bundle.min.js代表正常。<br>
如果想要动态监听文件变化需要在命令后面添加  `--watch`
# react
1. 安装[react](https://reactjs.org/)
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
xx.js:
export const test1 = 'a'
export function test2() {}

yy.js:
import { test1, test2 } from 'xx.js';
```
export default只能有1个
```javascript
xx.js:
let test1 = 'a';
export default test1;

yy.js:
import test1 from 'xx.js';
```
* `export` 和 `module.exports`
```javascript
let exports = module.exports;
```
4. 修改webpack配置入口文件
```javascript
entry: [
    path.resolve(srcRoot,'./page/index/index.js')
],
```
# 配置loader

1. 处理样式文件需要这些loader:
* [css-loader](https://github.com/webpack-contrib/css-loader)
* [sass-loader](https://github.com/webpack-contrib/sass-loader)
* [style-loader](https://github.com/webpack-contrib/style-loader)
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
2. [url-loader](https://github.com/webpack-contrib/url-loader)处理处理静态文件
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
`name:`可以设置图片的路径，名称和是否使用hash 具体[参考这里](https://github.com/webpack-contrib/url-loader)

# 引入babel

[bebel](https://babeljs.io/)是用来解析es6语法或者是es7语法分解析器，让开发者能够使用新的es语法，同时支持jsx，vue等多种框架。
1. 安装babel
* [babel-core](https://www.npmjs.com/package/babel-core)
* [babel-loader](https://www.npmjs.com/package/babel-loader)
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
babel支持自定义的预设(presets)或插件(plugins),只有配置了这两个才能让babel生效，单独的安装babel是无意义的<br>
<br>
`presets`：代表babel支持那种语法(就是你用那种语法写)，优先级是从下往上,`state-0|1|2|..`代表有很多没有列入标准的语法回已state-x表示,[参考这里](https://babeljs.io/docs/en/babel-preset-stage-0.html)<br>
`plugins`:代表babel解析的时候使用哪些插件，作用和presets类似，优先级是从上往下。
依次安装：
* [babel-preset-es2015](https://www.npmjs.com/package/babel-preset-es2015)
* [babel-preset-react](https://www.npmjs.com/package/babel-preset-react)
* [babel-preset-stage-0](https://www.npmjs.com/package/babel-preset-stage-0)
```bash
npm install babel-preset-es2015 babel-preset-react babel-preset-stage-0 --save
```
# 使用HtmlWebpackPlugin
记得我们之前新建的index.html么 我们执行构建命令之后并没有将index.html打包到dev目录下 我们需要[HtmlWebpackPlugin](https://github.com/jantimon/html-webpack-plugin)来将我们output的js和html结合起来

```bash
npm install html-webpack-plugin --save
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
更多参数配置可以[参考这里](https://github.com/jantimon/html-webpack-plugin)

# redux

关于[redux](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)的使用可以参考阮一峰老师的入门[教程](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)

1. 安装redux
* [redux](https://www.npmjs.com/package/redux)
* [react-redux](https://www.npmjs.com/package/react-redux)

```bash
npm install redux react-redux --save
```

2. 新建`reducers`，`actions`目录和文件
```
|—— index/                          
|—— Main/                   * 组件代码
|       |—— Main.jsx        * 组件jsx
|       |—— Main.scss       * 组件css
|
|—— actions/ 
|       |—— actionTypes.js  * action常量
|       |—— todoAction.js   * action
|
|—— reducers/ 
|       |—— todoReducer.js  * reducer
|
|—— store.js
|
|—— index.js

```

3. 修改代码，引入redux,这里以一个redux todo为demo例子：<br>

`index.js`
```javascript
import ReactDom from 'react-dom';
import React from 'react';
import Main from './Main/Main.jsx';
import store from './store.js';
import { Provider } from 'react-redux';

ReactDom.render(
	<Provider store={store}>
		<Main />
	</Provider>
, document.getElementById('root'));
```
`store.js`
```javascript
import { createStore } from 'redux';
import todoReducer from './reducers/todoReducer.js';

const store = createStore(todoReducer);

export default store;
```
`tabReducer.js`
```javascript
import { ADD_TODO } from '../actions/actionTypes.js';

const initialState = {
      todoList: []
};

const addTodo = (state, action) => {

  return { ...state, todoList: state.todoList.concat(action.obj) }
}

const todoReducer = (state = initialState, action) => {
  switch(action.type) {
    case ADD_TODO: return addTodo(state, action);
    default: return state;
  }
};
export default todoReducer;

```
`Main.jsx`
```javascript
import React from 'react';
import { connect } from 'react-redux';
import { addTodo } from '../actions/todoAction.js';

class Main extends React.Component {

    onClick(){
    	let text = this.refs.input;

    	this.props.dispatch(addTodo({
    		text: text.value
    	}))
    }
    render() {
        return (
        	<div>
        		<input ref="input" type="text"></input>
        		<button onClick={()=>this.onClick()}>提交</button>
        		<ul>
        		{this.props.todoList.map((item, index)=>{
        			return <li key={index}>{item.text}</li>
        		})}
        		</ul>
        	</div>
        );
    }
}

export default connect(
    state => ({
        todoList: state.todoList
    })
)(Main);
```
`todoAction.js`
```javascript
import { ADD_TODO } from './actionTypes.js';

export const addTodo = (obj) => {
  return {
    type: ADD_TODO,
    obj: obj
  };
};
```

# 使用webpack-dev-server
[webpack-dev-server](https://github.com/jantimon/webpack-dev-server)是一个小型的`Node.js Express`服务器,它使用webpack-dev-middleware来服务于webpack的包。
1. 安装
```bash
npm install webpack-dev-server --save
```
修改在package.json中添加的执行命令：
```json
  "scripts": {
    "dev": "./node_modules/.bin/webpack-dev-server --config webpack.config.dev.js",
  },
```
2. 配置webpack配置文件：
```json
devServer: {
    "contentBase": devPath,
    "compress": true,
},
```
`contentBase` 表示server文件的根目录
`compress` 表示开启gzip
更多的配置文档[参考这里](https://webpack.docschina.org/configuration/dev-server/)

* `webpack-dev-server`默认情况下会将output的内容放在内存中，是看不到物理的文件的，如果想要看到物理的dev下面的文件可以安装[write-file-webpack-plugin](https://www.npmjs.com/package/webpack-dev-server)这个插件。

* `webpack-dev-server`默认会开启livereload功能

3. `devtool`功能：
具体来说添加了`devtool: 'inline-source-map'`之后，利用source-map你在chrome控制台看到的source源码都是真正的源码，未压缩，未编译前的代码，没有添加，你看到的代码是真实的压缩过，编译过的代码，更多devtool的配置可以[参考这里](https://webpack.docschina.org/configuration/devtool/)

# 多入口文件配置
在之前的配置中，都是基于单入口页面配置的，entry和output只有一个文件，但是实际项目很多情况下是多页面的，在配置多页面时，有2中方法可以选择：

1. 在entry入口配置时，传入对象而不是单独数组,output时利用`[name]`关键字来区分输出文件例如：
```javascript
entry: {
    index: [path.resolve(srcRoot,'./page/index/index1.js'),path.resolve(srcRoot,'./page/index/index2.js')],
    detail: path.resolve(srcRoot,'./page/detail/detail.js'),
    home: path.resolve(srcRoot,'./page/home/home.js'),
},
output: {
    path: path.resolve(__dirname, './dev'),

    filename: '[name].min.js'
},
```
2. 通过node动态遍历需要entry point的目录，来动态生成entry：
```javascript
const pageDir = path.resolve(srcRoot, 'page');
function getEntry() {
	let entryMap = {};

	fs.readdirSync(pageDir).forEach((pathname)=>{
		let fullPathName = path.resolve(pageDir, pathname);
		let stat = fs.statSync(fullPathName);
		let fileName = path.resolve(fullPathName, 'index.js');

		if (stat.isDirectory() && fs.existsSync(fileName)) {
			entryMap[pathname] = fileName;
		}

	});

	return entryMap;
}
{
	...
	entry: getEntry()
	...
}
```

本demo采用的是第二中写法，能够更加灵活。

# 如何理解`entry point(bundle)`,`chunk`,`module`

在webpack中，如何理解`entry point(bundle)`,`chunk`,`module`?
![](https://oc5n93kni.qnssl.com/image/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-06-15%20%E4%B8%8B%E5%8D%889.06.45.png)

根据图上的表述，我这里简单说一下便于理解的结论：<br>
* 配置中每个文件例如index1.js,index2.js,detail.js,home.js都属于`entry point`.
* entry这个配置中，每个key值,index,detail,home都相当于`chunk`。
* 我们在代码中的require或者import的都属于`module`，这点很好理解。
* `chunk`的分类比较特别，有`entry chunk`,`initial chunk`,`normal chunk`,参考[这个文章](https://github.com/webpack/webpack.js.org/issues/970)
* 正常情况下，一个`chunk`对应一个output,在使用了`CommonsChunkPlugin`或者`require.ensure`之后，`chunk`就变成了`initial chunk`,`normal chunk`，这时，一个`chunk`对应多个output。<br>
理解这些概念对于后续使用webpack插件有很大的帮助。

# 多入口页面html配置

之前我们配置`HtmlWebpackPlugin`时，同样采用的是但页面的配置，这里我们将进行多页面改造,`entryMap`是上一步得到的entry：
```javascript
function htmlAarray(entryMap) {
	let htmlAarray = [];

	Object.keys(entryMap).forEach(function(key){
		let fullPathName = path.resolve(pageDir, key);
		let fileName = path.resolve(fullPathName, key + '.html')
		if (fs.existsSync(fileName)) {
			htmlAarray.push(new HtmlWebpackPlugin({
	            chunks: key, // 注意这里的key就是chunk
	            filename: key + '.html',
				template: fileName,
				inlineSource:  '.(js|css)'
	        }))
		}
	});

	return htmlAarray;

}
```
修改plugin配置：
```javascript
plugins: [
     ...
].concat(htmlMap)
```
# 模块热替换（Hot Module Replacement）

[模块热替换](https://webpack.docschina.org/guides/hot-module-replacement)(Hot Module Replacement 或 HMR)是 webpack 提供的最有用的功能之一。它允许在运行时更新各种模块，而无需进行完全刷新,很高大上有木有！

下面说一下配置方法，它需要结合`devServer`使用：
```javascript
devServer: {
	hot: true // 开启HMR
},
```
开启plugin：
```javascript
const webpack = require('webpack');
plugins: [
	new webpack.NamedModulesPlugin(),
	new webpack.HotModuleReplacementPlugin(),
].concat(htmlMap)
```

结合React一起使用：<br>

1. 安装[react-hot-loader](https://github.com/jantimon/react-hot-loader),
```bash
npm install react-hot-loader --save
```
并新建一个Container.jsx:<br>
```javascript
import React from 'react';
import Main from './Main.jsx';
import { hot } from 'react-hot-loader'

class Container extends React.Component {

    render() {
        return <Main />
    }
        
}
export default hot(module)(Container);
```

结合redux：如果项目没有使用redux，可以无需配置后面2步<br>
2. 修改store.js新增下面代码，为了让reducer也能实时热替换
```javascript
if (module.hot) {
    module.hot.accept('./reducers/todoReducer.js', () => {
      const nextRootReducer = require('./reducers/todoReducer.js').default;
      store.replaceReducer(nextRootReducer);
    });
}
```
3. 修改index.js
```javascript
import ReactDom from 'react-dom';
import React from 'react';
import Container from './Main/Container.jsx';
import store from './store.js';

import { Provider } from 'react-redux';

ReactDom.render(
	<Provider store={store}>
		<Container />
	</Provider>
, document.getElementById('root'));
```

当控制台看到`[WDS] Hot Module Replacement enabled.`代表开启成功

# 使用ESLint
[ESLint](https://eslint.org/) 是众多 Javascript Linter 中的其中一种，其他比较常见的还有 [JSLint](https://www.jslint.com/) 跟 [JSHint](http://jshint.com/)，之所以用 ESLint 是因为他可以自由选择要使用哪些规则，也有很多现成的 plugin 可以使用，另外他对 ES6 还有 JSX 的支持程度跟其他 linter 相比之下也是最高的。

1. 安装ESLint
```bash
npm install eslint eslint-loader babel-eslint --save
```
其中`eslint-loader`是将webpack和eslint结合起来在webpack的配置文件中新增一个eslint-loader种，修改如下
```javascript
{ test: /\.(js|jsx)$/, use: [{loader:'babel-loader'},{loader:'eslint-loader'}] ,include: path.resolve(srcRoot)},
```

2. 新建`.eslintrc`配置文件,将parser配置成`babel-eslint`
```json
{
    "extends": ["eslint:recommended"],
    
    "parser": "babel-eslint",

    "globals": {
    },
    "rules": {
    }
}
```

3. 安装[eslint-plugin-react](https://github.com/jantimon/eslint-plugin-react):
```bash
npm install eslint-plugin-react --save
```
* 说明一下，正常情况下每个eslint规则都是需要在`rule`下面配置，如果什么都不配置，其实本身eslint是不生效的。
* eslint本身有很多默认的规则模版，可以通过`extends`来配置，默认可以使用`eslint:recommended`。
* 在使用react开发时可以安装`eslint-plugin-react`来告知使用react专用的规则来lint。
3. 修改`.eslintrc`配置文件,增加rules，更多rules配置可以[参考这里](https://eslint.org/docs/rules/)
```javascript
{
    "extends": ["eslint:recommended","plugin:react/recommended"],
    
    "parser": "babel-eslint",

    "globals": {
        "window": true,
        "document": true,
        "module": true,
        "require": true
    },
    "rules": {
        "react/prop-types" : "off",
        "no-console" : "off"
    }
}
```

# 使用react-router
react-router强大指出在于方便代码管理，结合redux使用更加强大，同时支持web，native更多[参考这里](https://reacttraining.com/react-router/)
1. 安装[react-router-dom](https://github.com/jantimon/react-router-dom)
```bash
npm install react-router-dom --save
```
2. 如果项目中用了redux，可以安装[react-router-redux](https://github.com/ReactTraining/react-router/tree/master/packages/react-router-redux)
```bash
npm install react-router-redux@next history --save
```

3. 修改代码：
`index.js`:
```javascript
import ReactDom from 'react-dom';
import React from 'react';
import Container from './Main/Container.jsx';
import { store, history } from './store.js';

import { Provider } from 'react-redux';

import createHistory from 'history/createHashHistory';
import { ConnectedRouter } from 'react-router-redux';

const history = createHistory();

ReactDom.render(
	<Provider store={store}>
		<ConnectedRouter history={history}>
			<Container />
		</ConnectedRouter>
	</Provider>
, document.getElementById('root'));
```

结合`history`,react-router一共有3中不同的router：
* [BrowserRouter](https://reacttraining.com/react-router/web/api/BrowserRouter)通过`history/createBrowserHistory`引入:当切换时，url会动态更新，底层使用的时html5的[pushState](https://blog.csdn.net/tianyitianyi1/article/details/7426606)。
* [HashRouter](https://reacttraining.com/react-router/web/api/HashRouter)通过`history/createHashHistory`引入:当切换时，动态修改hash，利用hashchange事件。
* [MemoryRouter](https://reacttraining.com/react-router/web/api/MemoryRouter)通过`history/createMemoryHistory`引入:将路径，路由相关数据存入内存中，不涉及url相关更新，兼容性好。

更多配置可以[参考这里](https://reacttraining.com/react-router/)

4. 如果想要在代码逻辑中获取当前的route路径需要引入`router-reducer`:
新建`main.js`:
```javascript
import { combineReducers } from 'redux';
import { routerReducer } from "react-router-redux";
import todoReducer from './todoReducer.js';

const reducers = combineReducers({
  todoReducer,
  router: routerReducer
});
export default reducers;
```

修改`store.js`:
```javascript
import { createStore } from 'redux';
import mainReducer from './reducers/main.js';

const store = createStore(mainReducer);

export default store;
```
然后就可以在`this.props.router`里面获取单相关的路径信息
4. 如果需要自己通过action来触发router的跳转，需要引入`routerMiddleware`:
```
import { createStore,applyMiddleware } from 'redux';
import { routerMiddleware } from "react-router-redux";
const middleware = routerMiddleware(history);
const store = createStore(mainReducer,applyMiddleware(middleware));
```

5. 使用`Route`和`Link`和`withRouter`:
先说说都是干嘛的：<br>
* [Route](https://reacttraining.com/react-router/web/api/Route):component里面的内容即是tab的主要内容，这个从react-router4开始生效：
```javascript
<Route exact path="/" component={Div1}></Route>
<Route path="/2" component={Div2}></Route>
```
* [Link](https://reacttraining.com/react-router/web/api/Link):通常也可以用[NavLink](https://reacttraining.com/react-router/web/api/NavLink)，相当于tab按钮，控制router的切换,`activeClass`表示当前tab处于激活态时应用上的class。
* [withRouter](https://reacttraining.com/react-router/web/api/withRouter):如果你用了redux，那么你一定要引入它。
```javascript
export default withRouter(connect(
    state => ({
        todoList: state.todoReducer.todoList
    })
)(Main));
```

*如果你在使用hash时遇到`Warning: Hash history cannot PUSH the same path; a new entry will not be added to the history stack`错误，可以将push改为replace即*
```javascript
<NavLink
	replace={true}
	to="/2"
	activeClassName="selected"
	>切换到2号</NavLink>
```
6. 设置初始化路由：
* `BrowserRouter`和`HashRouter`:
```javascript
const history = createHistory();
history.push('2');
```
* `MemoryRouter`:
```javascript
const history = createMemoryHistory({
    initialEntries: ['/2']
});
```

# 使用redux-thunk

[redux-thunk](https://www.npmjs.com/package/redux-thunk) 是一个比较流行的 redux 异步 action 中间件，比如 action 中有 setTimeout 或者通过 fetch通用远程 API 这些场景，那么久应该使用 redux-thunk 了。redux-thunk 帮助你统一了异步和同步 action 的调用方式，把异步过程放在 action 级别解决，对 component 没有影响。

1. 安装`redux-thunk`:
```bash
npm install redux-thunk --save
```
2. 修改`store.js`:
```javascript

import { createStore,applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import mainReducer from './reducers/main';
...
const store = createStore(mainReducer, applyMiddleware(thunk));
...
export default store;
```
3. 在`action.js`使用redux-thunk：
```javascript
export const getData = (obj) => (dispatch, getState) => {
  setTimeout(()=>{
  	dispatch({
  		type: GET_DATA,
    	obj: obj
  	});
  },1000);
};
```

# 使用axios和async/await

[axios](https://github.com/axios/axios) 是一个基于Promise 用于浏览器和 nodejs 的 HTTP 客户端：

* 从浏览器中创建 XMLHttpRequest
* 从 node.js 发出 http 请求
* 支持 Promise API
* 自动转换JSON数据

1. 安装axios:
```bash
npm install axios --save
```

2. 在action中使用axios：
```javascript
import axios from 'axios';
export const getData = (obj) => (dispatch, getState) => {
    axios.get('/json/comments.json').then((resp)=>{
	     	dispatch({
				type: GET_DATA,
				obj: resp
			});
	});
};
```
