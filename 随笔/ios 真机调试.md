### ios 真机调试(也可用于安卓)
#### spy-debugger

#### 一、简介

1.一站式页面调试工具，可以调试任何手机浏览器页面，任何手机移动端webview（如：微信，HybirdApp等）HTTP/HTTPS。

2.spy-debugger 内部集成了weinre。

#### 二、使用步骤

##### 1.安装
```
// windows
npm install spy-debugger -g

// mac
sudo npm install spy-debugger -g

```

##### 2.生成证书（支持调试https）
```
spy-debugger initCA
// 生成的证书存在于用户根目录的node-mitmproxy文件夹下
```

##### 3.安装证书
把node-mitmproxy文件夹下的node-mitmproxy.ca.crt传到手机上，安装；

**注意：**
在ios上并不能直接安装。需要在设置 -> 通用 -> 描述文件 中找到该插件，进行安装；安装后在 设置 -> 通用 -> 关于本机 -> 证书信任设置中将证书设置为受信任的。

##### 4. 设置代理 （手机和pc需要保持在同一网络下）
设置手机的HTTP代理，代理IP地址设置为PC的IP地址，端口为spy-debugger的启动端口(默认端口：9888)。

##### 5. 打开spy-debugger，用手机打开想要调试的页面
```
// 启动
spy-debugger start

// 指定端口启动
spy-debugger start -p 8888 (默认：9888)

// 是否让weinre监控iframe加载的页面(默认false)
spy-debugger start -i true

```
**原理：spy-debugger原理是集成了weinre，简化了weinre需要给每个调试的页面添加js代码。spy-debugger原理是拦截所有html页面请求注入weinre所需要的js代码。让页面调试更加方便。**

**参考链接：**

1. [一站式远程页面调试工具spy-debugger 2.0，已支持HTTPS](https://www.cnblogs.com/wuchangming/p/5473616.html)

2. [使用spy-debugger调试移动端webview](https://www.jianshu.com/p/49dbebf086a1)


