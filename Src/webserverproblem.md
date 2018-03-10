---
title: Web服务器配置问题 (Some Problems in Web Server Configuration)
date: 2017-12-25 14:02:09
tags: [Configuration]
categories: [Web]
---

## Apache                  

1. 监听端口                       
```
# 修改\conf\httpd.conf
Listen *:8888
```                                                   

2. 让外网可访问              
```                         
# 修改\conf\httpd.conf
# 将最后一个deny from all修改成allow from all
<directory />
    options followsymlinks
    allowoverride none
    order deny,allow
    deny from all
</directory>               
```