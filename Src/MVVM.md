---
title: WPF使用MVVM模式
date: 2017-11-21 20:54:07
tags: [C#,WPF,MVVM]
categories: [Design Pattern,C#]
---
# MVVM设计模式           
> MVVM是Model、View、ViewModel的简写，这种模式的引入就是使用ViewModel来降低View和Model的耦合，说是降低View和Model的耦合。也可以说是是降低界面和逻辑的耦合，理想情况下界面和逻辑是完全分离的，单方面更改界面时不需要对逻辑代码改动，同样的逻辑代码更改时也不需要更改界面。同一个Model可以使用完全不用的View进行展示，同一个View也可以使用不同的Model以提供不同的操作。           
    
## 各部分责任分析           
1. **Model** 负责数据和逻辑的处理，对于Model层，它什么也不知道         
2. **ViewModel** 将逻辑交由Model处理，并向外暴露数据，它只知道Model          
3. **View** 显示界面，并将数据变化通知ViewModel，它只知道ViewModel            
![](https://github.com/xiong-ang/xiong-ang.github.io/blob/Hexo/MyBlog/MyBlog/images/MVVM.png?raw=true)                

# WPF使用MVVM设计模式            
> 在WPF的MVVM模式中，View和ViewModel之间数据和命令的关联都是通过绑定实现的，绑定后View和ViewModel并不产生直接的依赖。具体就是**View中出现数据变化时会尝试修改绑定的目标**。同样**View执行命令时也会去寻找绑定的Command并执行**。反过来，**ViewModel在Property发生改变时会发个通知说“名字叫XXX的Property改变了，你们这些View中谁绑定了XXX也要跟着变啊!”**，至于有没有View收到是不是做出变化也不关心。ViewModel中的Command脱离View就更简单了，因为Command在执行操作过程中操作数据时，根本不需要操作View中的数据，只需要操作ViewModel中的Property就可以了，Property的变化通过绑定就可以反映到View上。这样在测试Command时也不需要View的参与。            

## 待解决问题           
1. 数据绑定：View-->View; View-->ViewModel; ViewModel-->View            
2. 命令绑定与事件绑定：View-->ViewModel      
     
# 数据绑定          
## View-->View      
* XML绑定         
```
DestinationProperty=”{Binding ElementName=SourceObjectName, Path=SourceProperty}”
```               
* C#绑定            
```           
Binding binding = new Binding();
binding.Source = txtName;//设置源对象
binding.Path = new PropertyPath("Text");//设置源属性
tbShowMessage.SetBinding(TextBlock.TextProperty, binding);//添加到目标属性
//or
//BindingOperations.SetBinding(tbShowMessage, TextBlock.TextProperty, binding);
```                    
> 对集合添加、删除等操作又需要使用数据绑定时要优先考虑ObservableCollection<T>                        

## View-->ViewModel               
> View由依赖属性实现属性改变通知的传递，但要注意传递属性改变的时间，比如TextBox在响应TextChanged事件时，还没有传递，而响应LostFocus时，已经传递                      

## ViewModel-->View          
> ViewModel为了通知View数据的变化，必须借助INotifyPropertyChanged接口。                   

* 一般思路                 

```                   
class NotificationObject:INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    public void RaisePropertyChanged(string propertyName)
    {
        if (null != PropertyChanged)
        {
            PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
class ViewModel:NotificationObject
{
    private int _Age;
    public int Age
    {
        get { return _Age; }
        set
        {
            if(_Age != value) //防止通知死循环
            {
                _Age = value;
                RaisePropertyChanged("Age"); 
            }
        }
    }
    ...
}                        
```                         
 
# 命令绑定与事件绑定                       
## 命令绑定                    

```            
class DelegateCommand:ICommand
{
    public bool CanExecute(object parameter)
    {
        if(null != CanExecuteFunc)
            CanExecuteFunc(parameter);
        return true;
    }
    public event EventHandler CanExecuteChanged;
    public void Execute(object parameter)
    {
        if(null != ExecuteAction)
            ExecuteAction(parameter);
    }
    public Action<object> ExecuteAction{get;set;}
    public Func<object,bool> CanExecuteFunc{get;set;}
}
class ViewModel:NotificationObject
{
    public ViewModel()
    {
        ...
        OperCommand = new DelegateCommand();
        OperCommand.ExecuteAction += Oper;
    }
    ...
    public DelegateCommand OperCommand{get;set;}
    private void Oper(object parameter)
    {
        ...
    }
}

```        

## 事件绑定（两种方式）           
1. 引入reference库               
Microsoft.Expression.Interactions.dll                   
System.Windows.Interactivity.dll                        
2. XAML中加入引用                   

```                   
xmlns:i="http://schemas.microsoft.com/expression/2010/interactivity"           
xmlns:ei="http://schemas.microsoft.com/expression/2010/interactions"             
```                        

3. 在ViewModel中加入Method和事件                        

```              
public void Method()
{
    ...
}

public DelegateCommand OperCommand{get;set;}
```                             

4. 绑定事件                                      

```                     
<i:Interaction.Triggers>
　　<i:EventTrigger EventName="Loaded">
　　　　<i:InvokeCommandAction Command="{Binding OperCommand}" CommandParameter="{Binding ElementName=*}"/>
　　</i:EventTrigger>
　　<i:EventTrigger EventName="KeyUp">
　　　　 <i:EventTrigger EventName="Click">
               <ei:CallMethodAction TargetObject="{Binding}" MethodName="Method"/>
        </i:EventTrigger>
　　</i:EventTrigger>
</i:Interaction.Triggers>
```                                
                            
5. MVVMLight使用
* ViewModelBase提供ViewModel的基类                           
* RelayCommand和RelayCommand<T>实现命令机制                  
* EventToCommand将任意事件绑定到命令                        
* Message（消息）机制，提供了一个Messenger类型，可以用来发送和接收消息，它还提供了默认的几种消息类型。可以注册消息用来接收处理，可以产生并发送消息[[参考](https://www.cnblogs.com/chenxizhang/archive/2011/10/01/2197786.html)]                                            