# Linux的一些资料

## Linux虚拟机与主机同一网络
+ 在VirtualBox里，设置虚拟机网络的网卡为桥接，混杂模式选择：允许虚拟机(或全部允许)；  
+ 在虚拟机的网络设置里(这里通过GUI界面设置)把ip改成手机获取，ip、子网掩码、网关 、DNS等改成与主机一致；  
+ 若主机网络发生变化时，虚拟机也要重新手动设置，然后在终端输入命令重启网络:  
```
systemctl restart network.service
```

## CentOS 7 vi编辑命令
用vi打开一个yum文件
```
vi /usr/bin/yum
```
按 i 键后  进入insert模式，进入insert模式后才能进行修改  
修改完成后按esc键进入command模式，  
然后:wq 保存文件并退出vi（注意先冒号）  
保存命令  
按ESC键 跳到命令模式，然后：
+ :w      保存文件但不退出vi
+ :w file 将修改另外保存到file中，不退出vi
+ :w!     强制保存，不推出vi
+ :wq     保存文件并退出vi
+ :wq!    强制保存文件，并退出vi
+ :q      不保存文件，退出vi
+ :q!     不保存文件，强制退出vi
+ :e!     放弃所有修改，从上次保存文件开始再编辑


## CentOS7自动以root身份登录GNOME桌面
用vim修改配置文件 /etc/gdm/custom.conf，在 [daemon] 下面添加一下两行  
```
AutomaticLoginEnable=true
AutomaticLogin=root
```

## 防火墙设置
+ 查看主机当前开启的端口
```
sudo firewall-cmd --zone=public --list-ports
```
+ 开启主机端口, 如打开tcp的8090端口
```
sudo firewall-cmd --zone=public --add-port=8090/tcp --permanent
```
+ 关闭主机端口, 如关闭tcp的8090端口
```
sudo firewall-cmd --permanent --zone=public --remove-port=8090/tcp
```
+ 立即使端口配置生效
```
sudo firewall-cmd --reload
```

## 安装chrome浏览器
+ 输入命令安装，若弹出的依赖器直接回车默认安装；
```
sudo yum install https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
```
+ centos7 root下chrome打不开问题解决方案  
打开/usr/share/applications, 右键Google Chrome，属性, 在命令一栏加入 -no-sandbox, 全文应该为:
```
/usr/bin/google-chrome-stable %U -no-sandbox
```

## 安装海峰五笔输入法
输入命令安装  
```
sudo yum install ibus-table-chinese-wubi-haifeng.noarch -y
```
安装完成后重启  
+ 到设置->Region & Language, 添加输入法， 添加五笔输入法
+ 到设置->设备->keyboard，设置输入法切换快捷键为ctrl+space

## 安装docker
要求 CentOS 系统的内核版本高于3.10, 查看版本: 
```
uname -r
```
确保 yum 包更新到最新，升级: 
```
sudo yum update
```
卸载旧版本(如果安装过旧版本的话): 
```
sudo yum remove docker  docker-common docker-selinux docker-engine
```
安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的:
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
设置yum源: 
```
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
可以查看所有仓库中所有docker版本，并选择特定版本安装: 
```
yum list docker-ce --showduplicates | sort -r
```
安装docker(这里安装的是最新稳定版17.12.0): 
```
sudo yum install docker-ce-17.12.0.ce
```
启动并加入开机启动: 
```
sudo systemctl start docker
sudo systemctl enable docker
```
验证安装是否成功: 
```
docker version
```
查看所有本地容器镜像文件: 
```
sudo docker images
```
查看容器进程: 
```
sudo docker ps -a
```
结束容器进程, 进程id为5ff68a821c5d: 
```
sudo docker stop 5ff68a821c5d
```
重新启动之前结束的容器进程, 进程id为5ff68a821c5d: 
```
sudo docker restart 5ff68a821c5d
```
结束容器服务，所有的容器进程都会结束: 
```
sudo pkill docker
```

## 安装mysql 5.7
配置YUM源: 
```
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```
安装mysql源: 
```
sudo yum localinstall mysql57-community-release-el7-8.noarch.rpm
```
安装MySQL: 
```
sudo yum install mysql-community-server
sudo yum install mysql-client
sudo yum install libmysqlclient-dev
sudo yum install mysql-community-devel.x86_64
sudo yum install libssl-dev build-essential zlibczlib-bin libidn11-dev libidn11
yum install openssl-devel
```
若要卸载mysql，则执行命令:
```
sudo rm /var/lib/mysql/ -R
sudo rm /etc/mysql/ -R
sudo yum autoremove mysql* --purge
sudo yum remove apparmor
```
启动命令: 
```
sudo systemctl start mysqld
```
获取安装时的临时密码(在第一次登录时就是用这个密码): 
```
grep 'temporary password' /var/log/mysqld.log
```
倘若没有获取临时密码则：
- 删除原来安装过的mysql残留的数据: 
```
rm -rf /var/lib/mysql
```
- 再启动mysql: 
```
systemctl start mysqld
```  
 
登录并输入临时密码，输入命令后回车，再输入密码: 
```
mysql -u root -p
```
修改密码, 设置远程访问及全表权限, 更新权限。密码要求包含有大小定字母，数字，符号。注间sql语句要与分号结束: 
```
ALTER user 'root'@'localhost' IDENTIFIED BY 'xxxxxxxx';
grant all privileges on *.* to root@'%'identified by 'xxxxxxxx';
flush privileges;
```
修改密码后，重新登录mysql。
```
exit;
```
### mysql密码错误的办法
打开配置文件
```
vim /etc/my.cnf
```
在最后一行添加"skip-grant-tables"， 按esc建，然后输入:wq，进行保存退出编辑器
重启mysql服务
```
service mysqld restart
```
登录数据库
```
mysql -u root -p   #登录数据库，出现Enter password: 这地方不用输入密码，直接回车
```
可以打开数据库表查看信息
```
use mysql; #这个语句要有分号
show tables;
desc user;
```
修改配置，使所有外部访问远程访问都使用root账号xxxxx密码访问；密码要求包含有大小定字母，数字，符号。注间sql语句要与分号结束: 
```
update user set authentication_string=password('xxxxxxx') where user='root';
```
更新配置
```
flush privileges;
```
退出数据库
```
quit;
```
重启mysql
```
service mysqld restart
```

## opensips部署-docker方式, 安装opensips容器镜像(sip音视频通讯服务器)
最新版本安装命令: 
```
sudo docker pull opensips/opensips
```
或指定版本$VERSION来安装
```
sudo docker pull opensips/opensips:$VERSION
```

查看所有本地容器镜像: 
```
sudo docker images
```

运行容器镜像，将容器镜像里的5060端口映射到主机的5060端口, /udp指定端口类型为upd, 若不写则默认tcp: 
```
sudo docker run -d -p 5060:5060/udp opensips/opensips
```

## opensips部署-下载源码编译方式
这个教程只适用centos7, mysql5.7, opensips-2.4.0版本, 其他版本按此方法可能无法部署  
前置条件先安装好mysql，配置监听端口，开启防火墙5060端口：
```
firewall-cmd --zone=public --add-port=5060/udp --permanent
firewall-cmd --reload
```
开始下载源码, 先切换到指定目录下, git下载指定版本的源码  
```
cd /usr/local/src/
git clone https://github.com/OpenSIPS/opensips.git -b2.4.0 opensips-2.4.0
```
下载完成后，切换到opensips-2.4.0, 编译, 编译完成后配置菜单  
```
cd opensips-2.4.0
make all
make menuconfig
```
在菜单中：  
进入"Configure Compile Options->Configure Excluded Modules", 选择db_mysql  
进入"Configure Compile Options->Configure Install Prefix", 配置路径 /usr/local/   
选择--->Save Changes 保存修改  
按q返回  
选择--->Compile And Install OpenSIPS  
回车安装, 安装完成后退出整个menuconfig  
安装完成后，应该生产如下的重要目录和文件, 若缺少则需要重新按步骤配置和编译安装
```
# 配置文件目录
ls /usr/local/etc/opensips/
opensips.cfg  opensipsctlrc  osipsconsolerc  scenario_callcenter.xml
# 运行程序目录
ls /usr/local/sbin
opensips  opensipsctl  opensipsdbctl  opensipsunix  osipsconfig  osipsconsole
```
修改opensipsctlrc
```
cd /usr/local/etc/opensips/
vi opensipsctlrc
```
修改成如下内容
```
# 修改后的配置
# $Id$
#
# The OpenSIPS configuration file for the control tools.
#
# Here you can set variables used in the opensipsctl and opensipsdbctl setup
# scripts. Per default all variables here are commented out, the control tools
# will use their internal default values.
## your SIP domain
SIP_DOMAIN=192.168.0.191
## chrooted directory
# $CHROOT_DIR="/path/to/chrooted/directory"
## database type: MYSQL, PGSQL, ORACLE, DB_BERKELEY, DBTEXT, or SQLITE
## by default none is loaded
# If you want to setup a database with opensipsdbctl, you must at least specify
# this parameter.
DBENGINE=MYSQL
## database port (PostgreSQL=5432 default; MYSQL=3306 default)
DBPORT=3306
## database host
DBHOST=localhost
## database name (for ORACLE this is TNS name)
DBNAME=opensips
# database path used by dbtext, db_berkeley, or sqlite
DB_PATH="/usr/local/etc/opensips/dbtext"
## database read/write user
DBRWUSER=opensips
## password for database read/write user
DBRWPW="opensipsrw"
## engine type for the MySQL/MariaDB tabels (default InnoDB)
MYSQL_ENGINE="MyISAM"
## database super user (for ORACLE this is 'scheme-creator' user)
DBROOTUSER="root"
```
这里主要是mysql连接信息，保证能正常连接即可。还有一个SIP_DOMAIN能连接到本服务的域名或者IP地址即可。  
修改opensips.cfg 
```
vi opensips.cfg
# 修改配置项
listen=udp:192.168.0.191:5060 # CUSTOMIZE ME
```
创建数据库
```
cd /usr/local/sbin
opensipsdbctl create
```
执行后是一堆的信息， 之后就是根据提示傻瓜操作创建数据库就好了。
```
nstall tables for 
    b2b
    cachedb_sql
    call_center
    carrierroute
    cpl
    domainpolicy
    emergency
    fraud_detection
    freeswitch_scripting
    imc
    registrant
    siptrace
    userblacklist
? (Y/n): y
INFO: creating extra tables into opensips ...
INFO: Extra tables successfully created.
```
这样就创建好数据库了。
启动 opensips
```
opensipsctl start
```
启动后可以查看opensips进程
```
ps -aux | grep opensips
```
注册用户格式 opensipsctl 用户名 密码，
```
opensipsctl add 1001 1001    #前面的1001是用户名，后面的1001是密码
opensipsctl add 1002 1002
```
到这里就成功的启动了服务并添加了两个用户（1001，1002）。  
 [可以点击这个教程的最后测试步骤来测试通话](https://blog.csdn.net/qq_38631503/article/details/80005454)