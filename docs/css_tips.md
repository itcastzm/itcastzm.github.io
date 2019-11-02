## 常用css代码片段

### 实际工作中，让某一行字不换行，多余字便显示省略号的这种需求还是挺多的
```css
 //这行文字内容的宽度
 width:xxx;
 //clip：不显示省略标记（...），而是简单的裁切；ellipsis：当对象内文本溢出时显示省略标记（...）
 text-overflow: ellipsis;
 //禁止文字换行
 white-space: nowrap;
 //溢出内容为隐藏
 overflow: hidden;
```
