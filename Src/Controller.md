---
title: 继电器-->Pico控制器-->可编程控制器PLC
date: 2017-11-05 12:35:45
tags: Controller
categories: Product
---
# 继电器    

  继电器（英文名称：relay）是一种电控制器件，是当输入量（激励量）的变化达到规定要求时，在电气输出电路中使被控量发生预定的阶跃变化的一种电器。  
  它具有控制系统（又称输入回路）和被控制系统（又称输出回路）之间的互动关系。    
  通常应用于自动化的控制电路中，它实际上是用小电流去控制大电流运作的一种自动开关。故在电路中起着自动调节、安全保护、转换电路等作用。     
  
# Pico控制器     
  Pico是一种紧凑、友好、廉价的控制器，提供简单的逻辑、定时、计数和实时时钟操作。    
  为增强性能，Pico GFX增加了图形画面的使用,提供高级编程特征:如PID控制,高速计数器，以及位序列。     
  Pico是替代继电器应用的理想选择，适合于简单控制应用，如楼宇、暖通空调、停车场照明以及一些对成本要求很严的场合。     
  Pico控制器所有的编程和数据调整都能够通过面板上的键盘和显示来完成，或者利用Allen-Bradley的PicoSoft和PicoSoft Pro配置软件来完成。     
  
# 可编程控制器PLC    
  可编程控制器（Programmable Logic Controller）简称PLC，是一种通用的工业控制装置，其组成与一般的微机系统基本相似。     
  它采用可以编制程序的存储器，用来在执行存储逻辑运算和顺序控制、定时、计数和算术运算等操作的指令，并通过数字或模拟的输入(I)和输出(O)接口，控制各种类型的机械设备或生产过程。PLC基本组成如下图所示：      
  ![](https://github.com/xiong-ang/xiong-ang.github.io/blob/Hexo/MyBlog/MyBlog/images/PLC_base.PNG?raw=true)      
  可编程控制器是在电器控制技术和计算机技术的基础上开发出来的，并逐渐发展成为以微处理器为核心，把自动化技术、计算机技术、通讯技术融为一体的新型工业控制装置。PLC工作过程如下图所示：       
  ![](https://github.com/xiong-ang/xiong-ang.github.io/blob/Hexo/MyBlog/MyBlog/images/PLC_Work.PNG?raw=true)     
  目前，PLC已被广泛应用于各种生产机械和生产过程的自动控制中，成为一种最重要、最普及、应用场合最多的工业控制装置，被公认为现代工业自动化的三大支柱（PLC、机器人、CAD/CAM）之一。    
# Rockwell系列PLC               

## Logix系统控制器类型         
* ControlLogix控制器            
* CompactLogix控制器              
* FlexLogix控制器            
* SoftLogix控制器          
* DriveLogix控制器                

## Logix系统常见硬件类型           
* 机架	  
（1）1756-A4/B、1756-A7/B、1756-A10/B、1756-A13/B、1756-A17/B     
（2）4槽、7槽、10槽、13槽、17槽机架        
* 电源模块	     
（1）非冗余（1756-PA72，1756-PB72，1756-PA75，1756-PB75）      
冗余（1756-PA75R，1756- PB75R）       
（2）1756-PA72 电源（220VAC/5VDC 10A）      
     1756-PA75 电源（220VAC/5VDC 13A）       
     1756-PB72 电源（24VDC/5VDC 10A）         
     1756-PB75 电源（24VDC/5VDC 13A）         
* CPU模块	       
（1）1756-L55系列，1756-L55M12、1756-L55M16、1756-L55M22、1756-L55M24            
（2）1756-L6x系列，1756-L61、1756-L62、1756-L63、1756-L65           
（3）1756-L7x系列，1756-L71、1756-L72、1756-L73、1756-L75               
* 通讯模块	       
EtherNet通讯模块，1756-ENBT、1756-EN2T、1756-EN2TR             
  ControlNet通讯模块，1756-CNB、1756-CNBR、1756-CN2、1756-CN2R            
  DeviceNet通讯模块，1756-DNB             
* DI模块	           
（1）1756-IB16，1756-IB32（12/24V DC）              
（2）1756-IM16I（220V AC）                 
* DO模块	                   
（1）1756-OB8、1756-OB16、1756-OB32（12/24V DC）
* AI模块                  
  常规，1756-IF8、1756-IF16             
  热电阻模块，1756-IR6I               
  热电偶模块，1756-IT6I              
* AO模块	           
（1）1756-OF4、1756-OF8、1756-IF4FXOF2F              
* 其他	          
（1）1756-HSC，高速计数器模块

## ControlLogix硬件特性                    
* 框架：      1756-A7/B（本地7槽机架）、1756-A10/B（远程10槽机架）          
* 控制器型号：1756-L****                       
* 电源模块：	1756-PA72/C，120/240VAC，50/60Hz                 
* 通讯模块：  1756-ENBT，1756-CN2R,1756-CNBR,1756-DNB          
* I/O模块：	  
1756-IR6I，    6通道，热电阻模块             
1756-IF8，	    8通道，模拟量输入模块          
1756-OF8，    8通道，模拟量输出模块               
1756-IT6I，	    6通道，热电偶模块             
1756-IB16，    16点，数字量输入模块（DC24V）             
1756-OB16E，  16点，数字量输出模块（DC24V）              
![](https://github.com/xiong-ang/xiong-ang.github.io/blob/Hexo/MyBlog/MyBlog/images/AIO.PNG?raw=true)     
![](https://github.com/xiong-ang/xiong-ang.github.io/blob/Hexo/MyBlog/MyBlog/images/DIO.PNG?raw=true)    
* 通讯方式：RS232/DH-485串口通讯、EtherNet网络通讯         
* DC24V电源：AB-1606-XL          
* 其他：1756-HSC 高速计数 1756-PLS 限位控制 
* 系统设计目标：将多种控制方式集成在单一的控制器上，集成了顺序控制、运动控制、传动控制、过程控制。      
            
## CompactLogix硬件特性           
* 框架：		无
* 处理器：		1769-L***    
* 电源模块：	1769-PA4，120/240VAC，50/60Hz     
* 通讯模块：  1769-SDN，DeviceNet网络适配器     
* I/O模块：	
1769-IF8，      8通道，模拟量输入模块       
1769-IQ16，    16点，数字量输入模块（DC24V）        
1769-OF4CI，   4通道，模拟量输出模块             
1769-OB16，	 16点，数字量输出模块（DC24V）              
* 通讯方式：RS232/DH-485串口通信、EtherNet网络通讯
* DC24V电源：AB-1606-XL

