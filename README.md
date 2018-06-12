# 目录

1. [版本说明](#banben)

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

```
react-family/
    |
   ├──dist/                    * 发布版本构建输出路径
    |
   ├──dev/               * 调试版本构建输出路径
   │
   │──src/                    * 工具函数
   │    |
    |     |—— db.js               * localstorage操作
    |     |
    |     |__ index.js            * 别的工具函数
   │
   │ 
   │——constants/                * 一些常量(其实没怎么用)
   │
   │
   │
   │──.eslint                * 全局的样式，其中也有修改了ant-mobile的样式
   │____.babelrc                  * babel配置文件
   │──.eslint                * 全局的样式，其中也有修改了ant-mobile的样式
   │____.babelrc                  * babel配置文件
```

## Docs

* 使用者文档
	* [FIE介绍](docs/use-summary.md)
	* [安装FIE](docs/use-install.md)
	* [FIE基础命令详解](docs/use-cli.md)
	* [使用FIE套件](docs/use-toolkit.md)
	* [使用FIE插件](docs/use-plugin.md)
	* [FIE配置文件](docs/use-config.md)
* 开发者文档
	* [FIE API](packages/fie-api/README.md)
	* [套件开发指南](docs/dev-toolkit.md)
	* [插件开发指南](docs/dev-plugin.md)
* 参与FIE开发
	* [开发环境安装及代码组织](docs/dev-env.md)
	* [FIE开发规范](docs/dev-rules.md)

## Usage

可在终端输入`$ fie -h` 查看fie使用帮助

```bash
fie 使用帮助:  $ fie [command] [options]

  $  fie                     # 显示fie帮助信息,若目录下有使用的套件,则会同时显示套件的帮助信息
  $  fie init [toolkitName]  # 初始化套件
  $  fie install [name]      # 安装插件
  $  fie update [name]       # 更新插件
  $  fie list [type]         # 插件列表
  $  fie ii                  # 安装npm模块，类似于npm install，但安装速度更快更稳定
  $  fie clear               # 清空 fie 的本地缓存
  $  fie switch              # 切换 fie 的开发环境
  $  fie help                # 显示套件帮助信息
  $  fie [name]              # 其他调用插件命令

 Options:

   -h, --help                显示fie帮助信息
   -v, --version             显示fie版本
```

### Quick start

以 [fie-toolkit-blue](https://www.npmjs.com/package/fie-toolkit-blue) 套件为例，讲解开发流程。

1. 安装fie到npm全局环境中

	```bash
	$ npm install fie -g --registry=https://registry.npm.taobao.org
	```

2. 初始化项目

	```bash
	# 创建并进入项目文件夹
	$ mkdir my-project && cd $_
	
	# 初始化blue的开发环境
	$ fie init blue
	```
	
3. 开启本地环境

	```bash
	# 开启blue的开发环境
	$ fie start
	```
4. 项目编译及打包

	```bash
	# 打包blue项目的到指定的目录
	$ fie build
	```	

## Support

1. 使用过程中遇到的相关问题，可在github上提相关的issues : https://github.com/fieteam/fie/issues/new
2. 也可通过钉钉或旺旺联系：@宇果

## License

[GNU GPLv3](LICENSE)

