# COM vs DLL

## DLL
* 进程中服务器
* 一种二进制组件化的方式
* 普通动态链接库提供函数类型的接口
* 分享方式是文件拷贝，指定文件路径
* DLL Hell：共用的dll版本升级中，造成某些软件功能异常

## COM
* 一种二进制组件化的规范
* 定义面向对象的接口
* 接口一定定义，绝不更改；升级时，定义新接口
* 可以做到语言无关
* 不需要知道文件位置，全局注册，通过GUID进行组件查找和接口查找
* QueryInterface 组件的任何一个接口都可以被Client用来获取它所支持的其它接口[IUnknow]
* AddRef和Release提供对组件生命周期的控制[IUnknow]

## 关系
* Dll是COM的一种载体
* COM进行注册与反注册的函数用Dll接口的形式暴露
* 相对于Dll函数形式的接口定义方式，COM提供基于Interface方式的二进制接口更优
* 引用普通Dll需要获取二进制文件，COM提供全局注册的功能，二进制共享方式更优

## Reference
[DLL导出类和函数](https://huangwang.github.io/2018/06/15/DLL%E5%AF%BC%E5%87%BA%E7%B1%BB%E5%92%8C%E5%87%BD%E6%95%B0/)