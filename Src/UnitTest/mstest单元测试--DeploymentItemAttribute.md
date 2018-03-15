# mstest单元测试--DeploymentItemAttribute                 
Date: 2017-11-05     
Author：Barret      

## 应用场景     
从VS2012后，测试允许在独立部署的文件目录下运行，如果使用独立的测试部署目录，测试引擎将创建测试部署目录并拷贝相应的程序集到该目录。
但是有一些测试依赖附属文件，比如数据文件、配置文件、数据库等。为了确保在测试期间可以访问到这些文件，必须指定他们随测试程序集一起拷贝部署，此时就用到DeploymentItemAttribute          

## 作用     
DeploymentItemAttribute指定一个文件或一个目录应该随测试程序集一起部署，该特性不仅可用于**TestMethod**，还可用于**TestClass**                      

## 具体用法                          
1. 指定文件作为构建过程的一部分以拷贝到构建目录     
    * 如果仅仅需要指定到某个测试工程，可以在VS中Add该文件，并设置**Copy to Output Directory**：Copy if Newer           
    * 否则，可以定义一个post-build来拷贝文件：
    ```
    xcopy /Y /S "$(SolutionDir)SharedFiles\*" "$(TargetDir)"
    ```   
2. 在TestMethod或TestClass中使用DeploymentItemAttribute来指定依赖文件和拷贝目录     

## [参考](https://msdn.microsoft.com/en-us/library/microsoft.visualstudio.testtools.unittesting.deploymentitemattribute.aspx)

> 本文作者--Barret Xiong--    
> 转载请注明出处！







