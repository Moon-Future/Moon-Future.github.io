---
title: veu+node个人博客从零开始到部署上线（部署上线）
date: 2017-11-11 00:52:53
tags:
---

# 云服务器

## 阿里云 or 腾讯云
- 阿里云服务器品牌：ECS（Elastic Compute Service）
- 腾讯云服务器品牌：VCM（Cloud Virtual Machine）

![腾讯云or阿里云](http://otr9a8wg0.bkt.clouddn.com/%E9%98%BF%E9%87%8C%E4%BA%91or%E8%85%BE%E8%AE%AF%E4%BA%91.jpg)

两者都可以，具体可以根据自己的需求，都说阿里云稳定，腾讯云便宜，我自己买时发现两者入门级的价格都差不多，就买了阿里云的，以下即以阿里云的服务器操作。（腾讯云服务器操作应该也类似）

## 购买阿里云服务器ECS
入门级最低配即可，一年300多，每月几十块钱，也可以月付，那样就贵点。
![](http://otr9a8wg0.bkt.clouddn.com/%E8%B4%AD%E4%B9%B0%E9%98%BF%E9%87%8C%E4%BA%91ECS.jpg)
中间有些选项默认就可，镜像选择 公共镜像-CentOS-7.4 64位（最新的）
图中密码用来之后远程登陆服务器使用。

## 登陆服务器
### 阿里网页登陆
在 管理控制台-实例 中可以看到刚刚购买的服务器
![](http://otr9a8wg0.bkt.clouddn.com/%E7%BD%91%E9%A1%B5%E8%BF%9E%E6%8E%A5%E9%98%BF%E9%87%8C%E4%BA%91ECS.jpg)
点击远程连接，出现登陆界面，第一次进入会弹出一个密码，记住这个密码（只会出现一次），之后登陆输入这个密码即可进入阿里云服务器ECS系统。

### 客户端工具远程登陆
1. Mac 终端中输入：
```
SSH root@服务器IP地址(公)     // 如：(SSH root@192.18.222.12)
```
 回车 输入购买服务器时设置的实例密码即可
2. Windows
- 下载工具 Xshell
- 打开Xshell - 文件 - 新建
![](http://otr9a8wg0.bkt.clouddn.com/Xshell%E8%BF%9E%E6%8E%A5%E6%9C%8D%E5%8A%A1%E5%99%A8.jpg)
- 连接成功
![](http://otr9a8wg0.bkt.clouddn.com/Xshell%E8%BF%9E%E6%8E%A5%E6%88%90%E5%8A%9F.jpg)


# 配置环境
Linux 常用命令：
1. wget：一个从网络上自动下载文件的自由工具，支持通过 HTTP、HTTPS、FTP 三个最常见的 TCP/IP协议 下载，并可以使用 HTTP 代理。"wget" 这个名称来源于 “World Wide Web” 与 “get” 的结合。
2. tar：压缩解压命令
    - -c：建立压缩档案
    - -x：解压
    - -t：查看内容
    - -r：向压缩归档文件末尾追加文件
    - -u：更新原压缩包中的文件
这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。下面的参数是根据需要在压缩或解压档案时可选的。
    - -z：有gzip属性的
    - -j：有bz2属性的
    - -Z：有compress属性的
    - -v：显示所有过程
    - -O：将文件解开到标准输出
    下面的参数 -f 是必须的
    - -f：使用档案名称，最后一个参数，后面只能接档案名
3. ln：为某一个文件或目录在另一个位置建立一个同步的链接 常用：
```
ln -s 源文件 目标文件
```
4. makdir：创建目录
5. mv：为文件或目录改名、或将文件或目录移入其它位置
6. rm：删除文件
    - -f：忽略不存在的文件，从不给出提示
    - -r：将参数中列出的全部目录和子目录均递归的删除
7. yum：提供了查找、安装、删除某一个、一组甚至全部软件包的命令
8. ls：显示当前目录下文件， ls -f 隐藏文件也显示
9. netstat -tpln：查看进程端口
10. kill -9 PID号：关闭进程
11. cp：拷贝

Linux 目录：
前面进入Linux系统后，一般会在 root(~) 目录下
```
[root@xxxxxxxxxxx ~]#
cd .. // 可以即回到根目录
ls    // 查看当前目录下文件


[root@xxxxxxxxxxx ~]#
[root@xxxxxxxxxxx ~]# cd ..
[root@xxxxxxxxxxx /]#
[root@xxxxxxxxxxx /]# ls
bin  boot  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@xxxxxxxxxxx /]# cd root
[root@xxxxxxxxxxx ~]#
```
## 安装NodeJs
[阿里云帮助文档：部署Node.js项目（CentOS）](https://help.aliyun.com/document_detail/50775.html)
## 安装MySQL
[主要参考](http://blog.csdn.net/zhou920786312/article/details/77750604)
#### 1. 下载安装包
为了下载到最新的版本，先到官网上找到下载链接
[MySQL下载地址](https://dev.mysql.com/downloads/mysql/)
![](http://otr9a8wg0.bkt.clouddn.com/MySQL%E4%B8%8B%E8%BD%BD%E5%9C%B0%E5%9D%80.jpg)
先用浏览器或其他下载工具创建下载任务（如x86,64-bit），然后在下载中找到下载链接复制下来就可以把它删了。
- 进入root目录：(也可以其他目录)
```
cd /root 
```
- 下载安装包：
```
wget https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.20-linux-glibc2.12-x86_64.tar.gz
```
- 下载完成后 ls 可以看到下载的安装包
```
[root@xxxxxxxxxxx ~]# ls
mysql-5.7.20-linux-glibc2.12-x86_64.tar.gz ......
```

#### 2. 解压文件
```
tar -xzvf mysql-5.7.19-linux-glibc2.12-x86_64.tar.gz -C /usr/local/

[root@xxxxxxxxxxx ~]# ls
mysql-5.7.20-linux-glibc2.12-x86_64 (解压得到的目录)
mysql-5.7.20-linux-glibc2.12-x86_64.tar.gz

// 拷贝解压到目录到 /usr/local 目录下，并改名为 mysql
[root@xxxxxxxxxxx ~]# cp mysql-5.7.20-linux-glibc2.12-x86_64 /usr/local/mysql -r
[root@xxxxxxxxxxx ~]# cd /usr/local/mysql
[root@xxxxxxxxxxx mysql]# ls
bin  COPYING  docs  include  lib  man  README  share  support-files
```

#### 3. 添加系统mysql组和mysql用户
```
[root@xxxxxxxxxxx ~]# groupadd mysql #建立一个mysql的组
[root@xxxxxxxxxxx ~]# useradd -r -g mysql mysql #建立mysql用户，并且把用户放到mysql组
```

#### 4. 在 mysql 下添加 data 目录
```
[root@xxxxxxxxxxx mysql]# mkdir data
```

#### 5. 更改mysql目录下所有的目录及文件夹所属组合用户
```
[root@xxxxxxxxxxx mysql]# cd /usr/local/
[root@xxxxxxxxxxx local]# chown -R mysql mysql/
[root@xxxxxxxxxxx local]# chgrp -R mysql mysql/
[root@xxxxxxxxxxx local]# cd mysql/
[root@xxxxxxxxxxx mysql]# ls -l
total 56
drwxr-xr-x  2 mysql mysql  4096 Nov  9 16:00 bin
-rw-r--r--  1 mysql mysql 17987 Nov  9 16:00 COPYING
drwxr-xr-x  6 mysql mysql  4096 Nov  9 18:41 data
drwxr-xr-x  2 mysql mysql  4096 Nov  9 16:00 docs
drwxr-xr-x  3 mysql mysql  4096 Nov  9 16:01 include
drwxr-xr-x  5 mysql mysql  4096 Nov  9 16:01 lib
drwxr-xr-x  4 mysql mysql  4096 Nov  9 16:00 man
-rw-r--r--  1 mysql mysql  2478 Nov  9 16:00 README
drwxr-xr-x 28 mysql mysql  4096 Nov  9 16:00 share
drwxr-xr-x  2 mysql mysql  4096 Nov  9 18:06 support-files
```

#### 6. 安装和初始化数据库
很多老的教程中都是运行 
```
./scripts/mysql_install_db --user=mysql
```
进行安装，但在新版本的mysql中已经没了 scripts 目录，mysql_install_db 放在了 bin 目录下
```
[root@xxxxxxxxxxx mysql]# cd bin
[root@xxxxxxxxxxx bin]# ./mysqld --initialize --user=mysql --basedir=/usr/local/mysql/--datadir=/usr/local/mysql/data/


2017-11-09T09:09:52.826209Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2017-11-09T09:09:54.885578Z 0 [ERROR] Can't find error-message file '/usr/local/mysql/--datadir=/usr/local/mysql/data/share/errmsg.sys'. Check error-message file location and 'lc-messages-dir' con
figuration directive.2017-08-31T08:50:24.709286Z 0 [Warning] InnoDB: New log files created, LSN=45790
2017-11-09T09:09:55.105938Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2017-11-09T09:09:55.218562Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: c0844cc4-c52d-11e7-b74f-00163e0ae84e.
2017-11-09T09:09:55.221300Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2017-11-09T09:09:55.221784Z 1 [Note] A temporary password is generated for root@localhost: uf)qP3+C?jpJ
```
解决：（无视警告）
```
[root@xxxxxxxxxxx bin]# ./mysqld --initialize --user=mysql --basedir=/usr/local/mysql/ --datadir=/usr/local/mysql/data/ --lc_messages_dir=/usr/local/mysql/share --lc_messages=en_US
```

#### 7. 配置my.cnf
进入 /usr/local/mysql/support-files/ 目录下，查看是否存在my-default.cnf 文件，如果存在直接 copy 到 /etc/my.cnf 文件中
```
[root@xxxxxxxxxxx mysql]# cp -a ./support-files/my-default.cnf /etc/my.cnf
```
如果不存在 my-default.cnf 文件, 则在 /etc/ 目录下创建 my.cnf
```
[root@xxxxxxxxxxx bin]# cd /etc
[root@xxxxxxxxxxx etc]# vim my.cnf
```
写入内容
```
#[mysql]
#basedir=/usr/local/mysql/
#datadir=/usr/local/mysql/data/
```

#### 8. 启动服务
```
[root@xxxxxxxxxxx mysql]# cd bin/
[root@xxxxxxxxxxx bin]# ./mysqld_safe --user=mysql &
```

#### 9. 将mysqld服务加入开机自启动项
```
[root@xxxxxxxxxxx bin]# cd ../support-files
[root@xxxxxxxxxxx support-files]# cp mysql.server /etc/init.d/mysql
[root@xxxxxxxxxxx support-files]# chmod +x /etc/init.d/mysql
-- 把mysql注册为开机启动的服务
[root@xxxxxxxxxxx support-files]# chkconfig --add mysql
```

#### 10. 启动服务
```
[root@xxxxxxxxxxx bin]# service mysql start
```
若报错 ERROR! The server quit without updating PID file
```
[root@xxxxxxxxxxx mysql]# rm  /etc/my.cnf
rm: remove regular file '/etc/my.cnf'? y
[root@xxxxxxxxxxx mysql]# /etc/init.d/mysql start
Starting MySQL.Logging to '/usr/local/mysql/data/dbserver.err'.
 SUCCESS!
[root@xxxxxxxxxxx mysql]# service mysql start
Starting MySQL SUCCESS!
```

#### 11. 登录mysql
```
[root@xxxxxxxxxxx bin]# ./mysql -u root -p
密码是第6步产生的密码
```
如果出现错误：
```
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
```
重改密码
```
[root@xxxxxxxxxxx bin]# /etc/init.d/mysql stop
[root@xxxxxxxxxxx bin]# mysqld_safe --user=mysql --skip-grant-tables --skip-networking &
[root@xxxxxxxxxxx bin]# mysql -u root mysql
mysql> UPDATE user SET Password=PASSWORD('newpassword') where USER='root';

// 上面语句若出错，换为
update mysql.user set authentication_string=password('newpassword') where user='root'

mysql> FLUSH PRIVILEGES;
mysql> quit

[root@xxxxxxxxxxx bin]# /etc/init.d/mysqld restart
[root@xxxxxxxxxxx bin]# mysql -uroot -p
Enter password:

mysql>
```

#### 12. 设置远程登录权限
```
mysql>  grant all privileges on *.* to'root' @'%' identified by 'root';
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.06 sec)

mysql> quit
Bye
```

#### 13. 进程关闭
若以上步骤中出现其他错误，可以看看 mysql 是否关闭了，先关闭端口，然后在试试
```
[root@xxxxxxxxxxx ~]# netstat -tpln
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1105/sshd
tcp6       0      0 :::3306                 :::*                    LISTEN      25599/mysqld
[root@xxxxxxxxxxx ~]# kill -9 25599
```

#### 14. 本地连接数据库

我本地使用的是 Navicat for MySQL
![远程连接数据库](http://otr9a8wg0.bkt.clouddn.com/%E8%BF%9C%E7%A8%8B%E8%BF%9E%E6%8E%A5%E6%95%B0%E6%8D%AE%E5%BA%93.jpg)
