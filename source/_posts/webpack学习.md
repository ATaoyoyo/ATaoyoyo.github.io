---
title: webpack学习
date: 2021-07-18 13:58:39
tags:
cover: /img/article/webpack/webpack.jpg
top_img: /img/article/webpack/webpack.jpg
description: 学一学webpack！！！
---

# 基础概念

> 本质上，webpack 是一个现代 JavaScript 应用程序的静态模块打包工具。当 webpack 处理应用程序时，它会在内部构建一个 依赖图(dependency graph)，此依赖图会映射项目所需的每个模块，并生成一个或多个 bundle。

webpack 主要有五个核心概念：

- 入口
- 出口
- loader
- 插件
- 模式

## 入口

入口，通常是一个文件，用来指定 webpack 应该使用哪个模块用来构建内部依赖。

```js
module.exports = {
  entry: './src/index.js',
}
```

## 出口

出口，指 webpack 打包过后生成浏览器可识别的 js 文件等。

```js
module.exports = {
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'main.j',
  },
}
```

## loader

webpack 只能理解 JavaScript 和 JSON 文件。loader 配置可以让 webpack 识别并处理其他类型的文件，然后转换成有效的模块，以供程序使用。

loader 中有两个常用的配置属性，test 和 use。其中 test 的值为正则表达式，匹配需要处理哪些文件；use 则是使用什么 loade 来进行处理。

```js
module.exports = {
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'main.j',
  },

  module: {
    rules: [{ test: /\.jpg$/i, use: { loader: 'file-loader' } }],
  },
}
```

## 插件

插件可以用来优化打包的过程，配置打包环境等。

```js
module.exports = {
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'main.j',
  },

  plugins: [new HtmlWebpackPlugin()],
}
```

## 模式

通过选择 development, production 或 none 之中的一个，来设置 mode 参数，你可以启用 webpack 内置在相应环境下的优化。其默认值为 production。

```js
module.exports = {
  mode: 'production',
}
```

# 常用配置

## 使用 `loader` 打包静态资源

### 图片

查阅了 webpack 官方文档，发现对于文件打包，webpack4 使用的是 file-loader，但是在 webpck5 的版本中，这个 loader 已经被废弃。于是我在 webpack5 中试了一下，但是依然还是可以用的，只不过还是按照官方文档说的来吧。
现在使用的是 asset module，允许使用资源文件（字体，图标等）而无需配置额外 loader。

`asset/resource`就是用来取代 webpack4 中的`file-loader`的。

```js
module.export = {
  module: {
    rules: [{ test: /\.png$/i, type: 'asset/resource' }],
  },
}
```

配置好之后打包，发现图片正常出来了，但是图片名称是 hash 值，并且没有在 img 文件夹中，可以使用`assetModuleFilename`来指定输出：

```js
module.export = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
    assetModuleFilename: 'images/[name][ext][query]',
  },
  module: {
    rules: [{ test: /\.png$/i, type: 'asset/resource' }],
  },
}
```

配置好之后，再次打包，可以发现所有图片都能够被打包到 images 文件夹中了，并且名称已经不是 hash 值了。

在 webpack4 中，图片转 base64 使用的是`url-loader`，但是在 webpack5 中也已经被废弃了，取而代之的为`asset/inline`。

```js
module.export = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
    assetModuleFilename: 'images/[name][ext][query]',
  },
  module: {
    rules: [{ test: /\.png$/i, type: 'asset/inline' }],
  },
}
```

打包后发现没有了 images 文件夹以及图片，这是因为图片被转成了 base64 编码。一般来说，图片过大就不适合转换成 base64 编码了，那么什么时候转换为 base64，什么时候不转换为 base64 呢？可以直接设置`type`为`asset`。webpack5 已经帮我们实现了这方面的规则，小于 8kb 的文件，将会视为 inline 模块类型，否则会被视为 resource 模块类型。

如果决定这个规则不好， 这时候可以使用`Rule.parser.dataUrlCondition.maxSize`来对文件大小做限制。

```js
module.export = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
    assetModuleFilename: 'images/[name][ext][query]',
  },
  module: {
    rules: [
      {
        test: /\.png$/i,
        type: 'asset',
        parser: {
          dataUrlCondition: {
            maxSize: 204800, // 200kb
          },
        },
      },
    ],
  },
}
```

### 样式

常用的样式 `loader` 有 `css-loader` 与 `style-loader`，在写 `rules` 规则时，需要注意样式 `loader` 的顺序是反着来的，`css-loader`放在后面，`style-loader`放在前面。

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
}
```

如果使用的是`sass`或者`less`等 `css` 预处理器，得先将其编译为`css`，然后使用`css-loader`和`style-loader`进行编译打包。

比方说用`sass`:

```shell
npm install sass-loader sass --save-dev
```

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: [
          // Creates `style` nodes from JS strings
          'style-loader',
          // Translates CSS into CommonJS
          'css-loader',
          // Compiles Sass to CSS
          'sass-loader',
        ],
      },
    ],
  },
}
```

在使用到一些高级 `CSS` 语法特性时，需要加上厂商前缀，可以使用`postcss-loader`来进行帮助，首先安装`postcss`和`postcss-loader`：

```js
npm install --save-dev postcss-loader postcss
```

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [
          'style-loader',
          'css-loader',
          {
            loader: 'postcss-loader',
            options: {
              postcssOptions: {
                plugins: [['postcss-preset-env']],
              },
            },
          },
        ],
      },
    ],
  },
}
```

还有一个`autoprefixer`插件，也是帮助 `CSS` 添加厂商前缀的，但是默认的`postcss-preset-env`插件默认带上了`autoprefixer`，也可以不用配置。除此之外，需要添加一个`.browserslistrc`文件，添加以下内容之后`postcss`才会生效：

```shell
defaults,
not ie < 11,
last 2 versions,
> 1%,
iOS 7,
last 3 iOS versions
```

然后 CSS 高级语法特性就会自动添加厂商前缀了。

## 使用 `plugins` 配置让打包更编辑

### `html-webpack-plugin`

`html-webpack-plugin`可以配置一个`html`文件模版，打包之后生成一个`html`文件并且引入对应的`js`文件。

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const path = require('path');

module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js',
  },
  plugins: [new HtmlWebpackPlugin(template: path.resolve(__dirname, './src/index.html'))],
};
```

### `clean-webpack-plugin`

`clean-webpack-plugin`用于清除上次打包之后的文件。

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
const path = require('path')

module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js',
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({ template: path.resolve(__dirname, './src/index.html') }),
  ],
}
```

## `entry`与`output`基础配置

`entry`为打包入口，可以是一个字符串也可以是一个对象；为对象时，代表打包多个页面。对应的，如果`entry`打包多个页面，`output`中的`filename`就不能写死，否则会打包出错。可以使用占位符`[name]`动态替换打包后的文件名称，`name`对应`entry`中的 key 值。

```js
module.exports = {
  entry: {
    home: './home.js',
    about: './about.js',
    contact: './contact.js',
  },

  ourput: {
    filename: '[name].js',
  },
}
```

## `sourceMap`

代码调试映射

```shell
dev: cheap-module-eval-source-map
pro: cheap-module-source-map
```

## `devServer`

使用`devServer`可以开启一个服务，自动打开打包后的 index.html 文件，解决了每次打包完毕后手动启动的麻烦。

首先安装依赖：

```shell
npm install webpack-dev-server --save-dev
```

在`webpack.config.js`中添加以下代码：

```js
const path = require('path')
module.exports = {
  devServer: {
    contentBase: path.join(__dirname, './dist'), // 使用哪个目录用来提供服务
    compress: true, // 开启gizp压缩
    port: 8080, // 服务端口号
    open: true, // 自动打开浏览器
  },
}
```

添加服务之后，就可以不用每次改完代码都去重新打包了，但是必须要刷新浏览器才能让更改后的代码生效，这时可以使用热更新模块帮助页面刷新。

```js
const webpack = require('webpack')
const path = require('path')
module.exports = {
  devServer: {
    contentBase: path.join(__dirname, './dist'), // 使用哪个目录用来提供服务
    compress: true, // 开启gizp压缩
    port: 8080, // 服务端口号
    open: true, // 自动打开浏览器
    hot: true, // 打开热更新
    hotOnly: true, // 若是热更新失效，让浏览器自动刷新
  },

  plugins: [
    new webpack.HotModuleReplacementPlugin(), // 使用热更新模块
  ],
}
```

## 使用`babel`处理 ES6 语法

一些低版本的浏览器是无法识别 ES6 语法的，可以使用`babel`处理 ES6 语法的转换。

首先安装所需依赖：

```shell
npm install --save-dev babel-loader @babel/core
```

在`webpack.config.js`中添加 loader：

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
          },
        },
      },
    ],
  },
}
```

配置好 `loader` 之后，再次打包出来的 js 文件就是 ES5 的语法了，但是这样做 `babel` 会将所有的语法都转换为 ES6，出口文件的体积会变得很大。可以使用`@babel/polyfill` 来配置规则。

```shell
npm install --save @babel/polyfill
```

然后修改 `webpack.config.js`中的 `babel-loader` 文件：

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: [
              ['@babel/preset-env', { useBuiltIns: 'usage', target: { chrome: '58', ie: '11' } }],
            ],
          },
        },
      },
    ],
  },
}
```

再次打包，就可以发现代码体积变得很小了。

然后发现有一个警告，说是指定了 `useBuiltIns`，还需要配置 `corejs`：

```shell
npm install --save core-js@2
```

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: [
              [
                '@babel/preset-env',
                { useBuiltIns: 'usage', corejs: 2, target: { chrome: '58', ie: '11' } },
              ],
            ],
          },
        },
      },
    ],
  },
}
```

配置好后，便可以消除警告了。

### 组件库中的 `babel` 配置

如果需要编写组件库这类代码，上面的方法就不太合适。babel 转换后的代码是全局的，可能会影响其他的代码，若是业务代码，就没啥问题，但如果编写的是公共内库，就会有问题。

可以使用 `@babel/plugin-transform-runtime` 来解决这个问题，`@babel/plugin-transform-runtime`使用闭包可以避免命名重复的问题。

```shell
npm install --save-dev @babel/plugin-transform-runtime
```

```shell
npm install --save @babel/runtime @babel/runtime-corejs2
```

然后在 webpack.config.js 中修改 babel-loader：

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            plugins: [
              [
                '@babel/plugin-transform-runtime',
                {
                  absoluteRuntime: false,
                  corejs: 2, // 对应@babel/runtime-corejs2
                  helpers: true,
                  regenerator: true, // 不污染全局
                  version: '7.0.0-beta.0',
                },
              ],
            ],
          },
        },
      },
    ],
  },
}
```

## TreeShaking

TreeShaking 只支持 ESModule 模块导入
