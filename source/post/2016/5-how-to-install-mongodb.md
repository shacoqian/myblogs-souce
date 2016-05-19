```ini
title = "centos7 安装和配置mongodb"
slug = "2016-05-19-13-59"
desc = "2016-05-19-13-59"
date = 2016-05-19 13:59
update_date = 2016-05-19 13:59
author = 河边的老牛
author_email = 
author_url = 
tags = centos,mongodb
```

### 下载
官网下载地址：https://www.mongodb.com/download-center#community
    
    //下载
    curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.2.6.tgz

    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
    100 55.2M  100 55.2M    0     0  35665      0  0:27:05  0:27:05 --:--:-- 60124

    //解压
    tar -zxvf mongodb-linux-x86_64-3.2.6.tgz

    mongodb-linux-x86_64-3.2.6/README
    mongodb-linux-x86_64-3.2.6/THIRD-PARTY-NOTICES
    mongodb-linux-x86_64-3.2.6/MPL-2 
    mongodb-linux-x86_64-3.2.6/GNU-AGPL-3.0
    mongodb-linux-x86_64-3.2.6/bin/mongodump
    mongodb-linux-x86_64-3.2.6/bin/mongorestore
    mongodb-linux-x86_64-3.2.6/bin/mongoexport
    mongodb-linux-x86_64-3.2.6/bin/mongoimport
    mongodb-linux-x86_64-3.2.6/bin/mongostat
    mongodb-linux-x86_64-3.2.6/bin/mongotop
    mongodb-linux-x86_64-3.2.6/bin/bsondump
    mongodb-linux-x86_64-3.2.6/bin/mongofiles
    mongodb-linux-x86_64-3.2.6/bin/mongooplog
    mongodb-linux-x86_64-3.2.6/bin/mongoperf
    mongodb-linux-x86_64-3.2.6/bin/mongod
    mongodb-linux-x86_64-3.2.6/bin/mongos
    mongodb-linux-x86_64-3.2.6/bin/mongo

    //移动改名
    mv mongodb-linux-x86_64-3.2.6 mongodb

### 配置
#### 1.创建目录
    cd /usr/local/mongodb

    mkdir data
    mkdir logs

#### 2.添加配置文件
    vim mongodb.conf 
    port=27017
    logappend=true
    fork=true
    dbpath=/usr/local/mongodb/data
    logpath=/usr/local/mongodb/logs/mongodb.log
    :wq!

#### 3.不加auth验证启动
    //主要是 mongodb3.x 无默认账号 需要登录后创建admin账户 后面会讲
    ./bin/mongod -f mongodb.conf

    about to fork child process, waiting until server is ready for connections.
    forked process: 26932
    child process started successfully, parent exiting

#### 4.启动mongo shell
    ./bin/mongo

    MongoDB shell version: 3.2.6
    connecting to: test 
    Welcome to the MongoDB shell.
    For interactive help, type "help".
    For more comprehensive documentation, see
        http://docs.mongodb.org/
    Questions? Try the support group
        http://groups.google.com/group/mongodb-user

#### 5.添加管理用户
    use admin  
    db.createUser(  
        {  
            user: "admin",  
            pwd: "admin",  
            roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]  
       }  
    )  
    //查看创建的用户
    show users或者db.system.users.find()  

    {
        "_id" : "admin.admin",
        "user" : "admin",
        "db" : "admin",
        "roles" : [
                {
                        "role" : "userAdminAnyDatabase",
                        "db" : "admin"
                }
        ]
    }

#### 6.关闭mongod服务 使用--auth重新启动服务
    ps aux | grep mongo

    kill -2 xxxx

    //重新启动
    ./bin/mongod -f mongodb.conf --auth

#### 7.再次启动 mongo shell 使用auth验证登录

    ./bin/mongo

    use admin

    db.auth("admin","admin")
    //返回1  代表认证成功

#### 8.创建用户和库
    use test

    db.createUser({  
        user: "test",  
        pwd: "test",  
        roles: [  
            { role: "readWrite", db: "test" }  
        ]  
    }) 


#### 9. 使用新创建的用户登录
    use test
    db.auth("test", "test") 
    //返回1 登录成功

### 开机启动

#### 1.创建开机启动脚本
     cd /etc/init.d/

     vim mongodb

     //写入以下脚本

     #!/bin/bash

    #mongod - Startup script for mongod

    # chkconfig: 35 80 15
    # description: Mongo is a scalable, document-oriented database.
    # processname: mongod
    #config: /usr/local/mongodb/mongodb.conf
    # pidfile: /var/run/mongo/mongo.pid

    MONGOD=/usr/local/mongodb/bin/mongod

    mongod_start(){
        $MONGOD -f /usr/local/mongodb/mongodb.conf -auth
    }
    mongod_stop(){
        killall -2 mongod
    }

    case "$1" in
    start)
        echo start...
        mongod_start
        if [ $? == 0 ];then
            echo "Secuss start MongoDB!"
        fi
    ;;
    stop)
        mongod_stop
        if [ $? == 0 ];then
            echo "MongoDB is shutdown now !"
        fi
    ;;
    restart)
        mongod_stop
        mongod_start
    ;;
    *)
    echo "Use args (start|stop|restart)"
    ;;
    esac

    :wq

    //赋予执行权限
    chmod +x /etc/init.d/mongodb

    //测试一下开启和关闭mongodb
    service mongodb start
    service mongodb stop

#### 2.加入开机启动
    chkconfig mongodb on


参考资料

https://www.mongodb.com/download-center#community 

http://blog.csdn.net/hugengyong/article/details/49799447 

http://blog.csdn.net/lk10207160511/article/details/50281883 

http://my.oschina.net/fsxchen/blog/261887 



