# Style

## # Style Syntax

The basic syntax of a style is as follows:

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
Like Avalonia styles, Acss styles can be nested, and nested styles are placed in Children with the syntax of two pairs of parentheses '\[\[ ...]]'. In addition to child styles, children can also have animations.
{% endhint %}

{% hint style="danger" %}
**Attention**&#x20;

Acss Children must be placed at the end of the style. Subsequent Setters will not take effect.
{% endhint %}

## Selector Syntax

The syntax of Acss's Selector is exactly the same as in Avalonia. The only difference is that you don't need to add the "^" symbol in front of the Selector of a sub-style.

## Setter Syntax

The form in which the Setter is placed in the style has already been shown. The other features are described separately here.&#x20;

* The whitespace before and after the colon ":" in a Setter <mark style="color:red;">**does not**</mark> affect the effect of the statement.&#x20;
* The ";" character at the end of a Setter is essential, but is not required for collection properties, or can be omitted from the last Setter.&#x20;
* A Setter's Property is <mark style="color:red;">**case sensitive**</mark>.&#x20;
* A Setter's Value can use dynamic resources.&#x20;
* The parsing syntax of a Setter's Value is consistent with the parsing syntax of resource values.&#x20;
* A Setter's Value can use static instances, using the "@" symbol to mark static types and then accessing static properties directly, for example:

```css
// ...
TextDecorations: @TextDecorations.Strikethrough;
// ...
```

{% hint style="danger" %}
Note that only types registered with the type resolution service can be accessed. For example, the TextDecorations in the above code are registered with the Acss built-in type parser and you can use them directly.
{% endhint %}

* There is a special syntax for defining values for Setter's collection types. For example, for the Transitions property, it is a collection.

```css
// Defines a collection object Transitions with three Transitions assigned to the Transitions property.
// Where the first is a directly defined Transition and the second and third are accessed dynamic resources.
Transitions:[
    RenderTransform 0.075 0 LinearEasing.
    var(stRenderTransform); var(stBorderBrush;)
    var(stBorderBrush); var(stBorderBrush).
]
```

{% hint style="warning" %}
Currently there is no support for properties of collection types other than Transitions. We will consider adding support later.
{% endhint %}

## Classification of styles

Acss styles still end up generating Avalonia styles to produce the effect. But logically we still categorise it.

{% hint style="info" %}
The discussion here is of the root style, not including sub-styles nested within the style.
{% endhint %}

### 1. Theme sub-styles

A theme sub-style is a sub-style defined for a type of Control that, when parsed into an Avalonia style, looks up the ControlTheme for that type of Control in the current environment and Application scope and adds the style to that ControlTheme as a sub-style.&#x20;

This is not unlike when you define a sub-style directly in the ControlTheme.

{% hint style="warning" %}
The theme sub-style Selector needs to be preceded by a "<mark style="color:red;">^</mark>" symbol, which indicates that the style is a theme sub-style, not a normal style.



**NOTE**

The "^" symbol indicates a thematic sub-style, which is not the same as it is used in Avalonia, where it indicates a nested sub-style.&#x20;

:tada::tada::tada: In Acss, the symbol is not needed before the Selector of a nested sub-style!
{% endhint %}

In the following three pieces of code, in practical terms, Code #1 + Code #2 = Code #3.

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
**NOTE**

Conceptually and practically, theme sub-styles take effect to the extent that the first ControlTheme searched for can be applied, not to the extent that the Acss file in which the style is located can be applied.
{% endhint %}

### 2. Normal Style

Normal styles are Acss styles without the "^" symbol, which are parsed as Avalonia styles and put into Styles in IAcssFile. Note that IAcssFile inherits the Avalonia interface IStyle, and IAcssFile is loaded by IAcssLoadr into the Owner of the Styles type passed in during loading.

{% hint style="danger" %}
**NOTE**

The scope of effect of a normal style is the scope of effect of the IAcssFile, and the scope of effect of the IAcssFile is the scope of effect of its owner.
{% endhint %}

An example of the normal style is shown below:

```css
Button:pointerover{
    Background: red;
}
```

### 3. Logic sub-style (beta)

Logical sub-styles are not currently supported by Avalonia, they mean that the style will be placed in the Styles of the Target of the Selector of the parent style.

&#x20;For example, if the Target of "/template/Grid#PART\_MonthView" is a Grid, the logical child style will be placed into the Styles of the Grid as a normal child style.

{% hint style="danger" %}
**NOTE**

Logic sub-style currently belongs to the experimental function, temporarily unstable, not recommended.
{% endhint %}

Logic sub-style of the Selector before the need to add the ">" symbol, code examples:

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

## Style inheritance

Unlike Avalonia where styles can only BaseOn a base style, Acss styles can't directly inherit styles, but they can inherit multiple style templates.

### 1. Style templates

An abstract template is a style template that does not specify a Selector. Abstract templates do not specify a specific type, so they cannot be parsed into specific Avalonia styles.

For example, the following defines two style templates with ".ctrl" and ".border" as their Key.

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
Style templates can inherit style templates.
{% endhint %}

### 2. File Import

Inheriting a style template requires that the style template is available in the current IAcssFile environment, which can be achieved in two ways.&#x20;

* Define the template in the current IAcssFile. Note that it must be defined at the outermost level, not in any of Style's Children.&#x20;
* Define the template in a separate file and import it at the top of the file using the base keyword. The method is as follows:

```css
// Begin of the file.
base ./Bases/Bases.acss;
base ./Bases/Control.acss;

// ...
```

{% hint style="info" %}
**Note**

1. The relative directory of the imported file refers to the current IAcssFile directory. The relative directory of the imported file refers to the directory where the current IAcssFile is located.&#x20;
2. Multiple files can be imported, one line at a time, terminated by a ";" flag.
{% endhint %}

### 3. Inheritance templates

Acss styles can inherit from multiple templates.

```css
/* Style.Template.acss */

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

// Style Template Inheritance Template Multiple Inheritance
.const-box @extend(.ctrl, .border) { }
.box @extend(.ctrl, .border, .pointerover, .pressed) { }
```

Use the template:

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

// Multi-inheritance
^TextBox @extend(.box, .disabled){
    // ...
}
```
