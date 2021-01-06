#### 单独更新 npm
```
npm install npm --global
```

#### 全局安装的一般都是工具类的包

#### 查看包信息
```
npm info <package-name>
// 可以查看当前包的版本，和可用版本，最新版本等信息
```

#### package.json 文件
```
npm outdated
// 可以查看 package.json 文件中依赖可以使用的包的版本
如：
Package                      Current  Wanted   Latest  Location
@testing-library/jest-dom      4.2.4   4.2.4   5.11.1  global
@testing-library/react         9.5.0   9.5.0   10.4.7  global
// current 表示当前安装的版本号
// wanted 表示将要安装的版本号，可以安装的最新包
// 注意：需要有node_modules文件夹，就是说需要已安装过

npm view <package-name> versions
// 查看包的可用版本
```

#### 版本限制符号 ^ ~ * 代表的含义
```
把版本分为如此格式： 
主版本.大版本（次）.小版本（补丁）
major.minor.patch
// * . ^ . ~
* 表示任意版本号，会安装最新版本号；
^ 表示任意新的大版本号（次版本）包括小版本；
~ 表示任意新的小版本号（补丁版本）；
```

#### nrm
```
// 可以测试哪个源是最快的
nrm test 
```


