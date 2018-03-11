# 日志(Log in C#)
Date: 2018-01-28     
Author：Barret    
> 日志，又称为 Log，是我们开发人员的又一利器，其实，不管是在调试还是测试的时候，日志都可以帮助我们解决问题。所谓的日志，其实是一种记录机制，允许我们在程序代码中插入一些特殊的输出代码，将程序当前的运行状态随时输出，以便于在无人值守的情况下记录信息，在事后对程序的处理过程进行分析。        

## 一个完善的日志系统绝不是简单地在控制台输出，它至少需要支持下面的几个特征：            
1. 使用一种方式就支持输出到多个目的地，例如：控制台，文本文件，数据库，甚至电子邮件等等                
2. 允许控制输出的级别，过滤输出的内容，而不需要大幅度修改日志程序
3. 使用简单，可以使用简单的语法来记录日志
目前，存在着多个成熟的日志系统供我们选择，在 .NET 开发平台上，主要有两个日志系统：.NET 平台直接支持的日志系统和开源的**Log4Net**。

## .NET 平台内置的日志系统                 
### 三个概念：                  
1. 日志器：用来发送日志，在开发中，主要是用日志器来输出日志信息                                
2. 监听器：日志器用来输出日志信息，日志信息输出到哪里呢? 监听器用来完成实际的日志记录功能，在日志系统中，存在多种监听器，用来将日志信息记录到不同的目的地    
3. 日志控制开关：在软件生命周期的不同阶段，我们需要不同的日志信息，在开发阶段，可能需要比较详细的日志来检查错误，在运行阶段，大部分的问题已经被处理，我们可能仅仅需要记录一些关键信息，通过控制开关，可以在不需要修改代码的情况下，调整日志输出的内容                             
在 .NET 中，关于日志处理的相关类型，定义在命名空间 System.Diagnostics 中，有两个预定义的日志器类型，Debug 和 Trace。                   

### 监听器 Debug 和 Trace 的工作机制是相同的，区别仅仅在于：                        
> Debug 仅仅在编译器定义了 DEBUG 常量的情况下工作，而 Trace 仅仅在定义了 TRACE 常量的情况下工作。   

> 默认情况下，当我们使用 Debug 模式编译的时候，默认已经定义了这两个常量。所以，不管使用 Debug 还是 Trace 都可以输出日志信息；如果我们将程序编译为发布模式，那么，将仅仅定义 TRACE 常量，导致忽略 Debug 的存在，只记录 Trace 的日志信息。                   

### 监听器                   
> 所有的监听器都要从 TraceListener  派生，这是定义在命名空间 System.Diagnostics 中的一个抽象基类。通常我们直接使用系统的一些派生类:                   
```
System.Diagnostics.DefaultTraceListener 
System.Diagnostics.Eventing.EventProviderTraceListener 
System.Diagnostics.EventLogTraceListener 
System.Diagnostics.TextWriterTraceListener 
System.Web.WebPageTraceListener
```                   

> 作为文本格式的日志， System.Diagnostics.TextWriterTraceListener 又有几个常用的派生类:                     

```
System.Diagnostics.ConsoleTraceListener 
System.Diagnostics.DelimitedListTraceListener 
System.Diagnostics.EventSchemaTraceListener 
System.Diagnostics.XmlWriterTraceListener
```                    

1. Debug 和 Trace 使用相同的监听器，默认情况下，在它们的监听器集合属性 Listeners 中，已经添加了一个 System.Diagnostics.DefaultTraceListener 的实例，以便输出到 Visual Studio 的 Output 窗口中，所以，在使用了日志之后，我们可以打开 Output 窗口来看看实际的输出。如果我们希望能够在控制台窗口中看到输出，那么只需要增加一个 ConsoleTraceListener 就可以了。                  

2. 增加一个可以输出到控制台的 Listener                           
```
System.Diagnostics.Trace.Listeners.Add( 
    new System.Diagnostics.ConsoleTraceListener() 
    );
```                  

3. 增加一个可以输出到文本文件的 Listener                  
```
System.Diagnostics.Trace.Listeners.Add( 
    new System.Diagnostics.TextWriterTraceListener("log.txt") 
    );
```                

4. 增加一个可以输出到系统日志的 Listener         
```
System.Diagnostics.Trace.Listeners.Add( 
    new System.Diagnostics.EventLogTraceListener("Source") 
    );
```                   

5. 通过配置文件管理 Listener                      
> Listener也可以不在程序中固定声明，而是通过配置文件来更加方便地定义。这样的话，我们就可以动态地修改监听器，而不需要修改我们的代码了，如：     
```
<add name="fileListener"
        type="System.Diagnostics.TextWriterTraceListener"
        initializeData="log.txt" />
```           

### 日志控制             
1. 最简单的方式就是使用 WriteLine 来输出日志，就跟使用 Console 一样。
2. 通过 WriteLineIf ，可以指定一个条件，仅仅当条件满足的时候，才会输出日志。

### 日志开关
> 为了方便使用，在 .NET 中又提供了一个日志的开关，来方便我们指定条件:                    
0. 关闭，不希望输出日志
1. 错误级别的日志
2. 建议输出错误和警告级别的日志
3. 一般信息的日志也输出
4. 详细日志        

```
//设置日志开关
<add name="traceSwitch" value="0"/>

//获取日志开关
System.Diagnostics.TraceSwitch myTraceSwitch = 
    new System.Diagnostics.TraceSwitch("traceSwitch", string.Empty);

//使用日志开关
if( myTraceSwitch.TraceError) 
    System.Diagnostics.Trace.TraceError("Error!!!");   
if( myTraceSwitch.TraceWarning) 
    System.Diagnostics.Trace.TraceWarning("Warning!!!");
```             

## 参考——写系统日志              
```
class Program
{
    static void Main(string[] args)
    {
        //写系统日志
        EventLog.WriteEntry("source", "message", EventLogEntryType.Warning, 11, 21); 

        //读系统日志
        EventLog sample = new EventLog();
        sample.Log = "Application";
        EventLogEntryCollection myCollection = sample.Entries;
        foreach (EventLogEntry test in myCollection)
        {

            Console.WriteLine(test.Message, test.Source, test.EventID);

        }

        //Trace写系统日志、控制台、文件
        Trace.Listeners.Add(new EventLogTraceListener("TEST"));
        Trace.Listeners.Add(new ConsoleTraceListener());
        Trace.Listeners.Add(new TextWriterTraceListener("log.txt"));
        Trace.WriteLine("My first event log");
        Trace.Flush();
    }
}
```                 
> 本文作者--Barret Xiong--    
> 转载请注明出处！

