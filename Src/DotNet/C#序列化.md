# C#序列化(Serialization in C#)
Date: 2018-01-01      
Author：Barret   

## 序列化与反序列化             
1. 定义                     
> 序列化(Serialize)将对象的状态信息转换为可以存储或传输的形式的过程。在序列化期间，对象将其当前状态写入到临时或持久性存储区。以后，可以通过从存储区中读取或反序列化(Deserialize)对象的状态，重新创建该对象。                               

2. 应用                          
* 存储应用程序工程文件数据                        
* 存储数据对象，隔离组件，方便大型程序中组件的测试                     

## 几种序列化方式                  
1. 序列化方式                         
二进制序列化：对象序列化之后是二进制形式的，通过BinaryFormatter类来实现的，这个类位于System.Runtime.Serialization.Formatters.Binary命名空间下。       
SOAP序列化：对象序列化之后的结果符合SOAP协议，也就是可以通过SOAP 协议传输，通过System.Runtime.Serialization.Formatters.Soap命名空间下的SoapFormatter类来实现的，必要时注意包含程序集。                                          
XML序列化：对象序列化之后的结果是XML形式的，通过XmlSerializer 类来实现的，这个类位于System.Xml.Serialization命名空间下。XML序列化不能序列化私有数据。      
JSON序列化：与XML序列化类似，将public字段序列化成JSON格式，通过NewtonSoft.Json.dll实现。                                                           
2. 几种序列化的区别                                      
二进制格式和SOAP格式可序列化一个类型的所有可序列化字段，不管它是公共字段还是私有字段。XML格式和JSON格式仅能序列化公共字段或拥有公共属性的私有字段，未通过属性公开的私有字段将被忽略。XML序列化要求序列化的类必须是public的。                                        
使用二进制格式序列化时，它不仅是将对象的字段数据进行持久化，也持久化每个类型的完全限定名称和定义程序集的完整名称（包括包称、版本、公钥标记、区域性），这些数据使得在进行二进制格式反序列化时亦会进行类型检查。SOAP格式序列化通过使用XML命名空间来持久化原始程序集信息。而XML和JSON格式序列化不会保存完整的类型名称或程序集信息。这便利XML和JSON数据表现形式更有终端开放性。如果希望尽可能延伸持久化对象图的使用范围时，SOAP格式、XML格式和JSON是理想选择。                     

## 序列化实现方式                  
[代码实现](https://github.com/xiong-ang/CShape_SLN)                     

## 补充分析      
1. [Serializable] ：Binary序列化必须要求序列化对象标记[Serializable]；XML和Json序列化不要求         
2. Interface序列化：XML不可序列化；Json可以序列化，不可以反序列化；Binary既可以序列化，也可以反序列化。因此，如果类成员变量有接口或抽象类，要么用Binary序列化，要么进行封装                  
3. 泛型序列化：XML不可序列化；Binary和Json可以序列化，如Dictionary<>               
4. 序列化与dll签名：Binary序列化包含签名信息；XML包含类名；Json只包含属性值              
5. 是否要求默认构造函数：XML和Json要求默认构造函数，Binary不需要                        
6. [测试代码](https://github.com/xiong-ang/CShape_SLN)


## 注意事项                  
1. 要让一个对象支持.Net序列化服务，用户必须为每一个关联的类加上[Serializable]特性。如果类中有些成员不适合参与序列化（比如：密码字段），可以在这些域前加上[NonSerialized]特性                           
2. OptionalField表示可选字段，如果软件版本更新，多出一些字段，可使用[OptionalField]特性，可保证各版本文件的兼容                         
3. Serializationbinder是抽象类，用于控制在序列化和反序列化期间使用的实际类型，在序列化内容转移时尤为有用 [参考](https://msdn.microsoft.com/zh-cn/library/ffas09b2)          

> 本文作者--Barret Xiong--    
> 转载请注明出处！