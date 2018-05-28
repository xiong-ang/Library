# 创建npm库      
 
## npm库创建      
+ 创建命令行工具     
```
//package.json
"bin": {
"hi": "./index.js"
},
```  
+ 片段标识符     
```
//告知脚本使用node执行
#!/usr/bin/env node
```

## npm库发布流程       
+ 登录    
```
npm login
```
+ 发布    
```
npm publish
```
+ 重新发布     
修改package.json     
再执行npm publish