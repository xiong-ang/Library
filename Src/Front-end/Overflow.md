# Show ... When Overflow

## Single line case
```css
text-overflow: ellipsis;
overflow: hidden;
```
## Multiple lines case   
```css
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 2; //Specify the line count
-webkit-box-orient: vertical;
```