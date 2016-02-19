```ini
title = "Centos7 搭建git服务器"
slug = "centos7-git-server"
desc = "Centos7 yum安装 搭建git服务器"
date = 2016-02-19 10:02
update_date = 2016-02-19 10:02
author = 河边的老牛
author_email = 
author_url = 
tags = centos,yum,git
```

#### 安装

    yum install -y git

#### 创建用户

    adduser git

#### 创建登录证书

    cd /home/git

    mkdir .ssh

    cd .ssh

    //生成认证文件 authorized_keys 
    //公钥id_rsa.pub
    ssh-keygen -t rsa

    //公钥写入认证文件
    //这里可以将其他客户端的公钥写入这个文件进行授权认证
    cat id_rsa.pub >> authorized_keys 

#### 初始化git仓库

    mkdir /data/git

    cd /data/git

    //创建一个裸仓库
    git init --bare empty.git

    chown -R git:git empty.git

#### 禁用shell

    vim /etc/passwd
将

    git:x:1001:1001:,,,:/home/git:/bin/bash

修改为

    git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell

#### clone

    git clone git@x.x.x.x:/data/git/emtpy.git





