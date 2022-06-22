#### 1. 修改主机IP地址
打开配置文件  
`vi /etc/sysconfig/network-scripts/ifcfg-eth0`  

将  
`BOOTPROTO=dhcp` 改为 `BOOTPROTO=static`  
`ONBOOT=no` 改为 `ONBOOT=yes`  

添加  
`IPADDR=192.168.18.138`  
`NETMASK=255.255.255.0`  
`GATEWAY=192.168.18.2`  
`DNS1=114.114.114.114`  
`DNS2=8.8.8.8`  

重启网络服务  
`systemctl restart network.service`  

--------------------------------------------------

#### 2. 