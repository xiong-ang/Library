---
title: C# Clipboard使用（How to Use Clipboard in C#）
date: 2017-10-29 21:41:31
tags: [Clipboard, C#]
categories: C#
---
# **Clipboard引荐**
> Clipboard，即剪切板，当我们同时按动Ctrl＋C时，选定的对象就被存放在了剪切板中了。如果刚才选定的对象是文件，那么在定盘符后，按动Ctrl＋V键或者点击菜单“粘贴”，这时选定的文件就保存到指定的磁盘上面了；如果选定的对象是图象，那么在打开“画图”之后，按动Ctrl＋V键或者点击菜单“粘贴”，图象就会显示在画图中了；如果是文本，那么在打开文本编辑器后，按动Ctrl＋V键或者点击菜单“粘贴”，这时文本就会显示在文本编辑器中。当然选定的对象还有许多种，这就不一一举例了。上面这些操作其实就是剪切板几种典型的操作。那么这些操作如果用Visual C＃来实现到底是个什么样子？  
微软引入**Clipboard类**进行处理，我们先来介绍[MSDN](https://msdn.microsoft.com/zh-cn/library/system.windows.forms.clipboard.aspx)上的一个例子进入今天的讨论。   

```        
/*
下述代码放置文本数据进入Clipboard，并从中在提取出来。
Winfrm代码界面元素包括：button1, button2, textBox1, textBox2
*/

private void button1_Click(object sender, System.EventArgs e) {
    //button1_Click 调用SetDataObject将textBox1中选中的文字放入系统剪切板

    if(textBox1.SelectedText != "")
       Clipboard.SetDataObject(textBox1.SelectedText);
    else
       textBox2.Text = "No text selected in textBox1";
 }

 private void button2_Click(object sender, System.EventArgs e) {
    //button2_Click 调用GetDataObject从系统剪切板提取数据

    IDataObject iData = Clipboard.GetDataObject();

    //判断是否有Text数据格式
    if(iData.GetDataPresent(DataFormats.Text)) {
       textBox2.Text = (String)iData.GetData(DataFormats.Text); 
    }
    else {
       textBox2.Text = "Could not retrieve data off the clipboard.";
    }
 }
```     

从这个例子可以学到Clipboard的最基本用法：用SetDataObjec将数据存入系统剪切板；用GetDataObject从系统剪切板获取数据。  

# **使用Clipboard保存/获取各种媒体**   

以实际操作的经验，剪切板不仅可以拷贝文字，还可以拷贝图片、文件，当然，通过程序，还可以让Clipboard保存音频和自定义类型。下面针对这些功能分别加以介绍。  

* **Clipboard保存多媒体**      


```       
class ClipboardDataFormat
{
    public static readonly string TEXT="Text";
    public static readonly string IMAGE="Image";
    public static readonly string FILEDROP="FileDrop";
    public static readonly string AUDIO="Audio";
    public static readonly string USERDEFINED="UserDefined";
}

class ClipboardProcesser
{
    public static void SetDataToClipboard(object o, string type)
    {
        try
        {
            if (o != null)
            {
                if (type == ClipboardDataFormat.TEXT)
                {
                    Clipboard.SetText((string)o);
                }
                if (type == ClipboardDataFormat.FILEDROP)
                {
                    Clipboard.SetFileDropList((System.Collections.Specialized.StringCollection)o);
                }
                if (type == ClipboardDataFormat.IMAGE)
                {
                    Clipboard.SetImage((System.Drawing.Image)o);
                }
                if (type == ClipboardDataFormat.AUDIO)
                {
                    Clipboard.SetAudio((Stream)o);
                }
                if (type == ClipboardDataFormat.USERDEFINED)
                {
                    MyItem item = (MyItem)o;
                    item.ToClipboard();
                }
            }
        }
        catch (Exception)
        {
        }
    }
```     

```      
//自定义要往剪切板添加的类型
[Serializable]
public class MyItem
{
    public string ItemName { get; set; }
    public object ItemData { get; set; }
    
    public void ToClipboard()
    {
        DataFormats.Format format = DataFormats.GetFormat(ClipboardDataFormat.USERDEFINED);

        IDataObject dataObj = new DataObject();
        dataObj.SetData(format.Name, false, this);
        Clipboard.SetDataObject(dataObj, false);
    }
}
```      

* **Clipboard获取多媒体**     

```      
public static object GetDataFromClipboardByType(string type)
    {
        object retObj = null;
        try
        {
            if (type == ClipboardDataFormat.TEXT)
            {
                retObj = Clipboard.GetText();
            }
            if (type == ClipboardDataFormat.FILEDROP)
            {
                retObj = Clipboard.GetFileDropList();
            }
            if (type == ClipboardDataFormat.IMAGE)
            {
                retObj = Clipboard.GetImage();
            }
            if (type == ClipboardDataFormat.AUDIO)
            {
                retObj = Clipboard.GetAudioStream();
            }
            if (type == ClipboardDataFormat.USERDEFINED)
            {
                IDataObject iData = Clipboard.GetDataObject();
                retObj = (MyItem)iData.GetData(type);
            }                
        }
        catch (Exception)
        {
            retObj = null;
        }
        return retObj;
    }

    public static string GetDataTypeFromClipboard()
    {
        string type = string.Empty;

        try
        {
            if (Clipboard.ContainsText())
            {
                type = ClipboardDataFormat.TEXT;
            }
            else if (Clipboard.ContainsFileDropList())
            {
                type = ClipboardDataFormat.FILEDROP;
            }
            else if (Clipboard.ContainsImage())
            {
                type = ClipboardDataFormat.IMAGE;
            }
            else if (Clipboard.ContainsAudio())
            {
                type = ClipboardDataFormat.AUDIO;
            }
            else
            {
                IDataObject iData = Clipboard.GetDataObject();
                if (iData.GetDataPresent(ClipboardDataFormat.USERDEFINED))
                {
                    type = ClipboardDataFormat.USERDEFINED;
                } 
            }
        }
        catch (Exception)
        {
        }
        return type;
    }
}
```     

# **让窗体自动响应Clipboard的变化**   

让窗体自动响应Clipboard的变化这是一个非常酷的功能，实现这个需求并不难，大致分四步：   

* **Form类中引入三个函数**     

```      
// SetClipboardViewer 用于往观察链中添加一个窗口句柄，这个窗口就可成为观察链中的一员了，返回值指向下一个观察者
[System.Runtime.InteropServices.DllImport("user32")]
private static extern IntPtr SetClipboardViewer(IntPtr hwnd);

//ChangeClipboardChain删除由hwnd指定的观察链成员，这是一个窗口句柄，第二个参数hWndNext是观察链中下一个窗口的句柄
[System.Runtime.InteropServices.DllImport("user32")]
private static extern IntPtr ChangeClipboardChain(IntPtr hwnd,IntPtr hWndNext);

//SendMessage 发送消息
[System.Runtime.InteropServices.DllImport("user32")]
private static extern int SendMessage(IntPtr hwnd,int wMsg,IntPtr wParam,IntPtr lParam);

//Clipboard内容变化消息
const int WM_DRAWCLIPBOARD = 0x308;
//Clipboard观察链变化消息
const int WM_CHANGECBCHAIN = 0x30D;
```     

* **Form_Load中把窗口添加到观察链中成为观察者，并保存下一个观察者的句柄**     

```     
//存放观察链中下一个窗口句柄   
IntPtr NextClipHwnd;
private void Form_Load(object sender, System.EventArgs e)
{  
     //获得观察链中下一个窗口句柄
    NextClipHwnd=SetClipboardViewer(this.Handle);           
}
```     

* **重载WndProc方法，监视剪切板，并处理，并把剪切板变化的消息发送给下一个观察者**     

```      
protected override void WndProc(ref System.Windows.Forms.Message m)
{
    switch(m.Msg)
    {
        case WM_DRAWCLIPBOARD:
            //将WM_DRAWCLIPBOARD消息传递到下一个观察链中的窗口
            SendMessage(NextClipHwnd,m.Msg,m.WParam,m.LParam);

            //获取Clipboard内容，并处理
            IDataObject iData = Clipboard.GetDataObject();
            ...
            ...
            break;
        default:
            base.WndProc(ref m);
            break;
    }       
}
```     

* **Form_Closed中撤消自己定义的观察者，并通知下一个观察者**      

```     
private void Form_Closed(object sender, System.EventArgs e)
{
    //从观察链中删除本观察窗口
    ChangeClipboardChain(this.Handle,NextClipHwnd);
    //将变动消息WM_CHANGECBCHAIN消息传递到下一个观察链中的窗口
    SendMessage(NextClipHwnd,WM_CHANGECBCHAIN,this.Handle,NextClipHwnd);  
}
```       

# **解决Clipboard冲突**   
* 待补充       

# **[[Demo](https://github.com/xiong-ang/CShape_SLN)]**   
* **界面**   
![Alt text](https://github.com/xiong-ang/CShape_SLN/blob/master/Image/ClipBoard.PNG?raw=true)     

* **功能**   
自动捕捉剪切板变化，并显示其中的文字、图片、文件以及自定义类型。