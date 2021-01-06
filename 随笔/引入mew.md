[mew使用文档](https://wiki.baijiahulian.com/display/BJJS/mew)

- 安装配置  eslint-plugin-mew
- 安装 mew 和 mew-loader，在 webpack 设置配置
- 配置提交代码时自动格式化代码； 配置提交代码时 message 格式校验，不符合规则不允许提交
- vscode-mew 


#### 一、安装配置 eslint-plugin-mew
##### eslint-plugin-mew 是什么？
eslint-plugin-mew 目前是作为 eslint 的**规则集**，针对不同的项目适用不同的规则集。目前包含的规则集名称有：

- base
- esnext
- vue
- react
- react-native
- typescript

**注意：使用非 base 的规则集时，可能会要求安装一些 eslint-plugin-xxx 的依赖，根据使用时的提示安装即可。**

##### eslint-plugin-mew 使用方法？
安装 eslint-plugin-mew 依赖，在 eslint 的配置文件（.eslintrc | .eslintrc.js | .eslintrc.json | .eslintrc.yml）或 package.json 字段中配置：

```
// 1. 
.eslintrc 或 .eslintrc.json
{
    "extends": ["plugin:mew/规则集名称"]
}
// 2. 
.eslintrc.js
module.exports = {
    extends: ['plugin:mew/规则集名称']
};
// 3. 
.eslintrc.yml
    extends: [plugin:mew/规则集名称]
// 4. 
package.json
    {
        "eslint": {
            "extends": ["plugin:mew/规则集名称"]
        }
    }

```

#### 二、安装 mew 和 mew-loader，在 webpack 设置配置
打包时检测代码规则

#### 三、配置提交代码时自动格式化代码 和 提交时 message 消息格式校验
##### 需要安装的依赖 husky、lint-staged、validate-commit-msg
安装完以上依赖后如下配置：
```
// package.json
{
	"scripts": {
		"commitmsg": "validate-commit-msg"
	},
	"husky": {
	   	"hooks": {
	      		"pre-commit": "lint-staged",
	      		"commit-msg": "npm run commitmsg"
	    	}
  	},
  	"lint-staged": {
	    	"*.{html,less,styl,sass,scss,js,jsx,ts,tsx}": [
	     	 	"mew fix --replace",
	      		"git add"
	    	]
  	}
}
```

**注意事项**

- 引入mew要求node版本在至少10以上。否则会报如下错误

```
Console expect ....
```

- husky 4.x 版本貌似有些问题，配置了没有起作用， 配置如下：

```
// package.json 文件
"husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "commit-msg": "npm run commitmsg"
    }
 },
 "lint-staged": {
    "*.{js}": [
      "mew fix --replace",
      "git add"
    ]
 }
```

- npm run scripts 的参数传递

```
// npm 在 2.0 版本以后，在执行脚本时可以传递自定义参数，传递方式为 npm run some-script -- args
npm 将把 -- 之后的所有参数传递给脚本
npm run dev -- arg1 arg2 ...
```