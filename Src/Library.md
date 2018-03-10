---
title: 静态与动态链接库(Lib & Dll)
date: 2018-01-02 20:48:40
tags: [COM,C++]
categories: [COM]
---
## 介绍            
> 什么是静态链接呢？即在链接阶段，将源文件中用到的库函数与汇编生成的目标文件合并生成可执行文件。缺点是该可执行文件可能会比较大。好处是方便程序移植。                

> 静态连接库就是把(lib)文件中用到的函数代码直接链接进目标程序，程序运行的时候不再需要其它的库文件；这与动态链接相对应，动态链接就是把调用的函数所在文件模块（DLL）和调用函数在文件中的位置等信息链接进目标程序，程序运行的时候再从DLL中寻找相应函数代码，因此需要相应DLL文件的支持。                            

> 静态链接库与动态链接库都是共享代码的方式，如果采用静态链接库，则无论你愿不愿意，lib 中的指令都全部被直接包含在最终生成的 EXE 文件中了。但是若使用 DLL，该 DLL 不必被包含在最终 EXE 文件中，EXE 文件执行时可以“动态”地引用和卸载这个与 EXE 独立的 DLL 文件。静态链接库和动态链接库的另外一个区别在于静态链接库中不能再包含其他的动态链接库或者静态库，而在动态链接库中还可以再包含其他的动态或静态链接库。                        

> lib库有两种，一种是包含了函数所在DLL文件和文件中函数位置的信息，称为导出库；一种是包含函数代码本身。一般现有的DLL用的是前一种库           

1. 定义                    
LIB文件中包含函数代码本身，在编译时直接将代码加入程序当中。称为静态链接库static link library。             
LIB包含了函数所在的DLL文件和文件中函数位置的信息（入口），代码由运行时加载在进程空间中的DLL提供，称为动态链接库dynamic link library。                 
静态链接库包括两个文件：                                
（1）.h头文件，包含静态链接库中说明输出的类或符号原型或数据结构。应用程序调用静态链接库时，需要将该文件包含入应用程序的源文件中。                         
（2）.lib文件，放到固定位置，在应用程序中添加库目录，在附加依赖项中进行添加。                               
动态链接库包括三个文件：                                
（1）.h头文件，包含动态链接库中说明输出的类或符号原型或数据结构的.h文件。应用程序调用动态链接库时，需要将该文件包含入应用程序的源文件中。                           
（2）.lib文件，是动态链接库在编译、链接成功之后生成的文件，作用是当其他应用程序调用.dll时，需要将该文件引入应用程序，否则产生错误。如果不想用.lib文件或者没有.lib文件，可以用WIN32 API函数LoadLibrary、GetProcAddress装载。                              
（3）.dll文件，真正的可执行文件，开发成功后的应用程序在发布时，只需要有.exe文件和.dll文件，并不需要.lib文件和.h头文件。                               
2. .lib文件和.dll文件区别                                          
（1）.lib是编译时用到的，.dll是运行时用到的。如果要完成源代码的编译，只需要静态链接库；如果要使动态链接的程序运行起来，只需要动态链接库。                                
（2）如果有.dll文件，那么.lib一般是一些索引信息，记录了.dll中函数的入口和位置，.dll中是函数的具体内容；如果只有.lib文件，那么这个.lib文件是静态编译出来的，索引和实现都在其中。使用静态编译的.lib文件，在运行程序时不需要再挂动态库，缺点是导致应用程序比较大，而且失去了动态库的灵活性，发布新版本时要发布新的应用程序才行。                   
（3）动态链接的情况下，有两个文件：一个是.lib文件，一个是.dll文件。.lib包含被.dll导出的函数名称和位置，.dll包含实际的函数和数据，应用程序使用.lib文件链接到.dll文件。在应用程序的可执行文件中，存放的不是被调用的函数代码，而是.dll中相应函数代码的地址，从而节省了内存资源。.dll和.lib文件必须随应用程序一起发行，否则应用程序会产生错误。如果不想用.lib文件或者没有.lib文件，可以用WIN32 API函数LoadLibrary、GetProcAddress装载。                                                                

## 静态链接库                           

* **编写**                           
1. 新建工程           
New Project --> Visual C++ --> Win32 --> Application type(Static library)                     
2. 文件内容举例          

```              
// myMax.h  
  
#ifndef MY_MAX_HEADER  
#define MY_MAX_HEADER  
  
int myMax(int x, int y);  
  
#endif

//--------------------------------------------------------------

// myMax.cpp  
  
#include "myMax.h"  
  
int myMax(int x, int y)  
{  
    return x > y ? x : y;  
}
```              

* **使用举例**                     

```                   
//拷贝头文件myMax.h和库文件Mylib.lib到工程目录下

#include <stdio.h>  
#include "<path>/myMax.h"  
  
#pragma comment(lib, "<path>/Mylib.lib") //可以在VC编译器中手动设置
  
int main()  
{  
    int a = 80;  
    int b = 90;  
    printf("%d\n", myMax(a, b));  
  
    return 0;  
} 
```                             

## 动态链接库                       

* **编写**                           

1. 新建工程           
New Project --> Visual C++ --> Win32 --> Application type(DLL)                     

2. 文件内容举例              

```
// myMax.h  
  
#ifndef MY_MAX_HEADER  
#define MY_MAX_HEADER  
  
extern "C" _declspec(dllexport) int myMax(int x, int y);  
//也可以使用模块定义文件.DEF进行导出

#endif

//--------------------------------------------------------------

// myMax.cpp  
  
#include "myMax.h"  
  
int myMax(int x, int y)  
{  
    return x > y ? x : y;  
}
```                    



* **使用举例**                   
1. 显式链接             

```
//拷贝头文件myMax.h和库文件MyDll.dll到工程目录下
//修改头文件_declspec(dllexport)为_declspec(dllimport)

#include <stdio.h> 
#include <Windows.h> 
#include "<path>/myMax.h"  

typedef int (*pmyMax)(int, int);  
int main()  
{  
    int a = 80;  
    int b = 90;  

    //LoadLibrary函数装载DLL
    HINSTANCE hdll=LoadLibrary("<path>/MyDll.dll");

    //GetProcAddress函数获得函数地址
    pmyMax pfnMax=(pmyMax)GetProcAddress(hdll,"myMax");

    printf("%d\n", myMax(a, b));  

    //FreeLibrary释放DLL
    FreeLibrary(hdll);
  
    return 0;  
} 
```                 


2. 隐式使用(和静态库一样使用)           

```
//和使用静态库一样，静态库负责动态库的查找
//拷贝头文件myMax.h和库文件MyDll.dll以及MyDll.lib到工程目录下
//修改头文件_declspec(dllexport)为_declspec(dllimport)

#include <stdio.h>  
#include "<path>/myMax.h"  
  
#pragma comment(lib, "<path>/MyDll.lib") //可以在VC编译器中手动设置
  
int main()  
{  
    int a = 80;  
    int b = 90;  
    printf("%d\n", myMax(a, b));  
  
    return 0;  
} 
```                

## 补充                      
1. DLL 地狱                            
DLL 地狱（DLL Hell）是指因为系统文件被覆盖而让整个系统像是掉进了地狱。                     
简单地讲，DLL地狱是指当多个应用程序试图共享一个公用组件时，如某个DLL或某个组件对象模型（COM）类，所引发的一系列问题。      
2. DLL头文件定义                       

```
//在DLL导出的头文件*.h
#ifdef DLL_IMPLEMENT  
#define DLL_API extern "C" __declspec(dllexport)  
#else  
#define DLL_API extern "C" __declspec(dllimport)  
#endif

//在导出函数定义的地方
#define DLL_IMPLEMENT
#include "*.h" 

//在使用函数的地方使用默认声明
#include "*.h"
```                     

3. 注意                         
由于项目中创建的源文件为.CPP文件，即C++源文件，因此Visual C++按C++规范，并采用__cdecl调用约定对其进行编译。这样得到的导出函数就不能被C语言程序所调用。解决该问题的办法是在函数体名称前添加extern “C”修饰，告诉编译器，该函数按照C语言规范，并采用__cdecl调用约定进行编译。因此源文件Add.cpp中的代码可修改如下：         
```
extern “C” int add(int a, int b)
{
    return a + b;
}
```                           

最后重新编译该静态链接库项目，导出函数Add就能够被C语言程序所调用了。                      
4. [Demo](https://github.com/xiong-ang/COM_SLN)