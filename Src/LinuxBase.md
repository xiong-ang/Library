---
title: Linux基础
date: 2017-11-19 12:38:40
tags: Linux
categories: Linux
---
# Linux初步介绍                    
1. 特点：免费/开源、支持多用户、安全          
2. linux最小内存需要4M        

# 基本命令            
1. 关机: shutdown -h now         
2. 重启: shutdown -r now         
3. 重启: reboot                  
4. 进入图形界面: startx           
5. 用户注销: logout              
6. 切换身份: su                  
7. 以管理员身份允许: sudo        
8. 查看文件: ls/ls -la           
9. 显示当前路径: pwd             
10. 创建目录: mkdir             
11. 删除目录: rmdir             
12. 删除文件: rm                
13. 建立空文件: touch           
14. 复制: cp                    
15. 移动文件: mv                
16. 删除所有内容: rm -rf        
17. 建立符号连接: ln            
18. 显示文件内容，带分页: more/less     
19. 在文本中查询内容: grep              
20. 管道: |                            
21. 搜索文件及目录: find                
22. 重定向: >覆盖写；>>末尾添加；<反向传          
23. 查看文件: cat                 
24. 查看历史命令: history n              
25. 执行历史命令: !n

# vi编辑器                              
1. 创建文件: vi  文件名          
2. 三种模式:1.Comandmode-->控制屏幕光标的移动，字符或光标的删除，移动复制某区段；2.Insertmode：-->文字数据输入;3.Lastline mode-->将储存文件或离开编辑器，也可设置编辑环境[命令总结](https://www.cnblogs.com/jiayongji/p/5771444.html)                       

# Linux文件目录                    
1. 特点: 级层式的树状目录结构                   
2. 整体目录结构图                              
![](https://github.com/xiong-ang/xiong-ang.github.io/blob/Hexo/MyBlog/MyBlog/images/FileSystem.png?raw=true)                      
3. root:存放root用户相关的文件               
4. home:存放普通用户的相关文件               
5. bin: 存放普通命令的目录                   
6. sbin: 具有一定权限才能使用的命令          
7. mnt: 默认挂载软驱和光驱的目录             
8. usr: 用户软件默认安装位置                 
9. boot: 存放引导相关的文件                  
10. etc: 存放配置相关的文件                  
11. var: 存放经常变化的文件                             

# Linux用户管理                     
1. 添加用户: useradd 用户名                  
2. 设置密码: passwd 用户名                   
3. 删除用户: userdel 用户名                  
4. 删除用户及主目录: userdel -r 用户名                        

# Linux用户组和权限管理                
1. Linux中每一个用户都属于某个组                    
2. 添加组: groupadd 组名                           
3. 查看所有组文件: /etc/group                      
4. 创建用户并分配组: useradd -g 组名 用户名         
5. 查看所有用户文件: /etc/passwd                   
6. d|rwx|rwx|rwx:1.文件类型（文件-/文件夹d/链接l）；2.文件所有者对文件权限（读4/写2/可执行1）；3.文件所在组用户对文件的权限；4.其他组用户对文件的权限                     
7. 改变权限: chmod,比如chmod 777 文件名          
8. root改变用户组: usermod -g 组名 用户名        
9. 修改文件所有者: chown 用户名 文件名           
10. 修改文件所有组: chgrp 组名 文件名                               

# Linux常用技巧                  
1. 运行级别:0-->关机；1-->单用户；2-->多用户，没有网络；3-->多用户，用网络；4-->系统保留；5-->图形界面；6-->系统重启。修改：/etc/inittab - id:5:initdefault:。如果配置错误，在进入grub引导页面按“e”，再按“e”，在grub输入1进入单用户模式修改                      
2. 挂载/卸载分区文件: 1.挂载光驱: mount /mnt/cdrom/;2.卸载光驱: umount /mnt/cdrom;            
3. Linux不由后缀区分可执行文件，有x权限的文件是可执行文件，执行可执行文件:./文件名               
4. 安装压缩文件: tar -zxvf *.tar.gz             
5. 硬盘分区: 1.主分区（系统所在分区）、扩展分区（不能直接使用，可分成若干个逻辑分区）；2.一块硬盘上，主分区和扩展分区总数不超过4个；3.Windows分区标识与目录对应，Linux硬盘分区与目录结构对应不明确，各分区都是挂载在目录树上；4.查看Linux分区情况：fdisk -l；5.查看目录挂载的分区: df 目录；6.查看磁盘使用情况: df -h                        
5. Linux安装: 1.分区：/boot分区 100M；swap交换分区 一般是物理内存的两倍，不要大于256M；/根分区 尽可能大；2.swap相当于扩展内存，在系统的物理内存不够用的时候，该空间供当前运行的程序使用                                
6. Linux Shell:1.查看当前使用的shell:env;2.修改shell:chsh -s 新的shell如/bin/csh           
7. 安装.deb文件: dpkg -i 安装包名字     