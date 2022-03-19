---
title: "NetworkSet"
date: 2022-03-19T09:37:02+08:00
---
# Openwrt 如何下发不同的DNS和网关

修改/etc/config/network 文件
config 'interface' 'lan'            
        option 'type' 'bridge'      
        option 'ifname' 'eth0.0'    
        option 'proto' 'static'     
        option 'netmask' '255.255.255.0'
        option 'dns' '208.67.222.222'   
        option 'gateway' '192.168.3.1'  
        option 'ipaddr' '192.168.3.250'
————————————————

版权声明：本文为CSDN博主「zjqlovell」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zjqlovell/article/details/78598959
