# C#中路径、目录、文件的处理（Path, Directory, File in C#）          
Date: 2018-03-11     
Author：Barret    
## [Path](https://msdn.microsoft.com/en-us/library/system.io.path(v=vs.110).aspx)           
**Path类（静态类）**：专门操作文件路径，处理文件路径中的路径、文件名、扩展名等    

## [Directory](https://msdn.microsoft.com/en-us/library/system.io.directory(v=vs.110).aspx)          
**Directory类（静态类）**：Directory类提供了在目录和子目录中进行创建移动和列举操作的静态方法。此外，还可以访问和操作各种各样的目录属性，例如创建或最后一次修改时间以及Windows访问控制列表等     

## [File](https://msdn.microsoft.com/en-us/library/system.io.file(v=vs.110).aspx)              
**File类（静态类）**： 与Directory类似，只不过处理的是文件      

## Example--批量修改文件名                
```
using System;
using System.IO;

namespace fileRename
{
    class Program
    {
        static void Main(string[] args)
        {
            //Get file names
            string[] fileNames=Directory.GetFiles("C:\\***");
            for (int i = 0; i < fileNames.Length; i++)
            {
                string oldFileName=fileNames[i];

                //Create new file name
                string path=Path.GetDirectoryName(oldFileName);
                string name=Path.GetFileName(oldFileName);
                string newFileName=path+"\\"+i+"_"+name;

                Console.WriteLine(newFileName);

                //Rename file name
                File.Move(oldFileName,newFileName);
            }
        }
    }
}
```            
> 本文作者--Barret Xiong--    
> 转载请注明出处！