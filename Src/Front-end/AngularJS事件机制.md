# AngularJS事件机制        
<!-- TOC -->

- [AngularJS事件机制](#angularjs事件机制)
    - [$emit](#emit)
    - [$broadcast](#broadcast)
    - [$on](#on)
    - [Example](#example)

<!-- /TOC -->
> AngularJS提供了$emit, $broadcast和$on服务用于控制器之间基础事件的传递交流          

![](https://github.com/xiong-ang/Library/blob/master/Pic/event.png?raw=true)              

## $emit       
该服务贯穿作用域发出一个向上的事件，并通知哪些注册在rootScope.Scope上的监听器。该事件的生命周期开始于emit被启动的地方，事件一直朝着根作用域传递，传递期间并通知哪些注册在作用域上的监听器。       
## $broadcast       
该服务发布一个向下的事件给作用域中的所有子节点以及以下的节点，并通知注册在rootScope.Scope上的监听器，该事件的生命周期也是从broadcast被启动开始。下面的所有作用域都会接收到通知。
## $on         
该服务监听指定类型的事件，获取从emit或者broadcast发布的事件。          
$on的方法中的event事件参数:  
|属性或事件|作用|    
|:-:|:-:|    
|event.targetScope| 发出或者传播原始事件的作用域|    
|event.currentScope| 目前正在处理的事件的作用域|   
|event.name|事件名称|
|event.defaultPrevented| 如果调用了preventDefault则为true|
|event.preventDefault()| 这个方法实际上不会做什么事，但是会设置defaultPrevented为true。直到事件监听器的实现者采取行动之前它才会检查defaultPrevented的值|
|event.stopPropagation()|一个防止事件进一步传播(冒泡/捕获)的函数(这只适用于使用$emit发出的事件)|

## Example               
```
<!DOCTYPE html>
<html>
<head>
    <title>Event</title>
    <script src="https://cdn.bootcss.com/angular.js/1.5.11/angular.min.js"></script>
    <script>
        var app = angular.module('app', []);

        app.controller("p_parent", function ($scope) {
            $scope.$on('eventName', function (event, args) {
                $scope.message = args.message;
            });
        });
        app.controller("parent", function ($scope) {
            $scope.$on('eventName', function (event, args) {
                $scope.message = args.message;
            });
        });
        
        app.controller("msgSender", function ($scope) {
            $scope.handleClick = function (msg) {
                $scope.$emit('eventName', { message: msg });
                $scope.$broadcast('eventName', { message: msg });
            };
        });

        app.controller("child", function ($scope) {
            $scope.$on('eventName', function (event, args) {
                $scope.message = args.message;
            });
        });

        app.controller("c_child", function ($scope) {
            $scope.$on('eventName', function (event, args) {
                $scope.message = args.message;
            });
        });
    </script>
</head>

<body ng-app="app">
    <div ng-controller="p_parent" style="border:2px solid #E75D5C; padding:5px;">
        <h1>P_Parent</h1>
        <p>Message : {{message}} </p>
        <br />

        <div ng-controller="parent" style="border:2px solid #428bca;padding:5px;">
            <h1>Parent</h1>
            <p>Message : {{message}} </p>
            <br />

            <div ng-controller="msgSender" style="border:2px solid rgb(22, 22, 22); padding:5px;">
                <h1>Message Sender</h1>
                <input ng-model="msg">
                <button ng-click="handleClick(msg);">Send</button>
                <br />
                <br />

                <div ng-controller="child" style="border:2px solid #660fad;padding:5px;">
                    <h1>Child</h1>
                    <p>Message : {{message}} </p>
                    <br />

                    <div ng-controller="c_child" style="border:2px solid rgb(18, 214, 45); padding:5px;">
                        <h1>C_Child</h1>
                        <p>Message :{{message}} </p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</body>
</html>
```