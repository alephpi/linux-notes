系统：ubuntu v20.04 LTS

时间： 2023-4-26

4 月份入职之后，单位电脑一直都没装上 rime，还有一次甚至把 gnome 桌面搞崩了，只有鼠标能动。现在把一些坑记录下。

1. 用 fcitx 别用 fcitx5，后者和 gnome 八字不合。想用后者换 kde 桌面（奈何单位电脑我还是不搞事情了）
2. 用官网 deb 包的 vscode 别用 snap 版的，后者和 fcitx 八字不合。
3. 在下载 fcitx 之前彻底卸载 ibus 框架
4. 如下运行命令，im-config 配置自动就行

```
sudo apt install fcitx fcitx-rime
im-config  # and select fcitx as the input engine
sudo reboot # important!
fcitx-configtool # choose the correct input method you need
```

5. 最后配置 fcitx-rime，和之前笔记相同。
6. 终于能愉快打字了！
