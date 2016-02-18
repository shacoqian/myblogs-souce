```ini
title = "python apache下出现The _imaging C module is not installed"
slug = "2016-02-18-16-59"
desc = "python apache下出现The _imaging C module is not installed"
date = 2013-12-12 16:59
update_date = 2013-12-12 16:59
author = 河边的老牛
author_email = 
author_url = 
tags = post
```

操作系统：win7 64位

安装python版本 win32 2.7版本

安装的PIL插件PIL-1.1.7.win32-py2.7.exe

用本地自带的开发服务器上传图片处理等一切正常

放到APACHE下上传图片出现

    The _imaging C module is not installed

 

下载Pillow-2.2.1.win32-py2.7.exe 安装后正常使用

下载地址http://www.lfd.uci.edu/~gohlke/pythonlibs/

里面有很多python的扩展

 

linux 下

 

#### 安装PIP

 

    wget http://pypi.python.org/packages/source/p/pip/pip-1.0.2.tar.gz

    tar zxf pip-1.0.2.tar.gz
    
    cd pip-1.0.2

 

先要安装`python-deve`

    yum install python-devel

    python setup.py install

    pip install pillow
