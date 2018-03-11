# NXCOMPACT位标志与C#编译器（NXCOMPACT and the C# compiler）    
Date: 2018-03-11     
Author：Barret     
> C#编译器（.net 3.5 framework）目前编译的PE文件会自动带上NXCOMPACT标志位。这个标志对：1.与原生二进制互操作；2.暴露接口给第三方 的情形影响比较大。                                   
## 数据执行保护（Data Execution Prevention，DEP）             
DEP是阻止内存页中没有被标记为可执行的数据运行的一种技术，其对应PE文件的一个标识**IMAGE_DLLCHARACTERISTICS_NX_COMPAT**。对于可执行镜像，如果设置该标识，进程将在DEP模式下运行，除非系统配置DEP为关闭状态。对于DLL镜像，系统将这一检查以提高性能。这些仅仅适用于X86，32为的进程。          
在32位系统中，DEP依赖于PE配置和系统策略，而在64位的系统中，DEP总是打开的。            

## C#编译器处理              
C#编译器提供的MSIL PE格式提供了对这一标识的处理，但是其默认设置了**IMAGE_DLLCHARACTERISTICS_NX_COMPAT**，如果C#应用需要调用不兼容DEP的二进制文件，将产生**IP_ON_HEAP **异常。因此我们需要用到editbin.exe去掉该标识。               
如果用到VS，可以再Post-build中执行这些操作：1.使用 VSVARS32.BAT创建环境变量；2.editbin.exe去除标识。举例如下：
```
call $(DevEnvDir)..\tools\vsvars32.bat
editbin.exe /NXCOMPAT:NO $(TargetPath)
```                
对于需要签名的场景，如果在签名后去除该标识，将导致强签名的失败。因此我们还需要在Post-build中使用SN.EXE进行签名操作。       

**[Reference](https://blogs.msdn.microsoft.com/ed_maurer/2007/12/13/nxcompat-and-the-c-compiler/)**      
> 本文作者--Barret Xiong--    
> 转载请注明出处！