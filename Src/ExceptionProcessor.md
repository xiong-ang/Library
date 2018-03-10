---
title: C#中的异常处理(Exception Process in C#)
date: 2017-11-11 16:27:15
tags: [Exception, C#]
categories: C#
---
# 异常处理规则             
1. 不要抛出“new Exception()”-->抽象的异常往往让人迷惑          
2. 不要只记录Exception.Message的值，还需要记录Exception.ToString()-->Exception.ToString()包含“堆栈跟踪”信息     
3. catch要捕捉具体异常-->好的代码只捕捉知道的异常      
4. using使用-->异常发生时，using能放在资源泄露              
5. 新异常应将原始异常作为其内部异常            
6. catch抛出新异常会导致call stack丢失-->不处理异常时可以“throw;”              

# 自定义异常     

```             
public class SQLException : Exception
{
    public SQLException(string message) : base(message)
    {
    }
}
```          

# 异常处理                      
* **应用程序处理未捕获的异常**              

```                     
static void InitUnhandledException()
{
    AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException;
}
static void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs unhandledExceptionEventArgs)
{
    var exception = unhandledExceptionEventArgs.ExceptionObject as Exception;
    //Log
}
```                   

> 未捕获的异常，通常就是运行时期的**BUG**，于是我们可以在UnhandledException的注册事件方法CurrentDomain_UnhandledException中将未捕获异常的信息**记录在日志中**。值得注意的是，UnhandledException提供的机制并不能阻止应用程序终止，也就是说，CurrentDomain_UnhandledException方法执行后，应用程序就会被终止。当然，这只是处理Console的未处理异常，还有更多[相关细节](http://www.cnblogs.com/luminji/archive/2011/01/05/1926033.html)。          


# 异常报告--CrashReporter.NET功能介绍