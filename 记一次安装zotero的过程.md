系统版本：Deepin v20.8

时间：2023-4-1

Deepin 商店自带很多软件，一键式安装非常方便。不过有些软件年久失更，譬如 Zotero。此时商店版本为 6.0.18，而官方版本已经到了 6.0.23（相差半年多了）。官方商店安装的版本不支持应用内自动更新，于是我又卸载掉，重新从官网上安装了。

官方版本是一个下载包 tarball，按照指示安装完毕后，出现两个问题：

1. 提示 `unable to load translators and styles`
2. 生成的`.desktop`双击没有反应

对于第一点，原因应为 zotero 的数据文件夹限制其的访问，但什么限制了它，并不清楚。网上也没有统一的解决办法。我猜是因为上一个商店版本遗留的配置项残存在数据文件夹中，索性就全部删除了数据文件夹`~/zotero`和配置文件`~/.zotero`。再重新安装一遍 tarball，问题果然消失了。

对于第二点，我折腾了好久。官方步骤中的 symlink 发送到`~/.local/share/applications/zotero.desktop`显示已损坏，直接双击原文件夹的`.desktop`也没有反应。但是直接在终端运行`./zotero`,是能够成功开启应用的。那么说明不是应用本身的问题。于是我用文本编辑器打开`.desktop`，发现其中是这样写的：

```
[Desktop Entry]
Name=Zotero
Exec=bash -c "$(dirname $(realpath $(echo %k | sed -e 's/^file:\/\///')))/zotero -url %U"
Icon=/opt/zotero/chrome/icons/default/default256.png
Type=Application
Terminal=false
Categories=Office;
MimeType=text/plain;x-scheme-handler/zotero;application/x-research-info-systems;text/x-research-info-systems;text/ris;application/x-endnote-refer;application/x-inst-for-Scientific-info;application/mods+xml;application/rdf+xml;application/x-bibtex;text/x-bibtex;application/marc;application/vnd.citationstyles.style+xml
X-GNOME-SingleWindow=true
```

好嘛，密密麻麻一大堆。注意到 exec 那一行，用了一堆很繁琐的相对路径啥的，在命令行也是能运行的，但不知为何双击运行不成功。对比从 debian 包安装的 edge-beta，来看看它的`.desktop`是怎么写的：

```
Exec=/usr/bin/microsoft-edge-beta %U
StartupNotify=true
Terminal=false
Icon=microsoft-edge-beta
Type=Application
Categories=Network;WebBrowser;
MimeType=application/pdf;application/rdf+xml;application/rss+xml;application/xhtml+xml;application/xhtml_xml;application/xml;image/gif;image/jpeg;image/png;image/webp;text/html;text/xml;x-scheme-handler/http;x-scheme-handler/https;
Actions=new-window;new-private-window;
X-Deepin-CreatedBy=com.deepin.dde.daemon.Launcher
X-Deepin-AppID=microsoft-edge-beta
```

嗯，exec 写的很简单，于是乎我干脆把 exec 改成绝对路径`bash -c /opt/zotero/zotero`这下就能直接打开了。（因为是绝对路径，就没用 symlink，直接在桌面创建了这个`.desktop`）
