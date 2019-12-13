# 插件式设计方案     
> App为了提供可扩展性，经常会采用插件式的设计方案。        
> App层面上，在核心功能之上，提供扩展性功能的模块称为插件。    
> 本文主要介绍可动态扩展App功能的插件。    

## 应用场景  
> 插件式设计方案主要应用于整理功能相对稳定，对扩展性要求相对较高的场景。      

例如：   
1.  编辑器和IDE   
    * 整体功能：文本编辑，代码高亮，智能提示，代码编译，调试，运行
    * 扩展点： 对已知语言或未知语言的支持     
2. 音频处理软件     
    * 整体功能： 对音频编辑提供一体化的支持流程   
    * 扩展点： 对不同音频格式的支持    

## 设计要点 
### 插件安全控制     
1. 验证/授权 - 在执行插件操作前，应验证当前用户权限    
2. Context  - 使用Context模式进行资源的控制与保护         
### 动态发现与注册     
> 仅仅是一种host动态加载plugin和plugin订阅host方法的机制，可以通过任意方式实现，比如：反射，配置文件，注册表。     

* C#通过反射方式简单实现动态加载示例
```C#
public void Initialize()
{
    pluginInstances = pluginInstances ??
            (from file in Directory.GetFiles(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"Plugin\"), "*.dll")
            from type in Assembly.LoadFrom(file).GetTypes()
            where type.GetInterfaces().Contains(typeof(IPlugin))
            select (IPlugin)Activator.CreateInstance(type)).ToList();
}

public void Action()
{
    if(pluginInstances != null)
        pluginInstances.ForEach(plugin => plugin.Action());
}
```   

### 线程模型      
* 插件的线程模型不应该受Host线程的较大影响，最好能够相对独立   
* 插件的线程模型取决于插件的业务，如果需要费时操作，内部可采用多线程     

### 生命周期管理     
> 对于性能要求不高的场景，不要求管理插件的生命周期。       
1. 加载  
2. 注册   
3. 触发   
4. 卸载  


## 案例分析 - MEF    
> MEF - Managed Extensibility Framework    
> MEF = Container + Plugin Loading Mechanism       
> MEF 核心包括一个catalog和一个CompositionContainer, catalog用于发现扩展，而Container用于协调创建和梳理依赖性        
> 工作流程：
> 1. import/export - 指定实体的注册或导入
> 2. 指定catalog - 指定扩展的加载方式
> 3. 创建Container   
> 4. ComposeParts - 组合各部分      
![](https://github.com/xiong-ang/Library/blob/master/Pic/PluginPattern.jpg?raw=true)          

**Container**： MEF提供了类似容器的概念，实现了Service的注册与发现，可以通过C#特性的方式实现类实例的注册与实体的注入。        
**Plugin**： MEF提供了三种扩展发现方式：1）AssemblyCatalog在当前程序集发现部件；2）DirectoryCatalog在指定的目录发现部件；3）DeploymentCatalog在指定的XAP文件中发现部件（用于silverlight）    

## 参考
[1] https://medium.com/omarelgabrys-blog/plug-in-architecture-dec207291800   
[2] http://www.eclipse.org/articles/Article-Plug-in-architecture/plugin_architecture.html      
[3] https://istacee.wordpress.com/2013/09/09/very-simple-plugin-mechanism-in-c/      
[4] https://www.cnblogs.com/ulex/p/4186881.html     
