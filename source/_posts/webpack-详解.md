---
title: webpack 详解
abbrlink: 16890
date: 2019-01-15 14:48:49
tags:
---







### 一、什么是webpack

webpack是一个打包构建工具，其宗旨就是可以打包一切静态资源，它所做的事情就是，分析你的项目，找到JavaScript模块以及其他的一些浏览器所不能直接运行的扩展语言（Scss、TypeScript等），并将其打包为合适的格式以供浏览器使用，当webpack处理应用程序时，它就会递归的构建一个依赖关系图，其包含应用程序需要的每一个模块，然后将所有这些模块打包成一个或多个bundle



<!--more-->

### 二、四大核心概念



#### 1、入口（entry）

指明webpack应该以哪个模块作为构建其内部依赖图的开始

例子：

```js
module.exports = {
  entry: './path/to/my/entry/file.js'
};
```



#### 2、出口（output）

告诉webpack它所创建的bundles要输出到哪里，以及叫什么名字（fileName），这边其默认值为./dist

例子：

```js
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',      //入口文件
  output: {
    path: path.resolve(__dirname, 'dist'),   //出口文件的位置
    filename: 'my-first-webpack.bundle.js'    // 构建的bundles的名字
  }
};
```



#### 3、loader

转换，webpack只能识别JavaScript的代码，对于一些非JavaScript的代码呢，这边就可以使用loader进行转换，也就是说loader可以让webpack去处理哪些非JavaScript的代码。loader可以将所有类型的文件转换为JavaScript能够处理的模块，进而打包，对他们进行处理。

例子：

```js
const path = require('path');

const config = {
  output: {
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
    //test是想要转换的文件类型，use是使用什么loader进行转换
      { test: /\.txt$/, use: 'raw-loader' }     
    ]
  }
};

module.exports = config;
```





#### 4、插件（plugins）

插件这边可以用来处理各式各样的任务，例如：打包优化、压缩、重新定义环境中的变量

而其使用方法也特别简单，只需要require它然后添加到plugins中就可以了

例子：

```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件

const config = {
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({template: './src/index.html'})   //在这边使用就可以了
  ]
};

module.exports = config;
```







### 三、webpack的执行流程

webpack启动后会在entry里配置的module开始递归解析entry所依赖的所有module，每找到一个module, 就会根据配置的loader去找相应的转换规则，对module进行转换后在解析当前module所依赖的module，这些模块会以entry为分组，一个entry和所有相依赖的module也就是一个chunk，最后webpack会把所有chunk转换成文件输出，在整个流程中webpack会在恰当的时机执行plugin的逻辑





### 四、webpack简单打包案例

#### 1、简陋打包

1、准备一个文件夹用于创建项目，

2、npm init -y 初始化项目

3、安装webpack、和webpack-cli

```
npm install webpack --global                // 安装全局webpack命令
npm install webpack webpack-cli --save-dev  // 安装本地项目模块

// install    可简写为i,
// --global   可简写为-g
// --save     可简写为-S
// --save-dev 可简写为-D
```

4、新建文件夹及文件

 ![img](/webpack/目录) 

4.1、hello.js中

```js
// hello.js 
 module.exports = function() {
    let hello = document.createElement('div');
    hello.innerHTML = "hello xxx!";
    return hello;
  };
```

4.2、index.js中

```
// index.js
const hello = require('./hello.js');
document.querySelector("#root").appendChild(hello());
```

4.3、index.html中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Webpack demo</title>
</head>
<body>
    <div id='root'></div>
    <script src="bundle.js"></script>   <!--这是打包之后的js文件，我们暂时命名为bundle.js-->
</body>
</html>
```

5、进行打包

```
webpack src/index.js --output dist/bundle.js
```

结果如下：

 ![img](/webpack/打包结果) 

注意：这边如果webpack是4.x的版本，有可能报错（大致意思是，现在必须要装webpack-cli，请问是否统一安装webpack-cli，也许你会觉得很奇怪，之前已经安装了，为什么这边会报没有安装，因为 4.x版本的webpack移除了CLI，作为一个独立的webpack-cli包，安装一下这个包就可以了 ，这边可以重新安装一下就行了）

![1577352260922](/webpack/重新安装-cli)

打开dist里面的index.html文件就可以看到（并且此时你能看到你的dist文件夹中出现了一个bundle.js文件）

![1577352336018](/webpack/index结果)



#### 2、通过配置文件来使用webpack

 在当前项目的根目录下新建一个配置文件webpack.config.js，我们写下如下简单配置代码，目前只涉及入口配置（相当于我们的index.js，从它开始打包）和出口配置（相当于我们打包生成的bundle.js）。 

```js
// webpack.config.js
const path = require('path');
module.exports = {
    entry: path.join(__dirname, "/src/index.js"), // 入口文件
    output: {
        path: path.join( __dirname, "/dist"), // 打包后的文件存放的地方 
        filename: "bundle.js" // 打包后输出文件的文件名
    }
}
```

现在只要在终端中输入webpack就可以打包

 **package.json文件中自定义脚本命令** 

 Node项目一般都有一个package.json文件，该文件用于描述当前项目，其中有一个scripts属性，该属性可以自定义脚本命令，例如我们运行的打包命令，那么可以在scripts里添加自定义脚本为： 

 ![img](/webpack/自定义脚本) 

之后就可以使用npm run build来运行该脚本命令，这样有什么好处呢？如果命令行很短，好处当然不明显了，但是如何命令行很长呢？那么我们可以在这里添加每次都需要执行的命令，配置了scripts后， npm run key值相当于在终端运行了value值



### 五、构建本地服务器

 上面案例我们是通过打开本地HTML文件来查看页面的，vue，react框架时都是运行在本地服务器上的，那我们能不能也改成那样呢？接下来学习如何构建本地服务 

#### 5.1、 webpack-dev-server配置本地服务器

 Webpack提供了一个可选的本地开发服务器，这个本地服务器基于node.js构建，它是一个单独的组件，在webpack中进行配置之前需要单独安装它作为项目依赖： 

```
npm install webpack-dev-server -D    //这边如果安装了淘宝镜像的话，可以用cnpm比较快
```



#### 5.2、添加配置项到webpack.config.js

```js
// webpack.config.js
const path = require('path');
module.exports = {
  entry: path.join(__dirname, "/src/index.js"), // 入口文件
  output: {
    path: path.join(__dirname, "/dist"), // 打包后的文件存放的地方 
    filename: "bundle.js" // 打包后输出文件的文件名
  },
  devServer: {
    contentBase: path.join(__dirname, "dist"),  //指定服务器资源的根目录，默认当前执行的目录
    hot: true,		//有点热编译的感觉
    port: '8080',    //端口，默认8080
    inline: true,    //当文件改变时自动刷新页面
    open: true,      //构建完成，自动使用默认的系统默认的浏览器打开
    overlay: true,	 //编译出错时，在浏览器上显示错误，默认为false
    proxy: {         //代理（解决跨域问题）
      '/api': {
        target: '', 
        changeOrigin: true,  
        pathRewrite: {
          '^/api': ''  
        }
      }
    }
  }
}
```





#### 5.3、在package.json文件中添加启动命令

```js
  "scripts": {
    "build": "webpack",
    "dev": "webpack-dev-server --open"
  },
```

我们用dev来启动本地服务器， webpack-dev-server就是启动服务器的命令，- -open是用于启动完服务器后自动打开浏览器，这时候我们自定义命令方式的便捷性就体现出来了，可以多个命令集成在一起运行，即我们定义了一个dev命令名称就可以同时运行了webpack-dev-server和- -open两个命令



```
npm run dev    //运行
```

注意：这边有可能会报这样的错误：Cannot read property 'properties' of undefined，可能是webpack-cli的版本冲突，可以改成3.1.1的版本





#### 5.4、source  Maps 调试配置

 作为开发，代码调试当然少不了，那么问题来了，经过打包后的文件，你是不容易找到出错的地方的，`Source Map`就是用来解决这个问题的。通过如下配置，我们会在打包时生成对应于打包文件的`.map`文件，使得编译后的代码可读性更高，更易于调试。 

```js
// webpack.config.js
const path = require('path');
module.exports = {
  entry: path.join(__dirname, "/src/index.js"), // 入口文件
  output: {
    path: path.join(__dirname, "/dist"), // 打包后的文件存放的地方 
    filename: "bundle.js" // 打包后输出文件的文件名
  },
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    hot: true,
    port: '8080',
    inline: true,
    open: true,
    overlay: true,
  },
  devtool: 'source-map' // 会生成对于调试的完整的.map文件，但同时也会减慢打包速度
}
```

配置好后，我们再次运行npm run build进行打包，这时我们会发现在dist文件夹中多出了一个bundle.js.map。如果我们的代码有bug，在浏览器的调试工具中会提示错误出现的位置，这就是devtool：'source-map' 配置项的作用。



### 六、loaders

loaders是webpack最强大的功能之一，通过不同的loader，webpack有能力调用外部的脚本或工具，实现对不同格式的文件的处理，例如把scss转为css，将ES66、ES7等语法转化为当前浏览器能识别的语法，将JSX转化为js等多项功能。Loaders需要单独安装并且需要在webpack.comfig.js中的modules配置项下进行配置，Loaders的配置包括以下几方面：

- test：一个用以匹配loaders所处理文件的拓展名的正则表达式（必须）
- loader：loader的名称（必须）
- include/exclude： 手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）
- options： 为loaders提供额外的设置选项（可选）

例：

配置css-loader

 如果我们要加载一个css文件，需要安装style-loader和css-loader 

```
npm install style-loader css-loader -D
```



```js
// webpack.config.js
const path = require('path');
module.exports = {
  entry: path.join(__dirname, "/src/index.js"), // 入口文件
  output: {
    path: path.join(__dirname, "/dist"), // 打包后的文件存放的地方 
    filename: "bundle.js" // 打包后输出文件的文件名
  },
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    hot: true,
    port: '8080',
    inline: true,
    open: true,
    overlay: true,
  },
  devtool: 'source-map', // 会生成对于调试的完整的.map文件，但同时也会减慢打包速度
  module: {
    rules: [
      {
        test: /\.css$/,   // 正则匹配以.css结尾的文件
        use: ['style-loader', 'css-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      }
    ]
  }
}
```

我们在src文件夹下新建index.css文件，设置body的样式

```css
/* index.css */
body {
    background: gray;
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

在src文件夹下的index.js引入它

```js
// index.js
import './index.css' // 导入css

const hello = require('./hello.js');
document.querySelector("#root").appendChild(hello());复制代码
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

运行npm run dev启动服务器，会发现页面背景颜色变成了灰色

**【6.2】配置sass**

```
npm install sass-loader node-sass -D // 因为sass-loader依赖于node-sass，所以还要安装node-sass
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```js
// webpack.config.js
const path = require('path');
module.exports = {
  entry: path.join(__dirname, "/src/index.js"), // 入口文件
  output: {
    path: path.join(__dirname, "/dist"), // 打包后的文件存放的地方 
    filename: "bundle.js" // 打包后输出文件的文件名
  },
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    hot: true,
    port: '8080',
    inline: true,
    open: true,
    overlay: true,
  },
  devtool: 'source-map', // 会生成对于调试的完整的.map文件，但同时也会减慢打包速度
  module: {
    rules: [
      {
        test: /\.css$/,   // 正则匹配以.css结尾的文件
        use: ['style-loader', 'css-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      },
      {
        test: /\.(scss|sass)$/,   // 正则匹配以.scss和.sass结尾的文件
        use: ['style-loader', 'css-loader', 'sass-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      }
    ]
  }
}
```





### 七、插件（plugins）

插件（Plugins）是用来拓展Webpack功能的，它们会在整个构建过程中生效，执行相关的任务。
Loaders和Plugins常常被弄混，但是他们其实是完全不同的东西，可以这么来说，loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。

例1：

 如需使用某个插件，需要通过npm进行安装，然后在webpack.config.js配置文件的plugins配置项中添加该插件的实例，下面我们先来使用一个简单的版权声明插件。 

```js
// webpack.config.js
const path = require('path');
const webpack = require('webpack');  // 这个插件不需要安装，是基于webpack的，需要引入webpack模块
module.exports = {
  entry: path.join(__dirname, "/src/index.js"), // 入口文件
  output: {
    path: path.join(__dirname, "/dist"), // 打包后的文件存放的地方 
    filename: "bundle.js" // 打包后输出文件的文件名
  },
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    hot: true,
    port: '8080',
    inline: true,
    open: true,
    overlay: true,
  },
  devtool: 'source-map', // 会生成对于调试的完整的.map文件，但同时也会减慢打包速度
  module: {
    rules: [
      {
        test: /\.css$/,   // 正则匹配以.css结尾的文件
        use: ['style-loader', 'css-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      },
      {
        test: /\.(scss|sass)$/,   // 正则匹配以.scss和.sass结尾的文件
        use: ['style-loader', 'css-loader', 'sass-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      }
    ]
  },
  plugins: [
    new webpack.BannerPlugin('版权所有，翻版必究')  // new一个插件的实例 
  ]
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

运行npm run build 打包后，我们查看dist下面的handle.js文件显示如下：

![img](/webpack/版权所有)

例2： **自动生成html文件（HtmlWebpackPlugin）** 

到目前为止我们都是使用一开始建好的index.html文件，而且也是手动引入bundle.js，要是以后我们引入不止一个js文件，而且更改js文件名的话，也得手动更改index.html中的js文件名，所以能不能自动生成index.html且自动引用打包后的js呢？HtmlWebpackPlugin插件就是用来解决这个问题的

我们对项目结构进行一些更改：

1. 把整个dist文件夹删除
2. 在src文件夹下新建一个index.html(名称自定义)文件模板（当然这个是可选的，因为就算不设置模板，HtmlWebpackPlugin插件也会生成默认html文件，这里我们设置模块会让我们的开发更加灵活），如下：

```js
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <div id='root'>
    </div>
  </body>
</html>复制代码
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

安装HtmlWebpackPlugin插件

```
npm install html-webpack-plugin -D
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

引入HtmlWebpackPlugin插件，并配置了引用了我们设置的模板，如下：

```js
// webpack.config.js
const path = require('path');
const webpack = require('webpack');  // 这个插件不需要安装，是基于webpack的，需要引入webpack模块
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 引入HtmlWebpackPlugin插件
module.exports = {
  entry: path.join(__dirname, "/src/index.js"), // 入口文件
  output: {
    path: path.join(__dirname, "/dist"), // 打包后的文件存放的地方 
    filename: "bundle.js" // 打包后输出文件的文件名
  },
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    hot: true,
    port: '8080',
    inline: true,
    open: true,
    overlay: true,
  },
  devtool: 'source-map', // 会生成对于调试的完整的.map文件，但同时也会减慢打包速度
  module: {
    rules: [
      {
        test: /\.css$/,   // 正则匹配以.css结尾的文件
        use: ['style-loader', 'css-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      },
      {
        test: /\.(scss|sass)$/,   // 正则匹配以.scss和.sass结尾的文件
        use: ['style-loader', 'css-loader', 'sass-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      }
    ]
  },
  plugins: [
    new webpack.BannerPlugin('版权所有，翻版必究'),  // new一个插件的实例 
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "/src/index.html")// new一个这个插件的实例，并传入相关的参数
    })
  ]
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

运行npm run build进行打包，dist文件夹自动生成，包含index.html、bundle.js、bundle.js.map三个文件

![img](/webpack/生成dist文件夹)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

为什么会自动生成dist文件夹呢？因为我们在output出口配置项中定义了出口文件所在的位置为dist文件夹，且出口文件名为bundle.js，所以HtmlWebpackPlugin会自动帮你在 dist/index.html 中引用名为bundle.js文件，如果你在webpack.config.js文件中更改了出口文件名，dist/index.html 中也会自动更改该文件名，这样以后修改起来是不是方便多了？



例3： **清理dist文件夹（CleanWebpackPlugin）** 

webpack会生成文件，然后将这些文件放置在dist文件夹中，但是webpack无法追踪到哪些文件是实际在项目中用到的。通常，在每次构建前清理dist文件夹，是比较推荐的做法，因此只会生成用到的文件，这时候就用到CleanWebpackPlugin插件了。

```
npm install clean-webpack-plugin -D复制代码
```

```js
// webpack.config.js
const path = require('path');
const webpack = require('webpack');  // 这个插件不需要安装，是基于webpack的，需要引入webpack模块
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 引入HtmlWebpackPlugin插件
const CleanWebpackPlugin = require('clean-webpack-plugin'); // 引入CleanWebpackPlugin插件
module.exports = {
  entry: path.join(__dirname, "/src/index.js"), // 入口文件
  output: {
    path: path.join(__dirname, "/dist"), // 打包后的文件存放的地方 
    filename: "bundle.js" // 打包后输出文件的文件名
  },
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    hot: true,
    port: '8080',
    inline: true,
    open: true,
    overlay: true,
  },
  devtool: 'source-map', // 会生成对于调试的完整的.map文件，但同时也会减慢打包速度
  module: {
    rules: [
      {
        test: /\.css$/,   // 正则匹配以.css结尾的文件
        use: ['style-loader', 'css-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      },
      {
        test: /\.(scss|sass)$/,   // 正则匹配以.scss和.sass结尾的文件
        use: ['style-loader', 'css-loader', 'sass-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      }
    ]
  },
  plugins: [
    new webpack.BannerPlugin('版权所有，翻版必究'),  // new一个插件的实例 
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "/src/index.html")// new一个这个插件的实例，并传入相关的参数
    }),
    new CleanWebpackPlugin(['dist']),  // 所要清理的文件夹名称
  ]
}
```

现在我们每运行一次npm run build后就会发现，webpack会先将dist文件夹删除，然后再生产新的dist文件夹。



例4：

**热更新（HotModuleReplacementPlugin）**

**HotModuleReplacementPlugin**是一个很实用的插件，可以在我们修改代码后自动刷新预览效果。

设置方法：

1. devServer配置项中添加 hot：true 参数。
2. 因为HotModuleReplacementPlugin是webpack模块自带的，所以引入webpack后，在plugins配置项中直接使用即可。

```js
// webpack.config.js
const path = require('path');
const webpack = require('webpack');  // 这个插件不需要安装，是基于webpack的，需要引入webpack模块
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 引入HtmlWebpackPlugin插件
const CleanWebpackPlugin = require('clean-webpack-plugin'); // 引入CleanWebpackPlugin插件
module.exports = {
  entry: path.join(__dirname, "/src/index.js"), // 入口文件
  output: {
    path: path.join(__dirname, "/dist"), // 打包后的文件存放的地方 
    filename: "bundle.js" // 打包后输出文件的文件名
  },
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    hot: true,
    port: '8080',
    inline: true,
    open: true,
    overlay: true,
  },
  devtool: 'source-map', // 会生成对于调试的完整的.map文件，但同时也会减慢打包速度
  module: {
    rules: [
      {
        test: /\.css$/,   // 正则匹配以.css结尾的文件
        use: ['style-loader', 'css-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      },
      {
        test: /\.(scss|sass)$/,   // 正则匹配以.scss和.sass结尾的文件
        use: ['style-loader', 'css-loader', 'sass-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      }
    ]
  },
  plugins: [
    new webpack.BannerPlugin('版权所有，翻版必究'),  // new一个插件的实例 
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "/src/index.html")// new一个这个插件的实例，并传入相关的参数
    }),
    new CleanWebpackPlugin(['dist']),  // 所要清理的文件夹名称
    new webpack.HotModuleReplacementPlugin() // 热更新插件 
  ]
}
```

npm run dev 启动项目后，我们尝试着修改hello.js的内容，会发现浏览器预览效果会自动刷新



### 八、项目优化即扩展

webpack.config.js拆分：

1、 我们在根目录下新建三个文件，

webpack.common.js：公共配置文件

webpack.dev.js：开发环境配置文件

webpack.prod.js：生产环境（指项目上线时的环境）配置文件。

2、安装一个合并模块插件：

```
npm install webpack-merge -D
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

3、将webpack.config.js的代码拆分到上述新建的三个文件中，然后把将webpack.config.js文件删除，具体如下：

```js
// webpack.common.js
const path = require('path');
const webpack = require('webpack');  // 这个插件不需要安装，是基于webpack的，需要引入webpack模块
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 引入HtmlWebpackPlugin插件
module.exports = {
  entry: path.join(__dirname, "/src/index.js"), // 入口文件
  output: {
    path: path.join(__dirname, "/dist"), // 打包后的文件存放的地方 
    filename: "bundle.js" // 打包后输出文件的文件名
  },
  module: {
    rules: [
      {
        test: /\.css$/,   // 正则匹配以.css结尾的文件
        use: ['style-loader', 'css-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      },
      {
        test: /\.(scss|sass)$/,   // 正则匹配以.scss和.sass结尾的文件
        use: ['style-loader', 'css-loader', 'sass-loader']  // 需要用的loader，一定是这个顺序，因为调用loader是从右往左编译的
      }
    ]
  },
  plugins: [
    new webpack.BannerPlugin('版权所有，翻版必究'),  // new一个插件的实例 
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "/src/index.html")// new一个这个插件的实例，并传入相关的参数
    }),
    new webpack.HotModuleReplacementPlugin() // 热更新插件 
  ]
}

```



```js
// webpack.dev.js
const path = require('path');
const merge = require('webpack-merge');  // 引入webpack-merge功能模块
const common = require('./webpack.common.js'); // 引入webpack.common.js

module.exports = merge(common, {   // 将webpack.common.js合并到当前文件
    devServer: {
        contentBase: path.join(__dirname, "dist"),
        hot: true,
        port: '8080',
        inline: true,
        open: true,
        overlay: true,
    },
})
```



```js
// webpack.prod.js
const merge = require('webpack-merge');
const common = require('./webpack.common.js');
const { CleanWebpackPlugin } = require('clean-webpack-plugin'); // 引入CleanWebpackPlugin插件

module.exports = merge(common, { // 将webpack.common.js合并到当前文件
    devtool: 'source-map',  // 会生成对于调试的完整的.map文件，但同时也会减慢打包速度
    plugins: [
        new CleanWebpackPlugin(),  
    ]
})
```



4、设置package.json的scripts命令

```js
"scripts": {
    "build": "webpack --config webpack.prod.js",
    "dev": "webpack-dev-server --open --config webpack.dev.js"
  },
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

我们把build命令改为了webpack --config webpack.prod.js，意思是把打包配置指向webpack.prod.js配置文件，而之前我们只需要使用一个webpack 命令为什么就可以运行了？因为webpack 命令是默认指向webpack.config.js这个文件名称了，现在我们把文件名称改了，所以就需要自定义指向新的文件，dev命令中的指令也同理。

然后我们运行npm run build 和npm run dev，效果应该和我们分离代码前是一样的。

**【8.2】多入口 多出口**

到目前为止我们都是一个入口文件和一个出口文件，要是我不止一个入口文件呢？下面我们来试试：

在webpack.common.js中的entery入口有三种写法，分别为字符串、数组和对象，平时我们用得比较多的是对象，所以我们把它改为对象的写法，首先我们在src文件夹下新建index2.js文件，名称任意。因为有多个入口，所以肯定得多个出口来进行一一对应了，所以entry和output配置如下：

```js
  entry: {
        index: path.join(__dirname, "/src/index.js"),
        index2: path.join(__dirname, "/src/index2.js")
    },
    output: {
        path: path.join(__dirname, "/dist"), // 打包后的文件存放的地方 
        filename: "[name].js" // 打包后输出文件的文件名
    },
```



```js
// index2.js
function page2() {
    let element = document.createElement('div');
    element.innerHTML = '我是第二个入口文件';
    return element;
}

document.getElementById('root').appendChild(page2());
```



然后我们运行npm run build打包后发现dist文件夹下会多出index2.js文件，同时index.html也会自动将index2.js引入，然后我们运行npm run dev显示如下：

![img](/webpack/第二个入口)

