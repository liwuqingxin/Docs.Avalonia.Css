# 样式

## 样式语法

样式的基本语法如下：

<pre class="language-css"><code class="lang-css"><strong>selector {
</strong><strong>    // setter-property: setter-value;        // Use value.
</strong>    // setter-property: var(resource-key);   // Use dynamic resource.
    
    // Sample:
    Background: #0068B5;
    Foreground: var(Accent);
    
    // ...
    
    // Children must be at last of the style.
    [[
        ::animation{
            
        }
        child-style-selector {
            // ...
        }
        child-style-selector2 {
            // ...
        }
    ]]
<strong>}
</strong></code></pre>

{% hint style="info" %}
和 Avalonia 的样式一样，Acss 样式可以嵌套，嵌套的样式放在 Children 当中，其语法为两对中括号 '\[\[ ... ]]'。Children 当中除了可以放子样式，还可以放动画。
{% endhint %}

{% hint style="danger" %}
**特别注意**

Acss 的 Children 必须放在样式的最后，其后的 Setter 将不会生效。
{% endhint %}

## Selector 语法

Acss 的样式选择器（Selector）的语法和 Avalonia 中**完全一致**。唯一区别在于子样式的 Selector 前面不需要添加 “**^**” 符号。

## Setter 语法

Setter 放在样式当中的形式前面已经进行了展示。这里单独说明下其他特征。

* Setter 中的冒号 “:” 前后的**空白字符**<mark style="color:red;">**不**</mark>**会影响**语句效果。
* Setter 的结束符 “;” 是必不可少的，但是集合属性不需要，或者最后一个 Setter 可省略。
* Setter 的 Property 是大小写<mark style="color:red;">**敏感**</mark>的。&#x20;
* Setter 的 Value 可以使用动态资源。
* Setter 的 Value 的解析和资源值解析语法一致。
* Setter 的 Value 可以使用静态实例，使用 “@” 符号标记静态类型，后直接访问静态属性，例如：

```css
// ...
TextDecorations: @TextDecorations.Strikethrough;
// ...
```

{% hint style="danger" %}
需要注意的是，只有在类型解析服务当中注册过的类型，才能被访问。

例如上述代码中的 TextDecorations 在 Acss 内置的类型解析器当中被注册，你可以直接使用。
{% endhint %}

* Setter 的 集合类型的值定义有特殊的语法。例如对于 Transitions 属性，它是一个集合 。

<pre class="language-css"><code class="lang-css"><strong>// 定义了三个 Transition 的集合对象 Transitions，将其赋值给 Transitions 属性。
</strong><strong>// 其中，第一个是直接定义的 Transition，第二第三个是访问的动态资源。
</strong><strong>Transitions:[
</strong>    RenderTransform 0.075 0 LinearEasing;
    var(stRenderTransform);
    var(stBorderBrush);
]
</code></pre>

{% hint style="warning" %}
目前除了 Transitions，暂时没有支持其他集合类型的属性。后续会考虑增加支持。
{% endhint %}

## 样式的分类

Acss 的样式最终还是会生成 Avalonia 样式来产生效果。但是逻辑上我们仍然给它进行了分类。

{% hint style="info" %}
这里讨论的是根样式，不包括样式内嵌套的子样式。
{% endhint %}

### 1. 主题子样式

主题子样式是针对某个类型的 Control 定义的子样式，其样式解析成 Avalonia 样式后，会在当前环境和 Application 范围内查找该类型 Control 的 ControlTheme，并将样式加入到该 ControlTheme 当中作为子样式存在。

这和你直接在 ControlTheme 中定义子样式别无二致。

{% hint style="warning" %}
主题子样式的 Selector 前需要加上 “<mark style="color:red;">**^**</mark>” 符号，它表示该样式是主题子样式，而非普通样式。



**特别注意**

“<mark style="color:red;">**^**</mark>” 符号表示主题子样式，这和 Avalonia 中它的用法并不相同。Avalonia 中它表示嵌套子样式。

:tada::tada::tada: 而在 Acss 中，嵌套子样式的 Selector 前**并不需要**添加这个符号！
{% endhint %}

如下三段代码中，在实际效果上，Code #1 + Code #2 = Code #3。

```xml
<!-- Code #1 -->
<ControlTheme x:Key="{x:Type Button}" TargetType="Button">
    <Setter Property="Button.Template">
        <ControlTemplate TargetType="Button">
            <ContentPresenter x:Name="PART_ContentPresenter"
                              Content="{TemplateBinding Content}"
                              Background="{TemplateBinding Background}" />
        </ControlTemplate>
    </Setter>
</ControlTheme>
```

```css
// Code #2
^Button {
    [[
        :pointerover {
            Background: Red;
        }
    ]]
}
```

```xml
<!-- Code #3 -->
<ControlTheme x:Key="{x:Type Button}" TargetType="Button">
    <Setter Property="Button.Template">
        <ControlTemplate TargetType="Button">
            <ContentPresenter x:Name="PART_ContentPresenter"
                              Content="{TemplateBinding Content}"
                              Background="{TemplateBinding Background}" />
        </ControlTemplate>
    </Setter>
    
    <Style Selector="^:pointerover">
        <Setter Property="Background" Value="Red"/>
    </Style>
</ControlTheme>
```

{% hint style="danger" %}
**生效范围**

从概念上和实际效果来说，主题子样式的**生效范围**在于第一个被搜索到的 ControlTheme 所能应用的范围，而不是该样式所在的 Acss 文件所能应用的范围。
{% endhint %}

### 2. 普通样式

普通样式则是不带 “<mark style="color:red;">**^**</mark>” 符号的 Acss 样式，它会被解析为 Avalonia 样式后，放入 IAcssFile 的 Styles 当中。注意，IAcssFile 继承了 Avalonia 接口 IStyle。而 IAcssFile 则是在被 IAcssLoadr 加载时，放入加载时传入的 Styles 类型的 Owner 当中。

{% hint style="danger" %}
**生效范围**

普通样式的生效范围则是 IAcssFile 的生效范围，IAcssFile 的生效范围则是其 Owner 的生效范围。
{% endhint %}

普通样式的示例如下：

```css
Button:pointerover{
    Background: red;
}
```

### 3. 逻辑子样式（beta）

逻辑子样式不属于 Avalonia 现有支持的功能，它指的是，该样式将会被放入父样式的 Selector 的 Target 的 Styles 当中。

例如，“/template/Grid#PART\_MonthView” 的 Target 是 Grid，逻辑子样式将会被放入 Grid 的 Styles 当中，作为普通的子样式存在。

{% hint style="danger" %}
**特别注意**

逻辑子样式目前属于实验功能，暂不稳定，不推荐使用。
{% endhint %}

逻辑子样式的 Selector 前需添加 “>” 符号，代码示例：

```css
/template/ Grid#PART_MonthView{
    [[
        >TextBlock.Week{
            VerticalAlignment:Center;
            HorizontalAlignment:Center;
            Margin:0,12;
        }
    ]]
}
```

## 样式继承

不同于 Avalonia 中样式只能 BaseOn 一个基样式，Acss 的样式不能直接继承样式，但是可以继承**多个样式模板**。

### 1. 样式模板

抽象模板指的是，不指定 Selector 的样式模板。抽象模板没有指定具体的类型，因此它也不能被解析成具体的 Avalonia 样式。

例如，下面定义了两个样式模板，模板的 Key 分别为 “.ctrl” 和 “.border”。

```css
.ctrl{
    Foreground: var(fore);
    Background: var(ctrl-back);
}

.border{
    BorderBrush:var(ctrl-border);
    BorderThickness: 1;
}

.const-box @extend(.ctrl, .border) { }
```

{% hint style="success" %}
样式模板可以继承样式模板。
{% endhint %}

### 2. 文件导入

继承样式模板的条件是，当前 IAcssFile 环境中，具备该样式模板，有两种方式可以达到目的。

* 在当前 IAcssFile 中定义该模板。注意须定义在最外层，而不能放入任何 Style 的 Children 当中。
* 在独立的文件当中定义该模板，使用 `base` 关键字在文件最前面导入该文件。方法如下：

```css
// Begin of the file.
base ./Bases/Bases.acss;
base ./Bases/Control.acss;

// ...
```

{% hint style="info" %}
**注意**

1. 导入文件的相对目录参考当前 IAcssFile 的所在目录。
2. 可以导入多个文件，一个一行，以 “;” 标记结束。
{% endhint %}

### 3. 继承模板

Acss 的样式可以继承**多个**模板。

<pre class="language-css"><code class="lang-css">/* Style.Template.acss */

// Style template for properties.
.ctrl{
    Foreground: var(fore);
    Background: var(ctrl-back);
}
.border{
    BorderBrush:var(ctrl-border);
    BorderThickness: 1;
}

// Style template for states.
.pointerover{
    [[
        :pointerover{
            Foreground: var(fore-hover);
            Background: var(ctrl-back-hover);
            BorderBrush: var(fluent-ctrl-border-hover);
        }
    ]]
}
.pressed{
    [[
        :pressed{
            Foreground: var(fore-pressed);
            Background: var(ctrl-back-pressed);
            BorderBrush: var(fluent-ctrl-border-pressed);
        }
    ]]
}

<strong>// 样式模板继承模板多继承
</strong>.const-box @extend(.ctrl, .border) { }
.box @extend(.ctrl, .border, .pointerover, .pressed) { }
</code></pre>

使用模板：

```css
/* TextBox.acss */

base ./Bases/Bases.acss;

.disabled{
    [[
        :disabled{
            Opacity:0.4;
        }
    ]]
 }

// 多继承
^TextBox @extend(.box, .disabled){
    // ...
}
```

