# css flex属性

让所有弹性盒模型对象的子元素都有相同的长度，忽略它们内部的内容：

```
#main div
{
flex:1;
}
```

flex 属性用于设置或检索弹性盒模型对象的子元素如何分配空间。

flex 属性是 flex-grow、flex-shrink 和 flex-basis 属性的简写属性。

**注意：**如果元素不是弹性盒模型对象的子元素，则 flex 属性不起作用。

| 默认值：           | 0 1 auto                                 |
| -------------- | ---------------------------------------- |
| 继承：            | 否                                        |
| 可动画化：          | 是，*参见个别的属性*。请参阅 [*可动画化（animatable）*](http://www.runoob.com/cssref/css-animatable.html)。 |
| 版本：            | CSS3                                     |
| JavaScript 语法： | *object*.style.flex="1"                  |

## CSS 语法

flex: *flex-grow* *flex-shrink* *flex-basis*|auto|initial|inherit;