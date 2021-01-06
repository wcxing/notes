#### zsh设置简短命令（alias）
1.打开.zshrc文件

```
vi ~/.zshrc
```
2.按 i 键，进入编辑模式

```
// 如下， shut_commandName 可以自定义
alias shut_commandName="open /Applications/gaotu.app --args --IsAdmin"
```
3.按esc键退出编辑模式；输入:wq 保存并退出; 若不能退出则输入 :wq! 强制退出并保存。

4.然后运行下面命令，让编辑的配置文件生效

```
source ~/.zshrc
```
