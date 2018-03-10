---
title: Android学习笔记(一)
date: 2018-01-28 13:26:18
tags: Android
categories: Android
---
# GeoQuiz     
> Android权威指南第三版1-6章练习              

## Activity生命周期及状态保持                 
![](https://github.com/xiong-ang/Android-Practice/blob/master/GeoQuiz/Activity%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.PNG?raw=true)                   
                    
> onSaveInstanceState(Bundle outState)在onStop()方法之前由系统调用，**除非用户按后退键**。onSaveInstanceState的默认实现要求所有activity将自身状态数据保存在Bundle对象中。onSaveInstanceState不仅仅处理与设备旋转相关的问题。用户离开当前activity用户界面或者Android需要回收内存时，activity也会被销毁。         

> onStop()和onSaveInstanceState()是两个可靠的方法，常见的做法是：覆盖onSaveInstanceState()方法，在Bundle对象中，保存当前activity的小的或暂存状态的数据；覆盖onStop()方法，保存永久性数据，如用户编辑的文字等。onStop()方法调用完，activity系统随时会被系统销毁，所以用它保存永久性数据。          
  
## Activity间数据传递               
### 启动Activity并传递数据             
> 启动Activity的请求发送给操作系统的ActivityManager，ActivityManager负责创建Activity实例并调用其onCreate(Bundle)方法。     

```
//发送端
//启动Activity，Intent对象可以附带数据        
startActivity(Intent intent)
//or
startActivityForResult(Intent intent, int requestCode)

//接收端
//从发送Intent获取数据
getIntent().get*Extera(*)                   
```              

### Activity销毁并返回数据           
```
//发送端
//启动Activity，Intent对象可以附带数据，requestCode记录请求代码
startActivityForResult(Intent intent, int requestCode)

//接收端
//设置返回结果，Intent对象可附带返回数据
setResult(int resultCode)
//or
setResult(int resultCode, Intent data)

//发送端
//在用户退回到发送端时，ActivityManager调用activity方法：
onActivityResult(int requestCode, int resultCode, Intent data)
```          


# CriminalIntent        
> Android权威指南第三版7-8章练习             

## Fragment                 
> 采用Fragment而不是Activity管理应用UI，可以绕开Android系统Activity使用规则的限制，增加UI设计的灵活性             

容纳单个Fragment的Activity抽象类：                 
```
public abstract class SingleFragmentActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_fragment);

        FragmentManager fm=getSupportFragmentManager();
        Fragment fragment=fm.findFragmentById(R.id.fragment_container);

        if(fragment==null)
        {
            fragment=createFragment();
            fm.beginTransaction().add(R.id.fragment_container,fragment).commit();
        }
    }
    protected abstract Fragment createFragment();
}
```               

## 使用RecyclerView显示列表               
### RecyclerView        
> RecyclerView的任务仅限于回收和定位屏幕上的View          

### ViewHolder       
> 容纳View视图               

### Adapter                 
> Adapter负责创建必要的ViewHolder以及绑定ViewHolder至模型层数据             

### 各成员关系：
![](https://github.com/xiong-ang/Android-Practice/blob/master/CriminalIntent/RecyclerView.PNG)