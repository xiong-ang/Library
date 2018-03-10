---
title: EngjoyDrawing总结(EnjoyDrawing Summary)
date: 2017-11-11 20:55:20
tags: [C#,WPF,MVVM]
categories: [Just Do It]
---
## Command模式实现Redo和Undo                       
图形处理软件中Redo/Undo往往都是非常重要的功能，这里需要用到Command模式，将编辑图形的行为封装成Action，而每个Action有Do/Redo/Undo            

## 单例模式实现ModelProcessor                      
单例模式往往与静态类成员方法用来比较，如果有状态，适合用单例模式，如果有明确概念，也适合用单例模式，如果仅仅是一类辅助方法，可以考虑类静态成员方法                   

## UI线程与非UI线程--WaitBox实现                           
[C# Winform 跨线程更新UI控件常用方法汇总](http://www.cnblogs.com/marshal-m/p/3201051.html)              

## C# Log处理--Log4Net
软件的一些基础设施，比如：Log/Exception/CrashReport...          
这些都有一些现成的工具可以使用，善于重用别人的代码             

## 延迟签名
SNK
http://www.cnblogs.com/yangecnu/archive/2013/01/01/2841235.html
PFX
http://blog.csdn.net/zj510/article/details/39964533
http://blog.csdn.net/luminji/article/details/3960308
http://blog.csdn.net/laotse/article/details/6302333             
商业的化的产品一般采用延迟签名技术                               

## MVVM实现UI与逻辑分离
MVVM在界面与逻辑的处理方面是一种前沿，用前端的技术来处理界面，后端的基础处理逻辑是一种趋势，随着技术的发展，这种分工会更加明显                      

## UI界面中Shell的实现
这种技术目前来看没有什么意义，只是装酷~               

## XPath处理
XML的处理有很多种，掌握一种非常重要，C#框架支持这种处理，JAVA可以进入两个Jar包：dom4j.jar,jaxen.jar

## PerformanceRecord
一种代码的积累                 

[参考代码](https://github.com/xiong-ang/EnjogDrawing)