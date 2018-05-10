# Samba的安装和配置

Ubuntu16.04环境下搭建

---
### 流程目录

* 下载samba所需软件和软件结构
* 新建共享目录设置权限
* 配置服务器smb.conf
* 新建访问共享的用户和密码
* Windows以及ubuntu下访问共享目录

### 详细及操作
>* 1.下载samba所需软件和软件结构

1）samba：主要提供SMB服务器所需的各项服务程序（smbd以及nmbd）、的文件档、以及其他与SAMBA相关的logrotate配置文件及开机默认选项档案等；

2）samba-client：这个软件则提供了当linux作为SAMBAClient端时，所需要的工具指令，例如挂载SAMBA文件格式的mount.cifs、取得了类似网芳相关树形图的smbtree等等；

3）samba-common： 这个软件提供的则是服务器与客户端都会使用到的数据，包括SAMBA的主要配置文件（smb.conf）、语法检验指令（testparm）等等；

这三个软件都要用sudo apt-get install 下载过来。

>* 2.新建共享目录设置权限

1）终端输入 sudo mkdir /home/lh/app/share  这样share新建成功（这里是我的挂载目录）

2）输入sudo chmod 777 /home/lh/app/share 更改目录的权限 这样用户对共享目录都有写的权限

>* 3.配置服务器smb.conf

1）输入sudo vim /etc/samba/smb.comf 打开配置文件samba（最好对配置文件先备份一份）
 ![image](https://raw.githubusercontent.com/Lhisok/ItertkImage/master/res/sambaImage/conf1.png)

2）配置文件详细

配置文件内主要参数和注释如下
[global]         #全局参数。
    workgroup = MYGROUP     #工作组名称。
    server string = Samba Server Version %v     #服务器介绍信息,参数%v为显示SMB版本号。
    log file = /var/log/samba/log.%m     #定义日志文件存放位置与名称，参数%m为来访的主机名。
    max log size = 50     #定义日志文件最大容量为50Kb。
    security = user     #安全验证的方式,总共有4种。
    #share:来访主机无需验证口令，更加方便，但安全性很差。
    #user:需由SMB服务验证来访主机提供的口令后才可建立访问,更加的安全。
    #server:使用独立的远程主机验证来访主机提供的口令（集中管理帐号）。
    #domain:使用PDC来完成验证
    passdb backend = tdbsam     #定义用户后台的类型，共有3种。
    #smbpasswd:使用SMB服务的smbpasswd命令给系统用户设置SMB密码。
    #tdbsam:创建数据库文件并使用pdbedit建立SMB独立的用户。
    #ldapsam:基于LDAP服务进行帐户验证。
    load printers = yes     #设置是否当Samba服务启动时共享打印机设备。
    cups options = raw     #打印机的选项
[homes]         #共享参数
    comment = Home Directories     #描述信息
    browseable = no     #指定共享是否在“网上邻居”中可见。
    writable = yes     #定义是否可写入操作，与"read only"相反。
[printers]         #打印机共享参数
    comment = All Printers     
    path = /var/spool/samba     #共享文件的实际路径(重要)。
    browseable = no     
    guest ok = no     #是否所有人可见，等同于"public"参数。
    writable = no     
    printable = yes     

3）更改，这里介绍一种需要用户名和密码访问的配置文件更改
1.在[global]下添加
![image](https://raw.githubusercontent.com/Lhisok/ItertkImage/master/res/sambaImage/conf2.png)
（意为提供口令后才可访问）
2.添加共享名（名称不需要和所需要共享的目录相同）
![image](https://raw.githubusercontent.com/Lhisok/ItertkImage/master/res/sambaImage/conf3.png)
由上至下分别为对共享的描述；共享的目录；该共享可浏览；该共享可写
可参考上面参数注释。


>* 4.新建访问共享的用户和密码

（这里以LH为例）
sudo useradd LH
sudo smbpasswd –a LH
这里的密码需要输入两次
 ![image](https://raw.githubusercontent.com/Lhisok/ItertkImage/master/res/sambaImage/addUser.png)
这样就加入了LH这个用户
然后sudo service smbd restart 重启samba服务即可

>* 5.Windows以及ubuntu下访问共享目录

1）windows下访问虚拟机ip
 ![image](https://raw.githubusercontent.com/Lhisok/ItertkImage/master/res/sambaImage/Visit1.png)
 
可windows键+r呼出，打开要访问的ip地址
 ![image](https://raw.githubusercontent.com/Lhisok/ItertkImage/master/res/sambaImage/Visit2.png)
 
要求输入用户名和密码（将4中建立的用户输入即可）
![image](https://raw.githubusercontent.com/Lhisok/ItertkImage/master/res/sambaImage/Visit3.png)
进入文件即为访问成功！

2）ubuntu下
![image](https://raw.githubusercontent.com/Lhisok/ItertkImage/master/res/sambaImage/Visit4.png)
输入 smbclient //192.168.1.22/myshare  –U LH 
然后输入passwd 进入文件即为访问成功！





