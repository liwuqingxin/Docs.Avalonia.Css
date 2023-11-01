# 行为

## 行为语法

关于行为的[定义](../zhu-ti-bang-zhu/ru-he-shi-yong-acss.behaviors.md)，前文已经提到。

行为的使用比较简单，和 Setter 类似，使用行为集的名称作为属性，行为名称作为值即可。例如行为 window.esc.close 属于内置行为集 acss，其使用方式如下：

```css
Window{
    Background: red;
    .acss:window.esc.close;
}
```

如果你自定义了行为集 xbeh，其内定义了行为 window.xxx.xxx，使用方式如下：

```
Window{
    Background: red;
    .acss:window.esc.close;
    .xbeh:window.xxx.xxx;
}
```
