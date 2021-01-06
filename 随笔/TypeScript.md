### 问题
1. 引入带 ~、@ 符号的模块时没有代码提示；

- 配置 react-app-env.d.ts 文件
- 配置 tsconfig.json 文件，添加 path: {"~/*": ["src/*"]}

**注意：**有可能昨晚配置，重启项目后仍然没有代码提示。可以尝试重新打开编辑器解决。

2. 引入 classnames 等库报错；

在 typescript 项目里，引用用 ts 写的库可以直接引用，因为这些库里应有 ts 相关的声明文件。如果引用非 ts 的库，需要安装响应的 @types/<packagename> 类型声明文件。