时间：2023-5-15

方法

```
chmod 700 ~/.ssh
chmod 600 <对应文件名>
```

即可

今天突然发现 vscode 连接远程服务器不管用了。然后回到命令行，发现配置文件被`.ssh/gitpod/config`接管了（本来是`.ssh/config`）我于是把这个文件夹删了，再从命令行连接，成功。回到 vscode，还是不能连接。输出日志里面仍然是`Bad owner or permission on .ssh/gitpod/config`。回到命令行发现这个文件夹又产生了。我以为是 vscode 插件的问题，删了 remote 的插件，再试仍然没用。最后找到[这个问题](https://superuser.com/questions/1212402/bad-owner-or-permissions-on-ssh-config-file)。不知道是什么原因，之前没有出现过这个错误。有可能是因为 vscode 更新（1.78），然后把 config 文件重定向了？不知道。

5-22 更新：
破案了，原来是`.ssh/config`文件多了一行`Include "gitpod/config"`，怪哉之前怎么没发现。
