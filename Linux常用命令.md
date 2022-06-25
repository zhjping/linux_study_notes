## Linux常用命令  

### 1. 基本命令格式  
- [root@localhost~]#  
    ```
    其中：
        root:       当前登陆用户
        localhost   主机名
        ～          当前所在目录（家目录）
        #           超级用户的提示符，普通用户的提示符是$

    命令 [选项] [参数]
    中括号表示可选，即有些命令不需要选项和（或）参数
    注意：个别命令使用不遵循此格式，当有多个选项时，可以写在一起
    简化选项与完整选项：-a = --all

    文件属性：-rw-r--r--(共10位)
        -文件类型（-文件 d目录 l软链接文件）
        rw-（u所有者权限）
        r--（g所属组权限）
        r--（o其他人权限）
        r读	w写	x执行
    ```

### 2. 文件处理命令  
- 显示当前目录下的文件：ls  
    ```
    ls [选项] [文件或目录]  
    选项：  
        -a	显示所有文件，包括隐藏文件  
        -l	显示详细信息（ls -l=ll）  
        -d	查看目录属性  
        -h	人性化显示文件大小  
        -i	显示inode  
    ```

- 创建目录：mkdir  
    ```
    mkdir -p [目录名]
        -p	递归创建  
        命令英文原意：make directories
    ```

- 切换目录：cd
    ```
    cd [目录]
        命令英文原意：change directory

    简化操作
        cd ~    进入当前用户的家目录
        cd      进入当前用户的家目录
        cd -    进入上次目录
        cd ..   进入上一级目录
        cd .    进入当前目录
    ```

- 查询所在目录位置：pwd
    ```
    pwd
        命令英文原意：print working directory
    ```

- 删除空目录：rmdir  
    ```
    rmdir [目录名]
        命令英文原意：remove empty directories
    ```

- 删除文件或目录：rm  
    ```
    rm -rf [文件或目录]	
        命令英文原意：remove
    选项：
        -r    删除目录
        -f    强制
    ```

- 复制命令：cp  
    ```
    cp [选项] [原文件或目录] [目标目录]
        命令英文原意：copy
    选项：
        -r    复制目录
        -p    连带文件属性复制
        -d    若源文件是链接文件，则复制链接属性
        -a    相当于-pdr
    ```

- 远程复制命令：scp  
    ```
    1. 从本地复制到远程  
        scp [选项] [本地文件或目录] [远程用户名@远程ip或域名:/目录]
        scp /root/opensource/nginx.tar.gz root@www.cumt.edu.cn:/root/opensource/

    2. 从远程复制到本地  
        scp [选项] [远程用户名@远程ip或域名:/目录] [本地文件或目录]
        scp root@www.cumt.edu.cn:/root/opensource/ /root/opensource/nginx.tar.gz
            -v    和大多数linux命令中的-v意思一样，用来显示进度，
                  可以用来查看连接，认证，或是配置错误
            -C    使能压缩选项
            -P    选择端口 . 注意 -p 已经被 rcp 使用
            -4    强行使用 IPV4 地址
            -6    强行使用 IPV6 地址 
    ```

- 修改文件权限命令：chmod
    ```
    chmod [-cfvR] [--help] [--version]  filename
        -c          若该文件权限确实已经更改，才显示其更改动作
        -f          忽略错误信息
        -v          显示权限变更的详细资料
        -R          对目前目录下的所有文件与子目录进行相同的权限变更
        --help      显示辅助说明
        --version   显示版本
    
    例：
        chmod 755 test.sh
        chmod +x test.sh
    ```

- 修改文件所有者命令：chown
    ```
    chown [-cfhvR] [--help] [--version] user[:group] filename
        -c          若该文件所有者确实已经更改，才显示其更改动作
        -f          忽略错误信息
        -h          修复符号链接
        -v          显示详细的处理信息
        -R          对目前目录下的所有文件与子目录进行相同的所有者变更
        --help      显示辅助说明
        --version   显示版本
    
    例：
        chown zhjping:zhjping test.sh
        chown -R zhjping:zhjping projects
    ```

- 剪切或改名命令：mv  
    ```
    mv [原文件或目录] [目标目录]
        命令英文原意：move
    ```
- history  
    ```
    history [选项] [历史命令保存文件]
    选项：
	    -c  清空历史命令
	    -w  把缓存中的历史命令写入历史命令保存文件/root/.bash_history

    历史命令默认保存1000条，可以在环境变量配置文件/etc/profile中进行修改

    历史命令的调用
        使用上下箭头调用以前的历史命令
        使用"!n"重复执行第n条历史命令
        使用"!!"重复执行上一条命令
        使用"!字串"重复执行最后一条以该字串开头的命令
    ```
- 硬链接特征：
    ```
    1. 拥有相同的i节点和存储block块，可以看做是同一个文件
    2. 可通过i节点识别（具有相同的i节点）
    3. 不能跨分区
    4. 不能针对目录使用
    ```

- 软链接特征：
    ```  
    1. 类似windows快捷方式
    2. 软链接拥有自己的i节点和block块，但是数据块中只保存原文件的文件名
       和i节点号，并没有实际的文件数据
    3. lrwxrwxrwx    
        l 软链接
        软链接的文件权限都为rwxrwxrwx
    4. 修改任意文件，另一个都改变
    5. 删除原文件，软链接不能使用
    ```

### 3. 文件搜索命令
- 文件搜索命令：locate  
    ```
    locate 文件名
        在后台数据库中按文件名搜索，搜索速度更快
    /var/lib/mlocate
        locate命令所搜索的后台数据库
	updatedb
        更新数据库
    注意：locate只能搜索文件名，且新建的文件不能立即被搜索到，
          可以通过updatedb更新后搜索到新建的文件
    ```

- /etc/updatedb.conf配置文件  
    ```	
    PRUNE_BIND_MOUNTS="yes"     #开启搜索限制
    PRUNEFS=                    #搜索时，不搜索的文件系统
    PRUNENAMES=                 #搜索时，不搜索的文件类型
    PRUNEPATHS=                 #搜索时，不搜索的路径
    ```

- 搜索命令的命令：whereis  
    ```
    whereis 命令名
        搜索命令所在路径和帮助文档所在位置
    选项：
        -b	只查找可执行文件
        -m	只查找帮助文件
    ```

- 搜索命令的命令：which  
    ```
    which 文件名
        搜索命令所在路径及文件名
    ```

- 文件搜索命令：find  
    ```
    find [搜索范围] [搜索条件]
        find / -name install.log
        find /root -iname install.log	#不区分大小写
        find /root -user root		#按照所有者搜索
        find /root -nouser		#查找没有所有者的文件
        find /var/log -mtime +10		#查找10天前修改的文件
            -10		10天内修改的文件
            10		10天当天修改的文件
            +10		10天前修改的文件
            atime		文件访问时间
            ctime		改变文件属性
            mtime		修改文件内容
        find 目录 -size 25k     #查找文件大小是25KB的文件
            -25     小于25KB的文件
            25      等于25KB的文件
            +25     大于25KB的文件
        find 目录 -inum 262422  #查找i节点是262422的文件
        find /etc -size +20k -a -size -50k
            查找/etc/目录下，大于20KB且小于50KB的文件
            -a	and	逻辑与，两个条件都满足
            -o	or	逻辑或，两个条件满足一个即可
        find /etc/ -size +20k -a -size -50k -exec ls -lh {} \;
            查找/etc/目录下，大于20KB且小于50KB的文件，并显示详细信息
            -exec/-ok 命令 {} \;对搜索结果执行操作
    ```

- Linux中的通配符  
    ```
    *	匹配任意内容
    ？	匹配任意一个字符
    []	匹配任意一个中括号内的字符
    ```

- 搜索字符串命令：grep  
    ```
    grep [选项] 字符串 文件名
        在文件当中匹配符合条件的字符串
    选项：
        -i	忽略大小写
        -v	排除指定字符串
    ```

### 4. 帮助命令
- man命令概述  
    ```
    man 命令
        获取命令的帮助
    man ls
        获取ls的帮助
    man的级别
        1   查看命令的帮助
        2   查看可被内核调用的函数的帮助
        3   查看函数和函数库的帮助
        4   查看特殊文件的帮助（主要是/dev/目录下的文件）
        5   查看配置文件的帮助
        6   查看游戏的帮助
        7   查看其它杂项的帮助
        8   查看系统管理员可用命令的帮助
        9   查看和内核相关文件的帮助
    ```

- 查看命令命令拥有那个级别的帮助
    ```
    man -f 命令	相当于	whatis 命令	
    举例：
        man -5 passwd
        man -4 null
        man -8 ifconfig
    ```

- 查看和命令相关的所有帮助
    ```
    man -k 命令	相当于	apropos 命令
    例如：
        apropos passwd
    ```

- 选项帮助
    ```
    命令 --help
        获取命令选项的帮助
    例如：
        ls --help
    ```

- shell内部命令帮助
    ```
    help shell内部命令
        获取shell内部命令的帮助
    例如：
        whereis cd
        确定是否是shell内部命令
        help cd
        获取内部命令帮助
    ```

- 详细命令帮助info  
    ```
    info 命令
        -回车   进入子帮助页面（带有*号标记）
        -u      进入上层页面
        -n      进入下一个帮助小节
        -p      进入上一个帮助小节
        -q      退出
    ```

### 5. 压缩和解压缩命令

- .zip压缩格式（与windows中的zip相同）  
    ```
    zip 压缩文件名 源文件
        压缩文件
    zip -r 压缩文件名 源目录
        压缩目录
    unzip 压缩文件
        解压缩.zip文件
    ```

- .gz压缩格式  
    ```
    gzip 源文件
        压缩为.gz格式的压缩文件，源文件会消失

    gzip -c 源文件 > 压缩文件
        压缩为.gz格式，源文件保留
        例如：gzip -c cangls > cangls.gz

    gzip -r 目录
        压缩目录下所有的子文件，但是不能压缩目录

    gzip -d 压缩文件
        解压缩文件

    gunzip 压缩文件
        解压缩文件
    ```

- .bz2压缩格式（不能压缩目录）
    ```
    bzip2 源文件
        压缩为.bz2格式，不保留源文件

    bzip2 -k 源文件
        压缩后保留源文件

    bzip2 -d 压缩文件
        解压缩，-k保留压缩文件

    bunzip2 压缩文件
        解压缩，-k保留压缩文件
    ```

- 打包命令tar
    ```
    tar -cvf 打包文件名 源文件
    选项：
        -c  打包
        -v  显示过程
        -f  指定打包后的文件名
    例如：
        tar -cvf longzls.tar longzls
    ```

- 解打包命令tar
    ```
    tar -xvf 打包文件名
    选项：
        -x  解打包
    例如：
        tar -xvf longzls.tar
    ```

- .tar.gz压缩格式
    ```
    其实.tar.gz格式是先打包为.tar格式，再压缩为.gz格式

    tar -zcvf 压缩包名.tar.gz 源文件
    选项：
        -z  压缩为.tar.gz

    tar -zxvf 压缩包名.tar.gz
    选项：
        -x  解压缩.tar.gz格式
    ```

- .tar.xz压缩格式
    ```
    其实.tar.xz格式是先打包为.tar格式，再压缩为.xz格式
    压缩为.tar.xz格式
        1. tar -cvf 打包文件名 源文件
        2. xz -zk 要压缩的文件
    选项：
        -k 保留压缩文件
    ```

- 解压缩.tar.xz格式
    ```
    1. xz -dk 要解压的文件
    2. tar -xvf 打包文件名
    选项：
        -k 保留压缩文件
    ```

- .tar.bz2压缩格式   
    ```
    其实.tar.bz2格式是先打包为.tar格式，再压缩为.bz2格式

    tar -jcvf 压缩包名.tar.bz2 源文件
    选项：
        -j  压缩为.tar.bz

    tar -jxvf 压缩包名.tar.bz2
    选项：
        -x  解压缩.tar.bz2格式
    ```

### 6. 关机和重起命令
- shutdown命令  
    ```
    [root@localhost ~]#shutdown [选项] 时间
    选项：
        -c  取消前一个关机命令
        -h  关机
        -r  重起
    ```

- 其他关机命令  
    ```
    [root@localhost ~]#halt
    [root@localhost ~]#poweroff
    [root@localhost ~]#init 0
    ```

- 其他重起命令  
    ```
    [root@localhost ~]#reboot
    [root@localhost ~]#init 6
    ```

- 系统运行级别  
    ```
    0	关机
    1	单用户
    2	不完全多用户，不含NFS服务
    3	完全多用户
    4	未分配
    5	图形界面
    6	重起

    [root@localhost ~]#cat /etc/inittab
    修改系统默认运行级别
        id:3:initdefault:

    查询系统运行级别
    [root@localhost ~]#runlevel     
    ```

### 7. 其它常用命令

#### 7.1 挂载命令  
- 查询与自动挂载  
    ```
    [root@localhost ~]#mount
        查询系统中已经挂载的设备	
    [root@localhost ~]#mount -a
        依据配置文件/etc/fstab的内容，自动挂载
    ```

- 挂载命令格式  
    ```
    [root@localhost ~]#mount [-t 文件系统] [-o 特殊选项] 设备文件名 挂载点
    选项：
        -t  文件系统：加入文件系统类型来指定挂载的类型，可以是ext3，ext4，
            iso9660等文件系统
        -o  特殊选项：可以指定挂载的额外选项
    ```

- 挂载光盘  
    ```
    建立挂载点
    [root@localhost ~]#mkdir /mnt/cdrom/
    
    挂载光盘
    [root@localhost ~]#mount -t ios9660 /dev/cdrom /mnt/cdrom/
    [root@localhost ~]#mount /dev/sr0 /mnt/cdrom/
    ```

- 卸载命令  
    ```
    [root@localhost ~]#umount 设备文件名或挂载点
    [root@localhost ~]#umount /mnt/cdrom/
    ```

- 挂载U盘  
    ```
    [root@localhost ~]#fdisk -l
    查看U盘设备文件名

    [root@localhost ~]#mount -t vfat /dev/sdb1 /mnt/usb/
    注意：Linux默认是不支持NTFS文件系统的
    ```
#### 7.2 登录记录查询
- 用户登陆查看和用户交互命令
    ```
    who 用户名
    命令输出：
        -用户名
        -登陆终端
        -登陆时间（登陆来源IP地址）
    ```

- 查询当前登陆和过去登陆的用户信息
    ```
    last
    last命令默认是读取/var/log/wtmp文件数据
    命令输出：
        -用户名
        -登陆终端
        -登陆IP
        -登陆时间
        -退出时间（在线时间）
    ```

- 查看所有用户的最后一次登陆时间
    ```
    lastlog
    lastlog命令默认是读取/var/log/lastlog文件内容
    命令输出：
        -用户名
        -登陆终端
        -登陆IP
        -最后一次登陆时间
    ```

- 6 Linux防火墙iptables相关操作  
    ```
    开启防火墙
    service iptables start
        
    关闭防火墙
    service iptables stop
        
    重启防火墙
    service iptables restart
        
    开放或关闭端口操作
    vi /etc/sysconfig/iptables
    文件内有修改模板，依样画葫芦即可
    ```

- 7 linux内核版本和发行版本查看(CentOS系列)  
    ```
    查看内核版本
    uname -a
        
    查看发行版本
    cat /etc/redhat-release
        
    ldd 可执行文件
    列出可执行文件的动态依赖集
        
    nm 函数库(.a文件或.so文件)
        列出函数库的符号列表
        
    size 可执行文件
        列出可执行文件各个段的大小(text,data,bbs...)
    	
    ntpdate -u ntp.api.bz
        Linux同步网络时间
    ```

- 8 查看端口占用  
    ```
    yum install net-tools
    netstat -lnp
            
    sync && echo 3 > /proc/sys/vm/drop_caches 
    && echo 0 > /proc/sys/vm/drop_caches
    ```

### 8. CentOS7.5安装配置

#### 8.1 配置网络
- 修改主机IP地址  
    ```
    打开配置文件  
        vi /etc/sysconfig/network-scripts/ifcfg-eth0
    将  
        BOOTPROTO=dhcp 改为 BOOTPROTO=static
        ONBOOT=no 改为 ONBOOT=yes
    添加  
        IPADDR=192.168.18.138
        NETMASK=255.255.255.0
        GATEWAY=192.168.18.2
        DNS1=114.114.114.114
        DNS2=8.8.8.8
    
    重启网络服务  
        systemctl restart network.service  
    ```
#### 8.2 安装软件工具
- 安装开发工具  
    ```
    yum install -y lrzsz
    yum install -y unzip
    yum install -y gdb
    yum install -y gcc
    yum install -y gcc-c++
    ```
#### 8.3 配置防火墙
- 查看防火墙服务状态  
    `systemctl status firewalld.service`  
    或 `firewall-cmd --state`

- 打开防火墙  
    `systemctl start firewalld.service`

- 关闭防火墙  
    `systemctl stop firewalld.service`

- 重启防火墙  
    `systemctl restart firewalld.service`  
    或 `firewall-cmd --reload`

- 设置防火墙开机自启动  
    `systemctl enable firewalld.service`

- 永久关闭防火墙  
    先关闭防火墙服务  
    `systemctl stop firewalld.service`  
    `systemctl disable firewalld.service`

- 查看防火墙规则  
    `firewall-cmd --list-all`

- 查看所有开放端口  
    `firewall-cmd --list-ports`

- 开启端口  
    ```
    firewall-cmd --zone=public --add-port=80/tcp --permanent

    命令含义：
        – zone              # 作用域
        –add-port=80/tcp    # 添加端口，格式为：端口/通讯协议
        –permanent          # 永久生效，没有此参数重启后失效
    ```

- 移除端口  
    `firewall-cmd  --remove-port=80/tcp --permanent`

- 允许某个IP访问（默认允许）  
    `firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address=10.0.0.19 accept'`  
    `firewall-cmd --reload`

- 禁止某个IP访问  
    `firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address=10.0.0.42 drop'`  
    `firewall-cmd --reload`

- 允许某个IP访问某个端口  
    `firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address=10.0.0.42 port protocol=tcp port=6379 accept'`  
    `firewall-cmd --reload`

- 禁止某个IP访问某个端口  
    `firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address=10.0.0.42 port protocol=tcp port=8080 reject'`  
    `firewall-cmd --reload`

- 移除以上规则  
    `firewall-cmd --permanent --remove-rich-rule='rule family="ipv4" source address="10.0.0.42" port port="8080" protocol="tcp" reject'`  
    单引号中的内容（要移除的规则）要与命令`firewall-cmd --list-all`显示的相同

#### 8.4 设置SELinux
- 查看SELinux状态  
    ```
    [root@localhost ~]# getenforce
        Enforcing       表示启动
        Permissive      表示关闭
    ```

- 临时关闭SELinux
    ```
    setenforce [ Enforcing | Permissive | 1 | 0 ] 
        Enforcing       表示启动
        Permissive      表示关闭
        1               表示启动
        0               表示关闭
    ```

- 永久关闭（修改配置文件，即可永久关闭）  
    `[root@localhost ~]# vim /etc/selinux/config`
    ```
    SELINUX=disabled
    ```
----------------------------------------------------------------------

## Linux磁盘管理

### 1. 磁盘管理
- df 查看磁盘分区使用情况  
    ```
    选项：
        -l  仅显示本地磁盘（默认）
        -a  显示所有文件系统的磁盘使用情况，包含比如/proc/
        -h  以1024进制计算，最合适的单位显示磁盘容量
        -H  以1000进制计算，最合适的单位显示磁盘容量
        -T  显示磁盘分区类型
        -t  显示指定类型文件系统的磁盘分区，参数后需要指定文件系统类型
        -x  不显示指定类型文件系统的磁盘分区，参数后需要指定文件系统类型
    ```

- du 统计磁盘上的文件大小
    ```
    选项：
        -b  以byte为单位统计文件
        -k  以KB为单位统计文件
        -m  以MB为单位统计文件
        -h  按照1024进制，以最合适的单位统计文件
        -H  按照1000进制，以最合适的单位统计文件
        -s  指定统计目标
    ```

### 2. 虚拟机添加新硬盘
- 虚拟机添加新硬盘
    ```
    关闭虚拟机，添加新硬盘
    开启虚拟机，linux系统会自动识别新添加的硬盘，但此时还不能使用，
    需要对新硬盘进行分区、格式化、挂载后才能使用。

    分区--使用分区工具fdisk
    fdisk
        列出命令的使用说明和相关参数说明

    fdisk -l
        列出系统识别的硬盘的分区表信息

    fdisk /dev/sdb
        进入设备/dev/sdb的分区模式(进入分区交互模式，之后按命令提示操作即可)
    ```
	
### 3. 分区
- 使用分区工具parted
    ```
    启动parted分区工具，默认工作磁盘为系统第一块硬盘(parted命令有交互模式和命令模式)

    select /dev/sdb
        切换工作目录，或者启动parted分区工具时直接指定：parted /dev/sdb
    
    mklabel msdos  or  mklabel gpt
        指定分区表类型

    print
    	查看当前硬盘的分区详情

    print all
        看所有硬盘的分区详情
    
    rm 分区编号
    	删除指定的分区

    unit GB
    	指定分区大小的单位

    交互模式：
        1. mkpart
        添加分区，接下来会让我们指定分区名称，非强制，可留空
	
        2. 指定文件系统类型
	
        3. 指定开始和结束位置(单位为默认MB，如0-2000，不包含2000，
        如果遇到对齐问题，可选择取消操作，重新指定分区大小范围，如1-2000)
	
    命令模式：
        1. mkpart cache 2000 3000
        cache为分区名称，2000、3000分别为起始位置和结束位置
        使用命令模式创建分区时，分区名称不能省略

    格式化
        mkfs.ext4 /dev/sdb1
        mkfs -t ext4 /dev/sdb1
	
    挂载
        手动挂载：mount /dev/sdb1 /cache
        自动挂载：编辑/etc/fstab文件，添加一下内容
        /dev/sdb1    /cache    ext4    defaults    0    0
    ```

### 4. 为linux系统添加swap分区
- 为linux系统添加swap分区
    ```
    1. 建立一个普通linux分区，主分区或逻辑分区均可
    2. 修改分区类型的16进制编码
        fdisk交互模式下按t，输入分区编号，输入分区类型的16进制编码
    3. 格式化交换分区
        mkswap /dev/sdb2
    4. 启用交换分区
        swapon /dev/sdb2
        swapoff /dev/sdb2
    ```

----------------------------------------------------------------------

## Linux用户和用户组管理

### 1. 用户和用户组  
- 用户和用户组相关信息
    ```
    用户：使用操作系统的人  
    用户组：具有相同系统权限的一组用户  

    /etc/group	存储当前系统中所有用户组信息  
        Group : X : 123 : root,ping
        组名称 : 组密码占位符 : 组编号 : 组中用户名列表
        例如：
            zhjping:x:1000:
            其中用户名列表为空是因为该⽤户组（zhjping）是这个⽤户（zhjping）
            的初始组，所以该⽤户名（zhjping）不会写⼊这个字段

    /etc/gshadow存储当前系统中用户组的密码信息
        Group: * : : root,ping
        组名称 : 组密码 : 组管理者 : 组中用户名列表
        例如：
            zhjping:!::

    /etc/passwd存储当前系统中所有用户的信息
        user : x : 123 : 456 : xxxxxx : /home/user : /bin/bash
        用户名 : 密码占位符 : 用户编号 : 用户组编号 : 用户注释信息 : 用户主目录 : shell类型
        例如：
            zhjping : x : 1000 : 1000 : : /home/zhjping/ : /bin/bash
            其中用户注释信息为空

    /etc/shadow存储当前系统中所有用户的密码信息
        user : x :::::
        用户名 : 密码占位符 :::::
    ```

- 添加用户组  
    `groupadd groupname`  
    `group -g groupnumber groupname`

- 修改用户组名  
    `groupmod -n newname oldname`

- 修改用户组号  
	`groupmod -g groupnumber groupname`

- 删除用户组（先删除用户，再删除组）  
	`groupdel 用户组名`

- 添加用户命令  
    `useradd -g groupname username`  
    -g	指定用户组  

    `useradd -d /home/xxx username`  
	没有指定用户组，默认创建与用户名相同的用户组  
    -d	指定用户个人文件夹
	
- 指定用户密码  
   `passwd username`

- 修改用户备注信息  
    `usermod -c 备注信息 username`

- 修改用户名  
    `usermod -l newusername oldusername`

- 修改用户的个人文件路径  
    `usermod -d /home/文件名 username`

- 修改用户所属的用户组  
    `usermod -g newgroupname username`
    
- 修改用户密码(root权限)  
    `passwd username`

- 删除用户  
    `userdel username`  
    没有删除用户的个人文件夹

    `userdel -r username`  
    删除用户的同时删除个人文件夹

- 禁止普通用户登陆（即只允许root用户登陆）  
	`touch /etc/nologin`

- 锁定用户的账户  
	`passwd -l username`

- 解锁用户账户  
    `passwd -u username`

- 清除用户密码  
	`passwd -d username`


### 2. 主要组与附属组
用户可以同时属于多个组：一个主要组，多个附属组

- 为用户添加附属组  
    `gpasswd -a 用户名 附属组名1, 附属组名2...`

- 切换用户组  
    `newgrp groupname`（如有组密码，回车后需要输入组密码）

- 删除用户的附属组  
    `gpasswd -d username 附属组名1, 附属组名2...`

- 创建用户的同时指定主要组和附属组  
    `useradd -g 主要组名 -G 附属组名1, 附属组名2...`

- 修改用户组密码  
    `gpasswd groupname`

- 切换用户  
    `su username`

- 显示当前登陆的用户名  
    `whoami`

- 显示指定用户信息，包括用户编号，用户名，主要组编号及名称，附属组列表  
    `id username`

- 显示username用户所在组  
    `groups username`

- 设置用户详细资料，依次输入用户资料  
    `chfn username`

- 显示用户详细资料  
    `finger username`

----------------------------------------------------------------------

## Linux软件安装
### 1.  软件包管理简介
- 软件包分类  
    ```
    源码包（脚本安装包）
    二进制包（RPM包，系统默认包）
    ```

-  源码包  
    ```
    优点：
        开源，如果有能力，可以修改源代码
        可以自由选择所需的功能
        软件是编译安装，所以更加适合自己的系统更加稳定也效率更高
        卸载方便
    缺点：
        安装过程步骤较多，尤其安装较大的软件集和时（如LAMP环境搭建）
        容易出现拼写错误
        编译过程时间较长，安装比二进制安装时间长
        因为是编译安装，安装过程中一旦出错，新手很难解决
    ```

- RPM包  
    ```
    优点：
        包的管理系统简单，只通过几个命令就可以包的安装，升级，查询和卸载
        安装速度比源码包安装快得多

    缺点：
        经过编译，不再可以看到源代码
        功能选择不如源码包灵活
        依赖性
    ```

- 脚本安装包  
    ```
    所谓的脚本安装包，就是把复杂的软件包安装过程写成了程序脚本，
    初学者可以执行程序脚本实现一键安装。但实际安装的还是源码包和二进制包。
    优点：
        安装简单快捷

    缺点：
        完全丧失了自定义性
    ```

### 2. rpm命令管理

- RPM包命名规则  
    ```
    RPM包在系统光盘中

    RPM包命名原则：
        httpd-2.2.15-15.e16.centos.1.i686.rpm
        http        软件包名
        2.2.15      软件版本
        15          软件发布的次数
        el6.centos  适合的Linux平台
        i686        适合的硬件平台
        rpm         包扩展名

    RPM包依赖性：
        树形依赖    a->b->c
        环形依赖    a->b->c->a
        模块依赖    模块依赖，查询网站：www.rpmfind.net
    ```

- 安装命令
    ```
    包全名与包名：
        包全名：操作的包是没有安装的软件包时，使用包全名。而且要注意路径
        包名：操作已经安装的软件包时，使用包名，是搜索/var/lib/rpm中的数据库

    RPM安装：
        rpm -ivh 包全名
        选项：
            -i(install)     安装
            -v(verbose)     显示详细信息
            -h(hash)        显示进度
            --nodeps        不检测依赖性
    ```

- 升级与卸载
    ```
    RPM包升级
        rpm -Uvh 包全名
        选项：
            -U(upgrade) 升级

    卸载
        rpm -e 包名
        选项：
            -e(erase)   卸载
            --nodeps    不检查依赖性
    ```

- RPM包查询
    ```
    查询是否安装
        rpm -q 包名     #查询包是否已经安装
            -q          查询（query）

        rpm -qa         #查询所有已经安装的rpm包
            -a          所有（all）

        rpm -qi 包名
        选项：
            -i          查询软件安装信息（information）
            -q          查询未安装包信息（package）

        rpm -ql 包名
        选项：
            -l          列表（list）
            -p          查询未安装包信息（package）

    RPM默认包安装位置
        /etc/           配置文件安装路径
        /usr/bin        可执行的命令安装目录
        /usr/lib        程序所使用的函数库保存位置
        /usr/share/doc/ 基本的软件使用手册保存位置
        /usr/share/man/ 帮助文件保存位置

    查询系统文件属于哪个rpm包
        rpm -qf 系统文件名
        选项：
            -f          查询系统文件属于哪个软件包（file）

    查询软件包的依赖性
        rpm -qR 包名
        选项：
            -R          查询软件包的依赖性（requires）
            -p          查询未安装包信息（package）
    ```
- RPM包校验  
    ```
    RPM包校验
        rpm -V 已安装的包名
        选项：
        	-V          校验指定RPM包中的文件（verify）
        验证内容中的8个信息的具体内容如下：
            S           文件大小是否改变
            M           文件的类型或文件的权限（rwx）是否被改变
            5           文件MD5校验和是否改变（可以看成文件内容是否改变）
            D           设备的主从代码是否改变
            L           文件路径是否改变
            U           文件的所有者是否改变
            G           文件的所属组是否改变
            T           文件的修改时间是否改变

        文件类型：
            c           配置文件（config file）
            d           普通文档（documentation）
            g           “鬼”文件（ghost file）很少见，也就是该文件不应该被这个RPM包包含
            L           授权文件（license file）
            r           描述文件（read me）

    RPM包中文件提取
        rpm2cpio 包全名 | cpio -idv .文件绝对路径
            -rpm2cpio   将rpm包转换为cpio格式命令
            -cpio       是一个标准工具，它用于创建软件档案文件和从档案文件中提取文件
    
    [root@localhost ~]#cpio 选项 < [文件|设备]
        选项：
            -i          copy-in模式，还原
            -d          还原时自动新建目录
            -v          显示还原过程  
    ```

### 3. yum在线管理  

#### 3.1 yum 源文件

- CentOS-Base.repo文件说明  
    `vi /etc/yum.repos.d/CentOS-Base.repo`  

    ```
    [base]      容器名称，一定要放在[]中
    name        容器说明，可以自己随便写
    mirrorlist  镜像站点，这个可以注释掉
    baseurl     我们的yum源服务器的地址。默认是CentOS官方的yum源服务器，
                是可以使用的，如果你觉得慢可以改成你喜欢的yum源地址
    enabled     此容器是否生效，如果不写或写成enabled=1都是生效，
                写成enabled=0就是不生效
    gpgcheck    如果是1是指RPM的数字证书生效，如果是0则不生效
    gpgkey      数字证书的公钥文件保存位置。不用修改
    ```

#### 3.2 光盘搭建本地yum源
1. 挂载光盘  
- 建立挂载点  
	`mkdir /mnt/cdrom`  

- 挂载光盘  
    `mount /dev/cdrom /mnt/cdrom/`		

2. 使网络yum源失效  
- 进入yum源目录  
    `cd /etc/yum.repos.d/`  
- 修改yum源文件，使其失效  
    `mv CentOS-Base.repo CentOS-Base.repo.bak`    
    `vim CentOS-Media.repo`
    ```
    [c6-media]  
    name=CentOS-$releasever -Media  
    baseurl=file:///mnt/cdrom	#地址为你的光盘挂载地址  
    # file:///media/cdrom  
    # file:///media/cdrecorder/	#注释这两个不存在的地址  
    gpgcheck=1
    enabled=1	#把enabled=0改为enabled=1,使yum源配置文件生效
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    ```

3. yum命令  
- 搜索  
    ```
    yum list  
    查看yum源中有哪些软件包可以安装  

    yum search keyword  
    搜索服务器上所有和关键字相关的包  
    ```

- 安装  
    ```
    yum -y install packagename
    选项：  
        install 安装  
        -y      自动回答yes  

	例如：yum -y install gcc  
    ```

- 更新  
    ```
    yum -y update packagename  
    选项：  
        update  升级  
        -y      自动回答yes  
    ```

- 卸载  
    ```
    yum -y remove packagename
    选项：  
        remove  卸载  
        -y      自动回答yes  
    ```

4. 软件组管理命令  
    `yum grouplist`  
    列出所有可用的软件组列表

    `yum groupinstall softwaregroup`  
    安装指定软件组，组名可以由grouplist查询出来

    `yum groupremove softwaregroup`  
    卸载指定软件组

----------------------------------------------------------------------
