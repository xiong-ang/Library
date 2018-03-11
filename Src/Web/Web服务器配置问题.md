# Web服务器配置问题 (Some Problems in Web Server Configuration)
Date: 2017-12-25    
Author：Barret    

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



> 本文作者--Barret Xiong--    
> 转载请注明出处！