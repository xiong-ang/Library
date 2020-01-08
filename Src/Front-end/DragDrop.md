# DragDrop Skills

## DragDrop Events and Basic    
### 在拖动目标上触发事件 (源元素):     
    ondragstart - 用户开始拖动元素时触发     
    ondrag - 元素正在拖动时触发      
    ondragend - 用户完成元素拖动后触发     
### 释放目标时触发的事件:       
    ondragenter - 当被鼠标拖动的对象进入其容器范围内时触发此事件      
    ondragover - 当某被拖动的对象在另一对象容器范围内拖动时触发此事件       
    ondragleave - 当被鼠标拖动的对象离开其容器范围内时触发此事件        
    ondrop - 在一个拖动过程中，释放鼠标键时触发此事件     
[Reference](https://www.cnblogs.com/moqiutao/p/6365113.html)     

## Important Methods
1. ev.stopPropagation(); 阻止子元素向父元素的事件冒泡    
2. ev.preventDefault(); 阻止浏览器默认的Dragdrop样式    
3. ev.stopImmediatePropagation(); 阻止父元素向子元素的冒泡    

## A method to handle dragdrop   
1. 增加一个Overlay层，当任何元素触发DragEnter时，显示Overlay层
2. 之后所有的DragDrop事件响应基于Overlay来做

## [DragDrop to upload file](https://segmentfault.com/a/1190000013298317)   