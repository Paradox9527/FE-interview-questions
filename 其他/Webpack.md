### Webpack

----

### 对webpack的理解？

----

`webpack` 是一个用于现代 JavaScript 应用程序的**静态模块打包工具**。我们可以使用`webpack`管理模块。因为在`webpack`看来，项目中的所有资源皆为模块，通过分析模块间的依赖关系，在其内部构建出一个依赖图，最终编绎输出模块为 HTML、JavaScript、CSS 以及各种静态文件（图片、字体等），让我们的开发过程更加高效。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d38cf4cbcf42491aa983140403de81c3~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?) `webpack`的主要作用如下：

- `模块打包`。可以将不同模块的文件打包整合在一起，并且保证它们之间的引用正确，执行有序。利用打包我们就可以在开发的时候根据我们自己的业务自由划分文件模块，保证项目结构的清晰和可读性。
- `编译兼容`。在前端的“上古时期”，手写一堆浏览器兼容代码一直是令前端工程师头皮发麻的事情，而在今天这个问题被大大的弱化了，通过`webpack`的`Loader`机制，不仅仅可以帮助我们对代码做`polyfill`，还可以编译转换诸如`.less`，`.vue`，`.jsx`这类在浏览器无法识别的格式文件，让我们在开发的时候可以使用新特性和新语法做开发，提高开发效率。
- `能力扩展`。通过`webpack`的`Plugin`机制，我们在实现`模块化打包`和`编译兼容`的基础上，可以进一步实现诸如按需加载，代码压缩等一系列功能，帮助我们进一步提高自动化程度，工程效率以及打包输出的质量。

### webpack的构建流程？

----

`webpack`的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：

- `初始化参数`：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数
- `开始编译`：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译
- `确定入口`：根据配置中的 entry 找出所有的入口文件
- `编译模块`：从入口文件出发，调用所有配置的 loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理
- `完成模块编译`：在经过上一步使用 loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系
- `输出资源`：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会
- `输出完成`：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统

在以上过程中，`webpack`会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，并且插件可以调用`webpack`提供的 API 改变`webpack`的运行结果。

**简单说：**

- 初始化：启动构建，读取与合并配置参数，加载 Plugin，实例化 Compiler
- 编译：从 entry 出发，针对每个 Module 串行调用对应的 loader 去翻译文件的内容，再找到该 Module 依赖的 Module，递归地进行编译处理
- 输出：将编译后的 Module 组合成 Chunk，将 Chunk 转换成文件，输出到文件系统中

### 常见的loader有哪些？

----

默认情况下，`webpack`只支持对`js`和`json`文件进行打包，但是像`css`、`html`、`png`等其他类型的文件，`webpack`则无能为力。因此，就需要配置相应的`loader`进行文件内容的解析转换。

常用的`loader`如下：

- `image-loader`：加载并且压缩图片文件。
- `less-loader`： 加载并编译 LESS 文件。
- `sass-loader`：加载并编译 SASS/SCSS 文件。
- `css-loader`：加载 CSS，支持模块化、压缩、文件导入等特性，使用`css-loader`必须要配合使用`style-loader`。
- `style-loader`：用于将 CSS 编译完成的样式，挂载到页面的 style 标签上。需要注意 `loader` 执行顺序，`style-loader` 要放在第一位，`loader` 都是从后往前执行。
- `babel-loader`：把 ES6 转换成 ES5
- `postcss-loader`：扩展 CSS 语法，使用下一代 CSS，可以配合 `autoprefixer` 插件自动补齐 CSS3 前缀。
- `eslint-loader`：通过 ESLint 检查 JavaScript 代码。
- `vue-loader`：加载并编译 Vue 组件。
- `file-loader`：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)
- `url-loader`：与 `file-loader` 类似，区别是用户可以设置一个阈值，大于阈值会交给 `file-loader` 处理，小于阈值时返回文件 base64 形式编码 (处理图片和字体)

[更多loader，点击此处查看官方文档](https://link.juejin.cn?target=https%3A%2F%2Fwww.webpackjs.com%2Floaders%2F)

### 常见的plugin有哪些？

----

`webpack`中的`plugin`赋予其各种灵活的功能，例如打包优化、资源管理、环境变量注入等，它们会运行在`webpack`的不同阶段（钩子 / 生命周期），贯穿了`webpack`整个编译周期。目的在于**解决 loader 无法实现的其他事**。

常用的`plugin`如下：

- `HtmlWebpackPlugin`：简化 HTML 文件创建 (依赖于 html-loader)
- `mini-css-extract-plugin`: 分离样式文件，CSS 提取为独立文件，支持按需加载 (替代extract-text-webpack-plugin)
- `clean-webpack-plugin`: 目录清理

[更多plugin，点击此处查看官方文档](https://link.juejin.cn?target=https%3A%2F%2Fwww.webpackjs.com%2Fplugins%2F)

### loader和plugin的区别？

----

`loader`是文件加载器，能够加载资源文件，并对这些文件进行一些处理，诸如编译、压缩等，最终一起打包到指定的文件中；`plugin`赋予了`webpack`各种灵活的功能，例如打包优化、资源管理、环境变量注入等，目的是解决 `loader`无法实现的其他事。

在运行时机上，`loader` 运行在打包文件之前；`plugin`则是在整个编译周期都起作用。

在配置上，`loader`在`module.rules`中配置，作为模块的解析规则，类型为数组。每一项都是一个 Object，内部包含了 `test(类型文件)`、`loader`、`options (参数)`等属性；`plugin`在 `plugins`中单独配置，类型为数组，每一项是一个 `plugin` 的实例，参数都通过构造函数传入。

### webpack的热更新原理是？

----

`模块热替换(HMR - hot module replacement)`，又叫做`热更新`，在不需要刷新整个页面的同时更新模块，能够提升开发的效率和体验。热更新时只会局部刷新页面上发生了变化的模块，同时可以保留当前页面的状态，比如复选框的选中状态等。

热更新的核心就是客户端从服务端拉去更新后的文件，准确的说是 chunk diff (chunk 需要更新的部分)，实际上`webpack-dev-server`与浏览器之间维护了一个`websocket`，当本地资源发生变化时，`webpack-dev-server`会向浏览器推送更新，并带上构建时的`hash`，让客户端与上一次资源进行对比。客户端对比出差异后会向`webpack-dev-server`发起 Ajax 请求来获取更改内容(文件列表、hash)，这样客户端就可以再借助这些信息继续向`webpack-dev-server`发起 jsonp 请求获取该`chunk`的增量更新。

后续的部分(拿到增量更新之后如何处理？哪些状态该保留？哪些又需要更新？)由`HotModulePlugin` 来完成，提供了相关 API 以供开发者针对自身场景进行处理，像`react-hot-loader`和`vue-loader`都是借助这些 API 实现热更新。

### 如何提高webpack的构建速度？

----

1. 代码压缩

   - JS压缩
      `webpack 4.0`默认在生产环境的时候是支持代码压缩的，即`mode=production`模式下。 实际上`webpack 4.0`默认是使用`terser-webpack-plugin`这个压缩插件，在此之前是使用 `uglifyjs-webpack-plugin`，两者的区别是后者对 ES6 的压缩不是很好，同时我们可以开启 `parallel`参数，使用多进程压缩，加快压缩。

   - CSS压缩
      CSS 压缩通常是去除无用的空格等，因为很难去修改选择器、属性的名称、值等。可以使用另外一个插件：`css-minimizer-webpack-plugin`。

   - HTML压缩

     使用HtmlWebpackPlugin 插件，通过minify进行html优化

     ```javascript
     module.exports = {
         plugin:[
             new HtmlwebpackPlugin({
                 minify:{
                     minifyCSS: false, // 是否压缩css
                     collapseWhitespace: false, // 是否折叠空格
                     removeComments: true // 是否移除注释
                 }
             })
         ]
     }
     ```

2. 图片压缩
    配置`image-webpack-loader`

3. Tree Shaking是一个术语，在计算机中表示消除死代码，依赖于ES Module的静态语法分析（不执行任何的代码，可以明确知道模块的依赖关系）。 在webpack实现Tree shaking

   有两种方案：

   - usedExports：通过标记某些函数是否被使用，之后通过 Terser

      来进行优化的

     ```javascript
     module.exports = {
         ...
         optimization:{
             usedExports
         }
     }
     ```

     使用之后，没被用上的代码在webpack打包中会加入unused harmony export mul注释，用来告知Terser在优化时，可以删除掉这段代码。

   - sideEffects：跳过整个模块/文件，直接查看该文件是否有副作用sideEffects用于告知webpack compiler哪些模块时有副作用，配置方法是在package.json中设置sideEffects

     属性。如果sideEffects

     设置为false，就是告知webpack可以安全的删除未用到的exports。如果有些文件需要保留，可以设置为数组的形式，如：

     ```json
     "sideEffecis":[
         "./src/util/format.js",
         "*.css" // 所有的css文件
     ]
     ```

4. 缩小打包域
    排除`webpack`不需要解析的模块，即在使用`loader`的时候，在尽量少的模块中去使用。可以借助 `include`和`exclude`这两个参数，规定`loader`只在那些模块应用和在哪些模块不应用。

更多优化构建速度方式，推荐阅读：[浅谈 webpack 性能优化（内附 webpack 学习笔记）](https://link.juejin.cn?target=https%3A%2F%2Fzhuanlan.zhihu.com%2Fp%2F139498741)

#### vite和webpack的区别

----

- vite：工程化工具，脚手架

  vite启动项目不会对项目进行完全打包，启动服务，将资源通过服务进行托管，项目加载而是通过由浏览器利用esm（es module）的方式加载资源。使用到资源才会加载才会处理，实际上时让浏览器接管了打包程序的部分工作

- webpeack：先读取配置稳健，根据配置文件的规则进行打包，读打包的入口文件，根据入口文件的依赖进行打包。js转低级语法：babel。三方文件：loader，html-webpack-plugin，css文件的抽离：mini-css-extract-plugin，启动浏览器，浏览器加载资源，直接加载不会再次处理。



### 简单的实践操作 webpack5

-----

#### 在线文档

https://www.webpackjs.com/

https://www.npmjs.com/

#### 安装

```shell
npm i webpack webpack-cli -D
```

#### 简单使用

在任意 js 文件中，简单编写一段打印或一个方法等。完成后在控制台中输入以下代码查看 webpack 打包情况。

```shell
npx webpack ./src/index.js --mode development
```

#### 五大核心概念

1. entry（入口）：指 webpack 从哪个文件开始打包。
2. output（输出）：指 webpack 打包完的文件输出到哪里，如何命名等。
3. loader（加载器）：webpack 本身只能处理 js、json 等资源，其他资源需要借助 loader，webpack 才能解析。
4. plugin（插件）：扩展 webpack 功能。
5. mode（模式）：

主要分为两种模式：

development：开发模式

production：生产模式

#### 配置 webpack

webpack 默认读取项目根目录下 webpack.config.js 文件

#### webpack 基本配置

```javascript
const path = require("path")

module.exports = {
  //entry入口
  entry: "./src/index.js",//相对路径
  //output输出
  output: {
    //文件输出的路径
    //__dirname nodejs的变量，代表当前文件的文件夹目录
    path: path.resolve(__dirname, "dist"),//绝对路径
    filename: "built.js"
  },
  //loader加载器
  module: {
    rules: [
      //loader的配置
    ]
  },
  //plugins插件
  plugins: [
    //plugin的配置
  ],
  //mode模式
  mode: "development"
}
```

此时在打包代码，控制台命令简化为如下命令

```shell
npx webpack
```



### webpack 处理各类型资源

#### 样式资源

#### css 文件

1. 包下载

```shell
npm i css-loader style-loader -D
```

1. 需要打包文件引入

```javascript
import "./src/css/index.css"
```

1. webpack 配置
2. 应保证 loader 的先后顺序：[`'style-loader'`](https://www.webpackjs.com/loaders/style-loader) 在前，而 [`'css-loader'`](https://www.webpackjs.com/loaders/css-loader) 在后。如果不遵守此约定，webpack 可能会抛出错误。

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,//检测规则
        use: [//执行顺序，从右到左（从上到下）
            "style-loader",//将JS中css通过创建style标签添加至html文件中。
            "css-loader"//将css资源编译成commonjs的模块到js中
        ],
      },
    ],
  },
};
```

**详细文档**

https://webpack.docschina.org/loaders/css-loader#root



#### less 文件

1. 包下载

```shell
npm i less less-loader -D
```

1. 需要打包文件引入

```javascript
import "./src/less/index.less"
```

1. webpack 配置

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.less$/,
        use: [
          'style-loader',
          'css-loader',
          'less-loader',//将less编译成css文件
        ],//可以使用多个loader
      },
    ],
  },
};
```

**详细文档**

https://webpack.docschina.org/loaders/less-loader#root



#### sass 文件

步骤如上，不做赘述

**详细文档**

https://webpack.docschina.org/loaders/sass-loader/#root



#### style文件

步骤如上，不做赘述

**详细文档**

https://webpack.docschina.org/loaders/stylus-loader/#root



### 图片资源

webpack4，需要通过 file-loader、url-loader 处理。

webpack5 已将这两个 loader 功能内置到 webpack 中了。

#### 配置 webpack

```javascript
module.exports = {
    module: {
        rules: [
            {
                test: /\.(png|jpe?g|gif|webp)$/,
                type: "asset",
                parser: {
                    dataUrlCondition: {
                        maxSize: 10 * 1024 // 10kb 内的图片打包为 base64 格式，有点减少请求数量
                    }
                },
                generator: {
                    filename: "static/images/[hash:10][ext][query]"//输出文件路径
                }
            }
        ]
    }
}
```



### 打包其他资源

#### 配置 webpack

```javascript
module.exports = {
    module: {
        rules: [
            {
                test: /\.(mp3|mp4)$/,
                type: "asset/resource",
                generator: {
                    filename: "static/media/[hash:10][ext][query]"
                }
            }
        ]
    }
}
```

**asset 详细配置**

https://webpack.docschina.org/guides/asset-modules#inlining-assets



### 清除上次打包内容

#### 配置 webpack

```javascript
module.exports = {
    output: {
        //文件输出的路径
        //__dirname nodejs的变量，代表当前文件的文件夹目录
        path: path.resolve(__dirname, "dist"),//绝对路径
        filename: "built.js",
        clean: true//清除上次的打包文件
    }
}
```



### eslint

#### vscode 安装 eslint

[https://www.shuzhiduo.com/topic/vscode%E5%AE%89%E8%A3%85eslint%E6%8F%92%E4%BB%B6/](https://www.shuzhiduo.com/topic/vscode安装eslint插件/)

#### webstorm 安装 eslint

https://www.yisu.com/zixun/172825.html

#### eslint 配置文件格式

- .eslintrc.*：需位于项目根目录

- - .eslintrc
  - .eslintrc.js
  - .eslintrc.json

- package.json 中配置 eslintConfig

#### .eslintrc.js 配置

```javascript
module.exports = {
    //解析选项
    parserOptions: {
        ecmaVersion: 6,//es语法版本
        sourceType: "module",//es模块化
    },
    //具体检查规则
    rules: {
        "no-var": 2,//不能使用 var 定义变量
    },
    //继承其他规则
    extends: ["eslint:recommended"],//继承eslint规则
    env: {
        node: true,//启用 node 中全局变量
        browser: true,//启用浏览器中全局变量
    }
}
```

**可配置规则**

https://eslint.bootcss.com/docs/rules/



#### webpack 集成

1. 安装包

```shell
npm i eslint-webpack-plugin eslint -D
```

1. webpack 配置

```javascript
const ESLintPlugin = require('eslint-webpack-plugin');

module.exports = {
  // ...
    plugins: [new ESLintPlugin({
        context: path.resolve(__dirname, "src")
    })],
  // ...
};
```

**详细配置**

https://webpack.docschina.org/plugins/eslint-webpack-plugin/#root



### babel

#### 配置文件格式

- babel.config.*：新建文件位于项目根目录 

- - babel.config.js
  - babel.config.json

- .babelrc.*：新建文件位于项目根目录 

- - .babelrc
  - .babelrc.js
  - .babelrc.json

- package.json 中 babel：不需要创建文件，在原文件基础上写

#### 配置配置文件

举例：babel.config.js

```javascript
module.exports = {
    //预设
    presets: [
        "@babel/preset-env"//允许使用最新的 JavaScript
    ]
}
```

#### webpack 集成

1. 安装

```shell
npm i babel-loader @babel/core @babel/preset-env -D
```

1. babel.config.js 配置

```javascript
module.exports = {
    presets: ["@babel/preset-env"]
}
```

1. webpack 配置

```javascript
module.exports = {
    module: {               //loader配置
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/, // 排除依赖里的，依赖里的辅助代码
                loader: "babel-loader"
            }
        ]
    }
}
```

**详细配置**

https://webpack.docschina.org/loaders/babel-loader#root



### html 资源

1. 安装

```shell
npm i html-webpack-plugin -D
```

1. webpack 配置

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin")

module.exports = {
    plugins: [
        new HtmlWebpackPlugin()
    ]
}
```

**详细配置**

https://webpack.docschina.org/plugins/html-webpack-plugin/#root



### webpack-dev-server

1. 安装

```shell
npm i webpack-dev-server -D
```

1. webpack 配置

```javascript
module.exports = {
  devServer: {
    host: "localhost",
    port: 3000,
    open: true
  }
}
```

1. 启动命令

```shell
npx webpack serve
```

**详细配置**

https://webpack.docschina.org/guides/development#using-webpack-dev-server



### 命令行简化

#### package.json

```json
{
    "scripts": {
    "start": "npm run dev",
    "dev": "webpack serve --config ./webpack-dev.config.js",
    "build": "webpack --config ./webpack.config.js"
  }
}
```

### 其他网址

https://www.babeljs.cn/docs/usage



## 生产环境基础配置 webpack-prod

### 提取 CSS 成单独文件

本插件会将 CSS 提取到单独的文件中，为每个包含 CSS 的 JS 文件创建一个 CSS 文件，并且支持 CSS 和 SourceMaps 的按需加载。

避免在网络慢的情况加页面加载出现闪屏的现象（长时间空白，内容图片闪现的情况）。

验证方式：Slow 3G 可模拟该情况。游览器中调试

### 下载包

```shell
npm i -D mini-css-extract-plugin
```

### webpack 配置

```javascript
//1.导入插件
const MiniCssExtractPlugin = require("mini-css-extract-plugin")

//2.loader 替换，将 style-loader 替换为 MiniExtractPlugin.loader
module.exports = {

	module: {
  	rules: [
      {
          test: /\.css$/,
          use: [
              MiniCssExtractPlugin.loader,//提取 CSS 文件成单独文件
              "css-loader"
          ]
      },
    ]
  },

  plugins: [
  	new MiniCssExtractPlugin({
      filename: "static/css/main.css"
    })
  ]
}
```

### 参考文档

https://webpack.docschina.org/plugins/mini-css-extract-plugin

## CSS 兼容性处理

### 下载包

```shell
npm i -D postcss-loader postcss postcss-preset-env
```

### webpack 配置

编写位置：css-loader 下方，其他样式处理 loader 上方

```javascript
module.exports = {
	module: {
  	rules: [
      {
          test: /\.css$/,
          use: [
              MiniCssExtractPlugin.loader,//提取 CSS 文件成单独文件
              "css-loader",
            	{
                loader: "postcss-loader",
                options: {
                  postcssOptions: [
                  	"postcss-preset-env" //能解决大多数样式兼容问题
                  ]
                }
              }
          ]
      },
    ]
  }
}
```

### package.json 配置

配置 CSS 兼容到的版本，详细配置如下。

```json
{
	"browserslist": [
    "ie >= 8"
  ]
}

{
  "browserslist": [
    "last 2 version",
    "> 1%",
    "not dead"
  ]
}
```

### 样式 loader 封装

减少冗余代码，提高代码复用性

```javascript
const MiniCssExtractPlugin = require("mini-css-extract-plugin")

function getStyleLoader(pre) {
  return [
    MiniCssExtractPlugin.loader,//提取 CSS 文件成单独文件
    "css-loader",
    {
      loader: "postcss-loader",
      options: {
        postcssOptions: [
          "postcss-preset-env" //能解决大多数样式兼容问题
        ]
      }
    },
    pre
  ].filter(Boolean)
}

module.exports = {
	module: {               //loader配置
    rules: [
      {
        test: /\.css$/,
        use: getStyleLoader()
      }
    ]
	}
}
```

### 参考文档

https://webpack.docschina.org/loaders/postcss-loader

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter

## CSS 压缩

### 下载包

```json
npm i -D css-minimizer-webpack-plugin
```

### webpack 配置

```javascript
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin")

module.exports = {
	plugins: [
    new CssMinimizerPlugin()
  ]
}
```

### 参考文档

https://webpack.docschina.org/plugins/css-minimizer-webpack-plugin

## html、js 压缩

当 webpack.config.js 中 mode 改为 production webpack 会默认对 html、js 资源做压缩，无需额外配置

# 提升开发体验

## source map

### 使用原因

开发环境时 webpack 默认打包生成的代码都是压缩过的，报错时无法准确的定位错误代码的行和列，排查错误难度加大。source map 可以很好的解决这个问题。

### 概述

source map 是一个用来生成源代码与构建后代码一一映射的文件的方案。

### webpack 配置

```javascript
module.exports = {
	devtool: "source-map"
}
```

#### 可能值

[inline-|hidden-|eval-][nosources-][cheap-[module-]]source-map

开发环境：

- source-map：映射转换后的代码与源代码之间的关系，会精确到行和列，方便调试精准定位错误。
- nosources-source-map：只能展示错误信息，点击错误无法查看源代码。
- hidden-nosources-source-map：只能展示错误信息。
- hidden-source-map：只有错误代码错误原因，但没有错误位置，不能追踪源代码错误，只能提示构建后代码的代码位置。

生成环境建议删除 devtool 属性

### 参考文档

https://webpack.docschina.org/configuration/devtool

# 提升打包构建速度

## HRM

### 使用原因

开发时我们修改了其中一个模块代码，webpack 默认会将所有模块全部重新打包编译，速度很慢。

HRM 可以很好的解决这一问题，只重新打包编译修改的模块。

注意：js 文件无法自动 HRM 功能，需要手动编写。

## webpack 配置

```javascript
module.exports = {
	devServer: {
      host: "localhost",
      port: 3000,
      open: true,//是否自动打开浏览器
      hot: true//开启HRM功能（只能用于开发环境，生产环境不需要）webpack5此配置已是默认配置想体验比较的小伙伴可以配置该属性。
  }
}
```

### js 开启 HRM 功能

```javascript
if(module.hot) {
  //判断是否支持热模块替换功能 
	module.hot.accept("./js/test1.js")
}
```

## oneOf

由于 loader 匹配机制，每个文件都会比较所有 loader 配置，oneOf 可以很好的解决该问题，每个文件只匹配到一个 loader 后就会跳出后续的 loader 配置。

### webpack 配置

```javascript
module.exports = {
	module: {               //loader配置
      rules: [
          {
              oneOf: [
                  {
                      test: /\.css$/,
                      use: getStyleLoader()
                  },
                  {
                      test: /\.js$/,
                      exclude: /node_modules/,
                      loader: "babel-loader"
                  }
              ]
          }
      ]
  }
}
```

## cache

对 babel、eslint 处理结果进行缓存，让后续打包速度更快。

### webpack 配置

```javascript
module.exports = {
	module: {               //loader配置
        rules: [
            {
                oneOf: [
                    {
                        test: /\.js$/,
                        exclude: /node_modules/,
                        loader: "babel-loader",
                        options: {
                            cacheDirectory: true,//开启 babel 缓存
                            cacheCompression: false//关闭缓存文件压缩
                        }
                    }
                ]
            }
        ]
    },
  plugins: [
        new ESLintPlugin({
            context: path.resolve(__dirname, "../src"),
            exclude: "node_modules", //默认值
            cache: true,//开启缓存
            cacheLocation: path.resolve(__dirname, "../node_modules/.cache/eslintcache")//指定缓存目录
        })
    ]
}
```

## tree shaking

移除 JavaScript 中没有使用上的代码。

注意：它依赖 ES module

webpack 已经默认开启这个功能，无需其他配置。

### 参考文档

https://zhuanlan.zhihu.com/p/339893674

## babel 优化

### @babel/plugin-transform-runtime

三大作用：

1.自动移除语法转换后内联的辅助函数（inline Babel helpers），使用@babel/runtime/helpers里的辅助函数来替代；

2.当代码里使用了core-js的API，自动引入@babel/runtime-corejs3/core-js-stable/，以此来替代全局引入的core-js/stable;

3.当代码里使用了Generator/async函数，自动引入@babel/runtime/regenerator，以此来替代全局引入的regenerator-runtime/runtime；

### 参考文档

https://zhuanlan.zhihu.com/p/394783727

## 图片压缩

注：仅限本地图片，若项目中的图片都是在线的图片，就无需该配置了。

### 下载包

```shell
npm i -D image-minimizer-webpack-plugin imagemin
```

### 无损压缩

```shell
npm install imagemin-gifsicle imagemin-jpegtran imagemin-optipng imagemin-svgo --save-dev
```

### 有损压缩

```shell
npm install imagemin-gifsicle imagemin-mozjpeg imagemin-pngquant imagemin-svgo --save-dev
```

### webpack 配置参考

官网小坑按照步骤配置打包报错

https://blog.csdn.net/weixin_40746230/article/details/125184770

## code split

痛点：

打包代码时会将所有js文件打包到一个文件中，体积太大了。我们如果只要渲染首页，就应该只加载首页的js文件，其他文件不应该加载。

主要功能：

- 分割文件：将打包生成的文件进行分割，生成多个js文件。
- 按需加载：需要哪个文件就加载哪个文件。

### webpack 配置参考

https://www.pudn.com/news/62a13f08d9e07d02aa496e68.html

## 总结

1. 提升开发体验

- 使用 SourceMap 让开发或上线时代码报错能有更加准确的错误提示。 

1. 提升打包构建速度

- 使用 HMR 让开发时只重新编译打包更新变化了的代码，不变的代码使用缓存，从而使更新速度更快。
- 使用 oneof 让资源文件一旦被某个 loader 处理了，就不会继续遍历了，打包速度更快。
- 使用 include / exclude 排除或只检测某些文件，处理的文件更少，速度更快。
- 使用 cache 对 eslint 和 babel 处理的结果进行缓存，让第二次打包速度更快。
- 使用 thead 多进程处理 eslint 和 babel 任务，速度更快。（需要注意的是，进程启动通信都有开销的，要在比较多代码处理时使用才有效果）。

1. 减少代码体积

- 使用 TreeShaking 剔除了没有使用的多余代码，让代码体积更小。
- 使用 @babel/plugin-transform-runtime 插件对 babel 进行处理，让辅助代码从中引入，而不是每个文件都生成辅助代码，从而体积更小。
- 使用 ImageMinimizer 对项目中图片进行压缩，体积更小，请求速度更快。（需要注意的是，如果项目中图片都是在线链接，那么就不需要了。本地项目静态图片才需要进行压缩。）

1. 优化代码运行性能

- 使用 CodeSplit 对代码进行分割成多个 js 文件，从而使单个文件体积更小，并行加载 js  速度更快。并通过 import 动态导入语法进行按需加载，从而达到需要使用时才加载该资源，不用时不加载资源。
- 使用 NetworkCache 能对输出资源文件进行更好的命名，将来好做缓存，从而用户体验更好。
- 使用 core-js 对 js 进行兼容性处理，让我们代码能运行在低版本浏览器。

### 兼容性查询

https://caniuse.com/

### babel

#### babel文档

https://www.babeljs.cn/docs/usage

#### es6文档

https://es6.ruanyifeng.com/

### 配置

1. 初始化

```shell
npm init
```

1. 安装所需包

```shell
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

1. 创建配置文件

2. 创建一个命名为 babel.config.json 的配置文件。（根目录创建）
3. 需要 v7.8.0 或更高版本。

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "edge": "17",
          "firefox": "60",
          "chrome": "46",
          "safari": "11.1"
        },
        "useBuiltIns": "usage",
        "corejs": "3.6.5"
      }
    ]
  ]
}
```

注：chrome 版本为 46 及以下时，才会转换为 es5 的语法。其他浏览器版本需要自己尝试。

1. 编译所需代码

```shell
npx babel src --out-dir lib
```

其他命令参数请输入 npx babel --help 查看，自行筛选

兼容性

降低corejs版本或者babel