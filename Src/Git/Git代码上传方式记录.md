# Git代码上传方式记录(Git Upload)              
Date: 2018-03-11     
Author：Barret         

## Clone-->Change-->Push                     
```                    
git clone Git_Url
//Change files
git add .
git commit -m "..."
git push
```           

## Create a new local repository and upload                  
```
echo "# Library" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin Git_Url
git push -u origin master
```                    

## Push an existing local repository                             
```
git remote add origin Git_Url
git push -u origin master
```

> 本文作者--Barret Xiong--    
> 转载请注明出处！
