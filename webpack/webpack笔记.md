## 一、webpack 介绍
### 1、为什么选择 webpack？
1. 配置灵活和插件化扩展；
2. 社区生态丰富；
3. 官方更新迭代速度快；

### 2、初识webpack：配置文件名称
- webpack 默认配置文件: webpack.config.js
- 可以通过webpack --config 指定配置文件

### 3、初识webpack：webpack配置组成
```
module.exports = {
	entry: './src/index.js',  // 1. 打包的入口文件
	output: './dist/main.js', // 2. 打包的输出
	mode: 'production',       // 3. 环境
	module: {
		reles: [               // 4. loader 配置
			{test: /\.test$/, use: 'raw-loader'}
		]
	},
	plugin: [                 // 5. 插件配置
		new HtmlwebpackPlugin({
			template: './src/index.html'
		});
	]
};

```

## 二、webpack基础用法
### 1、entry：entry用来指定打包的入口文件；
```
1. 单入口：entry是一个字符串
module.exports = {
	entry: './index.js'
};

2. 多入口：entry是一个对象
module.exports = {
	entry: {
		app: './src/app.js',
		adminApp: './src/adminApp.js'
	}
};
```
### 2、output：用来告诉webpack如何将编译后的文件输出到磁盘
```
module.exports = {
	entry: {
		app: './src/app.js',
		search: './src/search.js'
	},
	output: {
		filename: '[name].js',
		path: __dirname + '/dist'
	}
};
```
### 3、loaders: webpack开箱即用只支持js和json两种文件类型，通过loaders去支持其他文件类型，并且把它们转化成有效的模块，并且可以添加到依赖图中。loader本身是一个函数，接受源文件作为参数，返回转换的结果。

##### 1. 常见的loaders

名称 | 描述
--- | ---
babel-loader | 转换ES6、ES7等js新特性
css-loader | 支持 .css 文件的加载和解析
less-loader | 将 less 文件转换成 css
ts-loader | 将 TS 转换成 js
file-loader | 进行图片、字体等的打包
raw-loader | 将文件以字符串的形式导入
thread-loader | 多进程打包js和css

##### 2. loaders 的用法
```

```

### 4、plugins: 插件用于bundle文件的优化，资源管理和环境变量注入，作用于整个构建过程。任何无法通过loaders完成的事情可以通过plugins去完成。
##### 1. 常见的plugins
名称 | 描述
--- | ---
CommonsChunkPlugin | 将chunks相同的模块代码提取成公共js
CleanWebpackPlugin | 清理构建目录
ExtractTextWebpackPlugin | 将css从bundle文件里提取成一个独立的css文件
CopyWebpackPlugin | 将文件或者文件夹拷贝到构建的输出目录
HtmlWebpackPlugin | 创建html文件去承载输出的bundle
UglifyjsWebpackPlugin | 压缩js
ZipWebpackPlugin | 将打包出的资源生成一个zip包

##### 2. plugins 的用法
```
const path = require('path');

module.exports = {
	output: {
		filename: 'bundle.js'
	},
	plugins: [
		new HtmlWebpackPlugin({ // 放到 plugins 数组里
			template: './src/index.html'
		}),
		...
	]
};
```

### 5、mode：用来指定webpack当前的构建环境，可取值有：production、development、none；设置 mode 可以使用 webpack 内置的函数，默认值为 production。
##### 1. mode 内置函数功能
选项 | 描述
--- | ---
development | 设置 process.env.NODE_ENV 的值为 development。开启 NameChunksPlugin 和 NameModulesPlugin。
production | 设置 process.env.NODE_ENV 的值为 production。开启FlagDependencyUsagePlugin，FlagIncludedChunksPlugin，ModuleConcatenationPlugin，NoEmitOnErrorsPlugin，OccurrenceOrderPlugin，SideEffectsFlagPlugin 和 TerserPlugin。
none | 不开启任何优化选项

### 6、解析ES6和React JSX
##### 1. 解析es6：使用babel-loader，babel的配置文件是 .babelrc。
```
// 1. 安装 @babel-core, @babel/preset-env, babel-loader;
// 2. webpack.config.js 添加loader
const path = require('path');

module.exports = {
	entry: './src/index.js',
	putput: {
		filename: './bundle.js',
		path: path.resolve(__dirname, 'dist')
	},
	module: {
		rules: [
			test: /\.js$/,
			use: 'babel-loader',
			options: {
				// 可以在此处配置， 可替代 .babelrc 文件
			}
		]
	}
};
// 3. 在 .babelrc 文件添加配置
{
	"presets": [ // 一系列 plugins 的集合
		"@babel/preset-env", // es6
		
		
		
		"@babel/preset-react", // react
	],
	"plugins": [ // 一个plugin 对应一个功能
		"@babel/proposal-class-properties"
	]
}

```
##### 2. 解析react JSX
```
{
	"presets": [
		"@babel/preset-evn",
		"@babel/preset-react" // 增加 react 的 babel preset 配置
	],
	"plugins": [ // 一个plugin 对应一个功能
		"@babel/proposal-class-properties"
	]
}
```
### 7、解析 css 和 less、sass
##### 1. 解析 css 和 less 
```
// 1. css-loader 用于加载 .css 文件，并且转换成 commonjs 对象；
// 2. style-loader 将样式通过 <style> 标签插入到 head 中；
// 3. less-loader 用于将 less 转换成 css；
// 4. loaders 的执行顺序是：从右向左；前一个loader的运行结果会作为下个loader运行的参数；

const path = require('path');

module.exports = {
	entry: './src/index.js',
	putput: {
		filename: './bundle.js',
		path: path.resolve(__dirname, 'dist')
	},
	module: {
		rules: [
			{
				test: /\.js$/,
				use: 'babel-loader'
			},
			{
				test: /\.css$/,
				use: [
					'style-loader',
					'css-loader',
					'less-loader'
				]
			},
			{	
				test: /\.less/,
				use: [
					'style-loader',
					'css-loader',
					'less-loader'
				]
			}
		]
	}
};

```
### 8、解析图片和字体
##### 1. 解析图片 file-loader： 用于处理文件和字体
```
module: {
	rules: [
		{
			test: /\.(png|svg|jpg|gif)$/,
			use: [
				'file-loader'
			]
		}，
		{
			test: /\.(woff|woff2|eot|ttf|otf)$/,
			use: 'file-loader'
		}
	]
}
```
##### 2. url-loader 也可以处理图片盒字体，可以设置较小资源自动 base64 编码；
```
module: {
	test: /\.(png|jpg|gif)&/,
	use: [
		loader: 'url-loader',
		options: {
			limit: 10240
		}
	]
}
```
### 9、webpack 中的文件监听
1. 文件监听是在发现源码发生变化时，自动重新构建出新的输出文件；
2. webpack 开启文件监听模式，有两种方式
	- 启动 webpack 命令时带上参数 --watch;
	- 在配置 webpac.config.js 中设置 watch: true;
##### 1、文件监听的原理分析

轮询判断文件的最后编辑时间是否变化，某个文件发生了变化，并不会立刻告诉监听者，而是先缓存起立，等aggregateTimeout 

```
module.export = {
	// 默认false， 也就是不开启
	watch: true,
	// 只有开启监听模式时，watchOptions才有意义
	watchOptions: {
		// 默认为空，不监听的文件或者文件夹，支持正则匹配；
		ignored: /node_modules/,
		// 监听到变化发生后会等300ms再去执行，默认300ms；
		aggregateTimeout: 300,
		// 判断文件是否发生变化是通过不停询问系统指定文件有没有变化实现的，默认没每秒1000次；
		poll: 1000
	}
};
```
### 10、webpack 中使用热更新及原理分析

##### 1、热更新：使用webpack-dev-server
1. WDS 不刷新浏览器；
2. WDS 不输出文件，而是放在内存中；
3. 使用 HotModuleReplacementPlugin 插件；

##### 2、热更新：使用 webpack-dev-middleware
1. WDM 将 webpack 输出的文件传输给服务器；
2. 适用于灵活的定制场景；

```
const express = require('express');
const webpack = require('webpack');
const webpackDevMiddleware = require('webpack-dev-middleware');

const app = express();
const config = require('./webpack.config.js');
const compiler = webpack(config);

app.use(webpackDevMiddleware(compiler, {
	publicPath: config.output.publicPath
}));

app.listen(3000, function() {
	console.log('Example app listening on port 3000');
});
```

##### 3、热更新的原理分析
1. Webpack Compile：将js编译成 bundle；
2. HMR Server：将热更新的文件输出给 HMR Runtime；
3. Bundle server：提供文件在浏览器的访问；
4. HMR Runtime：会被注入到浏览器，更新文件的变化；
5. bundle.js：构建输出的文件；

![热更新](./images/热更新.jpg)

- 热更新的两个过程

1. 1 -> 2 -> A -> B: 文件系统检测到文件改变后，通过 webpack compiler 编译打包， 把打包好的文件传输给 bundle server （本职上是一个浏览器）， bundle server 可以通过服务器的方式让浏览器访问到。
2. 1 -> 2 -> 3 -> 4 -> 5: 文件系统检测到变化，通过 webpack compiler 编译打包，把打包好的文件传输给 HMR Server ，HMR Server 把改变通知在客户端的 HMR Runtime（通常是json格式数据），HMR Runtime 更新代码，无需刷新浏览器。

### 11、webpack：文件指纹
##### 1、什么是文件指纹？
打包后输出的文件名的后缀（md5），通常作为文件的版本管理；

##### 2、文件指纹如何生成？
1. hash：和整个项目的构建相关，只要项目文件有修改，整个项目构建的hash值就会更改；
2. Chunkhash：和 webpack 打包的 chunk 有关，不同的entry会生成不同的chunkhash；
3. Contenthash：根据文件内容来定义hash，文件内容不变，则contenthash不变；

##### 3、 js 的文件指纹设置
```
// 设置 output 的 filename ，使用【chunkhash】
// 不同的entry生成不同的chunk，app 页面引用的js改变不会影响到search页面引用的的chunk；
module.exports = {
	entry: {
		app: './src/app.js',
		search: './src/search.js'
	},
	output: {
		filename: '[name][chunkhash:8].js',
		path: _dirname + './dist'
	}
};

```
##### 4、 css 的文件指纹设置

```
// 设置 MiniCssExtractPlugin 的 filename，使用 【contenthash】
// 注意 MiniCssExtractPlugin 与 style-loader 是互斥的， 功能不同；style-loader 是把css通过style标签放到head标签中。MiniCssExtractPlugin.loader 是提取css到单独的文件，通过link标签引入。
module.exports = {
	entry: {
		app: './src/app.js',
		search: './src/search.js'
	},
	putput: {
		filename: '[name][chunkhash: 8].js',
		path: __dirname + '/dist'
	},
	plugins: [
		new MiniCssExtractPlugin({
			filename: '[name][contenthash: 8].css'
		});
	]
};
```

##### 5、图片或字体的文件指纹设置
```
// 设置 file-loader 的name, 使用 【hash】
const path = require('path');
module.exports = {
	entry: './src/index.js',
	output: {
		filename: 'bundle.js',
		path: path.resolve(__dirname, 'dist')
	},
	module: {
		rules: [
			test: /\.(png|svg|jpg|gif)$/,
			use: [{
				loader: 'file-loader',
				options: {
					name: 'img/[name][hash:8].[ext]'
				}
			}]
		]
	}
};
```

站位符名称 | 含义
--- | ---
[ext] | 资源后缀名
[name] | 文件名称
[path] | 文件的相对路径
[folder] | 文件所在的文件夹
[contenthash] | 文件的内容hash，默认是md5生成
[hash] | 文件内容的hash，默认是md5生成
[emoji] | 一个随机的指代文件内容的emoji

### 12、html、css、js代码压缩
##### 1、js文件的压缩：内置了 uglifyjs-webpack-plugin

##### 2、css压缩：使用 optimize-css-assets-webpack-plugin，同时使用 cssnano （css处理器）

**注意：css压缩不再使用 MiniCssExtractPlugin**

```
module.exports = {
	...
	plugins: [
		new OptimizeCSSAssetsPlugin({
			assetNameRegExp: /\.css$/g,
			cssProcessor: require('cssnano')
		})
	]
	...
};

```

##### 3、html文件的压缩：html-webpack-plugin，设置压缩参数
```
module.exports = {
	...
	plugins: [
		new HtmlWebpackPlugin({
			template: path.join(__dirname, 'src/search.html'),
			filename: 'search.html',
			chunks: ['srarch.js'],
			inject: true,
			minify: {
				html5: true,
				collapseWhitespase: true,
				preserveLineBreaks: false,
				minifyCSS: true,
				minifyJS: true,
				removeComments: false
			}
		})
	] 
	
	...
};

```
## 三、webpack 进阶用法
### 1、自动清理构建产物

每次构建的时候不会清理目录，造成构建的输出目录 output 文件越来越多。有两种方式解决此问题。

##### 1. 通过npm scripts清理构建目录；
##### 2. 通过插件清理： clean-webpack-plugin；

```
// 1.
rm -rf ./dist && webpack
或
rimraf ./dist $$ webpack

// 2. clean-webpack-plugin
module.exports = {
	...
	plugins: [
		new CleanWebpackPlugin()
	]
	...
};

```

### 2、webpack 中配置对css的增强 - 自动补全前缀
1. PostCSS 插件 autoprefixer 自动补齐CSS3前缀，根剧can i use规则；
2. PostCSS 是后处理器，打包后（生成css后）再次处理；less 和 sass 是预处理器（打包前处理）；

```
// PostCSS 配置
module.exports = {
	...
	rules: [
		test: /\.less$/,
		use: [
			MiniCssExtractPlugin.loader,
			'css-loader',
			'less-loader',
			{
				loader: 'postcss-loader',
				options: {
					plugins: () => [
						require('autoprefixer')({
							browsers: ['last 2 version', '>1%', 'ios 7']
						})
					]	
				}
			}
		]
	]
	...
};


```

### 3、webpack 中配置对css的增强 - px自动转换成rem
##### 1、rem是什么？

W3C对rem的定义： font-size of the root element。

rem和px对比：

- rem是相对单位；

- px是绝对单位；

##### 2、移动端px自动转换成rem

```
// 使用方法：
// 1. 首先，使用px2rem-loader；
// 2. 然后，页面渲染时计算根元素的font-size值，可以使用比较成熟的手淘的lib-flexble库；
module.exports = {
	module: {
		rules: [
			test: /\.less$/,
			use: [
				'style-loader',
				'css-loader',
				{
					loader: 'px2rem-loader',
					option: {
						remInit: 75, // 1rem = 75px
						remPrecision: 8 // pc转换成rem时小数点的保留位数
					}
				}
				'less-loader',
			]
		]
	}
};

// 评论区讨论
感觉现在rem这种适配方案逐渐过时了（大屏手机不是不是为了看到更大的字体，而是为了看到更多的内容），可以用vw，vh方案，加上PostCSS中的 postcss-px-viewport，在结合flex布局。

```

### 4、静态资源内联
##### 1、资源内联的意义，从两个层面来看
1. 代码层面：

	- 页面框架的初始化脚本；
	- 上报相关打点；
	- css内联避免页面闪动

2. 请求层面：减少http网络请求次数

	- 小图片（base64）或者字体内联（url-loader）

##### 2、html和js内联
```
// raw-loader 内联 html
${require('raw-loader!bable-loader!./meta.html')}

// raw-loader 内联 js, 如果内联的js 使用了es6语法，可以在raw-loader之前加上babel-loader
<script>${require('raw-loader!babel-loader!../node_modules/lib-flexble')}</script>

```

##### 3、css内联

- 方案一：借助 style-loader
- 方案二：html-inline-css-webpack-plugin

```
module.exports = {
	module: {
		rules: [
			test: /\.scss$/,
			use: [
				{
					loader: 'style-loader',
					options: {
						insertAt: 'top',	// 样式插入到 <head>标签
						singleton: true, // 将所有的style标签合并成一个
					}
				},
				'css-loader',
				'sass-loader'
			]
		]
	}
};

```
###  5、多页面应用打包通用方案
##### 1、多页应用（MPA）概念

每一次页面跳转的时候，后台服务器都会返回一个新的html文档，这种类型的网站也就是多页网站，也叫做多页应用。

- 优势
	1. 每个页面之间是解耦的；
	2. SEO友好；

- 缺点：
	1. 每次新增或删除页面都需要更改webpac配置

每个页面对应一个entry，一个 html-webpack-plugin

##### 2、多页面打包通用解决方案

利用 glob.sync

```
// entry: glob.sync(path.join(__dirname, './src/*/index.js'))

```

### 6、使用sourcemap
作用：通过sourcemap定位到源代码；

开发环境开启，线上环境关闭；

线上排查问题时可以将sourcemap上传到错误监控系统。

**source map 关键字**

关键字 | 含义
--- | ---
eval | 使用eval包裹模块代码
source map | 产生 .map 文件
cheap | 不包含列信息
inline | 将 .map 作为 DataURI 嵌入，不单独生成 .map 文件
module | 包含 loader 的 sourcemap

### 7、提取页面公共资源
##### 1、基础库分离
思路：将 react、react-dom 基础包通过cdn引入，不打入bundle中；

方法：使用html-webpack-externals-plugin

```
const HtmlWebpackExternalsPlugin = require('html-webpack-externals-plugin');

...
plugins: [
	new HtmlWebpackExternalsPlugin({
		externals: [
			{
				module: 'react',
				entry: '//11.url.cn/now/lib/15.1.0/react-with-addons.min.js?_bid=3123',
				global: 'React'
			},
			{
				module: 'react-dom',
				entry: '//11.url.cn/now/lib/15.1.0/react-dom.min.js?_bid=3123',
				global: 'ReactDOM'
			}
		]
	});
]
...

```

##### 2、利用 splitChunksPlugin 进行公共脚本分离

webpack4 内置的，替代 CommonsChunkPlugin 插件；

chunks参数说明：

1. async： 异步引入的库进行分离(同步引入的库会被忽略)；
2. inital： 同步引入的库进行分离（异步引入的库会被忽略）；
3. all：所有引入的库进行分离（推荐）；

```
module.exports = {
	optimization: {
		splitChunks: {
			chunks: 'async',
			minSize: 30000,  // 分离的包体积的大小
			maxSize: 0,
			minChunks: 1, // 设置最小引用次数
			maxAsyncRequests: 5,
			maxInitialRequests: 3,
			automaticNameDelimiter: '~',
			name: true,
			cacheGroups: {
				vendors: {
					test: /[\\/]node_modules[\\/]/, // 匹配打包规则
					priority: -10
				}
			}
		}
	}
};

```














