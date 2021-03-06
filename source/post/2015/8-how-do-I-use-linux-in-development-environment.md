```ini
title = "我是如何将linux用在开发环境中的"
slug = "2016-02-18-16-31"
desc = "2016-02-18-16-31"
date = 2015-08-08 16:31
update_date = 2016-02-18 16:31
author = author
author_email = 
author_url = 
tags = post
```

## 我为什么要写这篇文章


一直想深入学习一下linux的使用，于是将家里的笔记本装了linux系统，但是要将自己的系统打造一个适合开发的环境确实是一件费心费力的事，而且会经常出现一些莫名其妙的问题，以我自己的使用经验觉得要想用linux做开发环境，你要了解每个软件，不然出现一些问题就很难解决，其他不说，就是光搞好驱动问题就让人蛋疼了。

我的电脑装了linux之后一直高温不下，在网上找了一些方法，关掉独显也好不了多少，开机2个小时温度就飙到70-90度，试过很多发行版，debian,ubuntu,centos,fedora均没解决问题，至于那些gentoo之类的光看评论就吓尿了，每次都要折腾好久的驱动问题，还有些系统的源都被党国屏蔽了，如果要将驱动，开发环境，办公使用的软件折腾完真的是一件费心费力的事，而且只能使用web qq， wineqq还是那么老的版本也不好用, bclode经常登录不上去，报未知错误，由于工作的需要，我注定不适合完全linux办公环境。但是作为一个程序员linux的使用和排错能力还是要有的，于是我今天就分享我使用的方式。

## windows + 虚拟机

在虚拟机里面装linux是很普遍的，我很多同事为了学习就在虚拟机里面装了linux，没事的时候倒腾两下，但是并没有用到实际工作中去，于是我就想为何不将我们的代码，服务放到linux虚拟机里面去，然后用windows访问里面的服务，代码可以在windows下开发，部署放到linux里，我觉得这样做有以下好处：


   1.跟生产环境同步，保证自己的代码可以在linux下运行。

   2.移植方便，直接把虚拟机文件打包考到另外一台电脑上面就可以使用了。

   3.定期可以将虚拟机文件放到云盘中，等于备份了系统，随时还原 （但是文件太大上传有点麻烦，备份到硬盘倒是很方便的）

   4.因为linux用到开发中了，经常玩肯定能学到东西，这个就不用说了。

## 实现

在网上看了各种虚拟机比较，当然还是VMware比较好，而且提供了VMvare-tools，共享文件夹，共享网络都很好，共享的文件夹就挂载到了linux中，可以将开发的代码放到共享文件夹里面，这样linux就可以部署了，也不需要代码拷来考去，或者用svn更新什么的，就比较麻烦了。

我的方案是win7+centos7 因为生产环境使用的是centos，不过实际实现时有一些注意事项：

vmvare提供了3种网络共享的方式 桥接、NAT及host-only 具体有什么区别我就不说了，自己查吧。一般都是使用桥接，这样虚拟机就相当于一台独立的机器，其他机器就可以访问虚拟机里面的服务，但是我工作的公司因为每台电脑都要进行mac登记才能上网，所以我不得不选择nat方式，nat方式不需要什么设置，比较简单，但是主机无法访问虚拟机里面的服务，后来在网上找到解决的方法就是做端口映射。

上图：

![image](@media/2015/8/082335296597330.jpg)

通过本机的800端口来访问虚拟机里面的80端口，实现也很简单，只需要在虚拟机里面做一个端口映射就可以了。
在vmware里的 编辑->虚拟网络编辑器-> 选择nat模式 net设置->添加

如图：
![image](@media/2015/8/082326106432048.jpg)

然后保存就好了，如果不行还要做检查一下linux的防火墙，开放80端口。如果要让别人的电脑也能访问，还需要在windows防火墙设置一下。当然哥比较懒，因为开发的时候经常要给别人看，都是直接关闭防火墙。当然你也可以将数据库什么的都放到linux里面去，windows只装一些软件使用linux里面的服务。

但是用虚拟机如果要想不卡对电脑的配置还是要有要求的，我是8G内存，虚拟机开2G内存，开启虚拟机，IDE等工作软件基本要占用80%-90%的内存，使用还是比较流畅的，也可以将虚拟机后台运行，只使用它的服务就好了。

