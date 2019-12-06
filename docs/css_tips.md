# 常用css代码片段

## 实际工作中，让某一行字不换行，多余字便显示省略号的这种需求还是挺多的
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

## position(定位) display(框盒) float(浮动)解析(TODO)

> 参考
[CSS布局 ——从displayposition float属性谈起](https://www.cnblogs.com/dolphinX/archive/2012/10/13/2722501.html)
[Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)
[Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)


## 包含块（containing block)
[css规范2.2 10.1节对包含块的定义](https://www.w3.org/TR/CSS22/visudet.html#containing-block-details)
包含块（Containing Block）是视觉格式化模型的一个重要概念，它与框模型类似，也可以理解为一个矩形，而这个矩形的作用是为它里面包含的元素提供一个参考，元素的尺寸和位置的计算往往是由该元素所在的包含块决定的。
包含块简单说就是定位参考框，或者定位坐标参考系，元素一旦定义了定位显示（相对、绝对、固定）都具有包含块性质，它所包含的定位元素都将以该包含块为坐标系进行定位和调整。

用户代理（比如浏览器）选择根元素作为 containing block（称之为初始 containing block）。
对于其它元素，除非元素使用的是绝对位置，containing block 由最近的块级祖先元素盒子的内容边界组成。
如果元素有属性 'position:fixed'，containing block 由视口建立。
如果元素有属性 'position:absolute'，containing block 由最近的 position 不是 static 的祖先建立，按下面的步骤：
    如果 direction 是 ltr（左到右），祖先产生的第一个盒子的上、左内容边界是 containing block 的上方和左方，祖先的最后一个盒子的下、右内容边界是 containing block 的下方和右方。
    如果 direction 是 rtl（右到左），祖先产生的第一个盒子的上、右内容边界是 containing block 的上方和右方，祖先的最后一个盒子的下、左内容边界是 containing block 的下方和左方。
    如果祖先是块级元素，containing block 由祖先的 padding edge 形成。
    如果祖先是内联元素，containing block 取决于祖先的 direction 属性。
    如果没有祖先，根元素盒子的内容边界确定为 containing block。
> 参考
[你不一定知道的css知识——包含块](https://www.jianshu.com/p/ac7771ea1e9e)
