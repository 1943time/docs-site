Markdown已经流行了很多年，它简单的语法和通用的格式是被人们喜爱的关键，在这些年里业界诞生了非常多的出色的Markdown编辑器，在这里我们不再一一介绍他们。

我们先来看一下Inkdown的程序界面：

![](https://resource.inkdown.me/inkdown/h-3.png)

![](https://resource.inkdown.me/inkdown/h-2.png)

Inkdown的界面非常的简洁，因为作为Inkdown的发起者，我是一个极简主义者，特别是工具应用，应当避免过多的非必要功能和凌乱的界面。

虽然Inkdown是一个网络编辑器，但它使用了浏览器数据库作为缓存，响应速度与单机软件一致，完全感受不到延迟。现代浏览器数据库已经拥有几乎与硬盘空间一样的大小，所以我们不用担心浏览器不能存储大量的文件。

同时Inkdown通过浏览器文件系统来随时批量导入或导出您的markdown文档与图片附件，Inkdown也可以支持以标准的Markdown格式实时写入本机，让使用者永远拥有一份文档副本。著名代码编辑器vscode 的在线版也使用了chrome文件系统来与本机文件进行交互。

Inkdown的数据流转过程如下：

![](https://www.inkdown.me/images/dt1.png)

那么既然现代浏览器可以解决运行速度与文件读写的问题，是否可以直接将编辑器放在浏览器中就好了？Inkdown认为是的，借助`pwa`技术，可以将web app安装至桌面，并且Inkdown支持离线运行，当您的网络恢复后，Inkdown会自动同步内容。在实际使用中，他的使用体验与桌面软件基本一致，还免去了下载与更新的步骤。绝大部分编辑器的底层都基于html实现，直接使用浏览器也嵌套在APP中性能表现更好。

## 发布

Inkdown认为个人知识应该非常容易的与他人共享，无需借助第三方工具。Inkdown提供一键生成文档站点的功能，它是响应式的，您可以在任何设备上阅读它。就像Inkdown的使用文档那样，[Inkdown Doc](https://pb.inkdown.me/inkdown/book/docs).

通过图片来展示：

![](https://resource.inkdown.me/inkdown/p-1.png)

<img src="https://resource.inkdown.me/inkdown/pb-m1.png" alt="" height="616" />

Inkdown可以免费使用，如果您不是重度使用者，完全可以一直免费使用，Inkdown的Plus账号也很便宜。

如果您对Indkown感兴趣，可以访问[Inkdown](https://www.inkdown.me)网站，尝试一下吧。