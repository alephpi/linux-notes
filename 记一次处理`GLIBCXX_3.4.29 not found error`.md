系统：ubuntu 20.04

时间：2023-4-13

今天在实习的服务器上尝试安装 spacy 的一个分词模型。安装过程中提示 Import Error: /usr/lib/x86_64-linux-gnu/libstdc++.so.6 GLIBCXX_3.4.29 not found error。一般来说如果你有管理员权限，[直接 apt update | apt upgrade 即可](https://stackoverflow.com/questions/65349875/where-can-i-find-glibcxx-3-4-29)
但我没有管理员权限，再加上我在 python 虚拟环境中工作，完全可以在虚拟环境中配置。这里我使用的是 micromamba（和 conda 大同小异）
先说解决办法：

1. 首先来到 `~/micormamba/lib`（这里替换为你的虚拟环境安装目录），看看 GLIBCXX 的版本

```
strings libstdc++.so.6 | grep GLIBCXX
```

2. 如果有 GLIBCXX 3.4.29，则转到 3，否则 4
3. 运行
   `export LD_LIBRARY_PATH=~/micromamba/lib` (也可直接加入`~/.bashrc`，自动添加路径。)
4. 如果没有 GLIBCXX 3.4.29，运行
   `ls libstdc++.so.6* -l`看看.so.6 符号链接到的版本是什么
   我的是链接到了 6.0.30 没问题。如果太老的话，micromamba update 再 upgrade 一下（micromamba 在这里的地位就和 apt 一样），完了转到 3。

下面简单解释一下原因。默认情况下，bash 是在/usr/lib 中寻找 libstdc++.so.6。然而这个文件夹下的 libstdc++.so.6 可能版本较老（譬如我公司服务器对应的是 6.0.25）因而报错。
而我使用 micromamba 管理虚拟环境，查看~/micormamba/lib 可以发现其中有 3.4.29 的版本（第 1、2 步），只需把路径添加到.bashrc 即可（第 3 步）
