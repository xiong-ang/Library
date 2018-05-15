# ES6-Angularjs 写法总结              

> 总结Angularjs的ES6写法             

## module           
```
import Module1 from 'path1';
import Module2 from 'path2';
import Module3 from 'path3';

const appModule = angular.module('MyApp',[
    'Module1',
    'Module2',
    'Module3'
]);
```

## controller            
```
//Create Controller Class
class MyCtrl
{
    constructor(srv1, srv2, ...)
    {
        ...
    }
    ...
}
MyCtrl.$inject = ['srv1', 'srv2'];
export default MyCtrl;
```

```
//Create Controller
MyModule.controller('MyCtrl',MyCtrl);
```       

## component & directive        
```
//Create Controller Class
class MyCtrl
{
    constructor(srv1, srv2, ...)
    {
        ...
    }
    ...
}
MyCtrl.$inject = ['srv1', 'srv2'];

//Create directive class
class MyDirect
{
    constructor()
    {
        this.bindings={...};
        this.template=require('...');
        this.controller=MyCtrl;
    }
}
export default new MyDirect;

//Or
export default{
    bindings:...,
    template:require(...),
    controller:MyCtrl
}
```
          
```
//Create component
import MyCom from '...';
MyModule.component('MyComp',MyComp);
//or
MyModule.directive('MyComp',MyComp);
```    

## service      
```
//Create MySvr Class
class MySvr
{
    constructor(srv1, srv2, ...)
    {
        ...
    }
    ...
}
MySvr.$inject = ['srv1', 'srv2'];
export default MySvr;
```

```
//Create Service
MyModule.service('MySrv',MySrv);
//Or
MyModule.factory('MySrv',()=>{return new MySrv});
```


## filter            
```
...
```

## config       
```
//Create config
const MyConfig=(srv1, srv2)=>{
    
};
MyConfig.$inject=['srv1','srv2'];
```  

```
//Use config
MyModule.config(MyConfig);
```             

## [example](https://github.com/xiong-ang/ES6-AngularJS1.x.git)