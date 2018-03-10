---
title: Docker使用（Some Problems in Using Docker）
date: 2017-12-25 14:02:20
tags: [Docker]
categories: [Cloud]
---

## 安装Docker                                     

1. 安装Docker                     
```
apt-get install docker.io
```                   

2. 查询和启动服务                            
```                               
service docker.io status
service docker.io start
```                             

3. 创建软连接                       
```           
ln -sf /usr/bin/docker.io /usr/local/bin/docker
```                                   

4. 添加Docker自启动服务                               
```            
update-rc.d docker.io defaults
```                                                    

5. 创建Docker组并添加用户                  
```                   
sudo groupadd docker
sudo gpasswd -a user_name docker
```                                       

## 更新Docker到最新版本                                   
```                   
sudo apt-get install apt-transport-https 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9 
sudo bash -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list" 
sudo apt-get update 
sudo apt-get install lxc-docker    
```                                          

## Docker常用命令                      
```                  
# 查看所有镜像
docker images

# 正在运行容器
docker ps

# 查看docker容器
docker ps -a

# 启动tomcat:7镜像
docker run -p 8080:8080 tomcat:7

# 以后台守护进程的方式启动
docker run -d tomcat:7

# 停止一个容器
docker stop b840db1d182b
docker kill b840db1d182b

# 进入一个容器
docker attach d48b21a7e439

# 进入正在运行容器并以命令行交互
docker exec -it e9410ee182bd /bin/sh

# 以交互的方式运行
docker run -i -t -p 8081:8080 tomcat:7 /bin/bash

# 退出容器
Ctrl+D 或 Exit

# 根据已有容器制作镜像
docker commit d48b21a7e439 tomcat_new
```                                     

## Ubuntu镜像apt-get update fail问题                                      
* **根源:** Docker dns server 设置问题                                 
* **解决:**               

```                
# Get dns server
nmcli dev show |grep "IP4.DNS"

# Edit /etc/default/docker and add the following line
DOCKER_OPTS="--dns <your_dns_server_1> --dns <your_dns_server_2>"

# Restart docker sevice
sudo service docker restart                
```              

## Docker制作镜像                

```                    
# 得到配置好的images ID
docker ps -a  ->得到配置好的images ID

# 提交镜像
docker commit image_ID new_image_name
```                                                    

## Docker 运行Django                    
* **根源:** Docker只能守护一个进程，如果在Docker里写脚本，脚本执行结束时，Docker守护进程也结束，所以不适合在docker容器里写脚本运行Django server                 
* **解决:**                       
```                            
# 让Docker守护进程守护Django Server进程
docker run -d -p 8080:8000 ubuntu/django /usr/bin/python3 /home/firstproject/manage.py runserver 0.0.0.0:8000
```                                        
