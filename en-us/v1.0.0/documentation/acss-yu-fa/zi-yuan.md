# Resource

## Resource Collection

Resources are defined inside a resource collection. The syntax of a resource collection is as follows:

<pre class="language-css"><code class="lang-css"><strong>/* Normal resources. */
</strong>::res {
    /* Resource here. */
}

/* Resources is available when accent is blue. */
::res[accent=blue] {
    /* Resource here. */
}

/* 
Resource is available when accent is blue. 
And description is "Resources description". 
*/
::res[accent=blue][desc=Resources description] {
    /* Resource here. */
}

/* 
Resource is available when theme is light and accent is blue. 
And description is "Resources description". 
*/
::res[theme=light][accent=blue][desc=Resources description] {
    /* Resource here. */
}
</code></pre>

{% hint style="info" %}
For resource collections, we currently only support the **desc**, **accent** and **theme** attributes. The desc is a normal attribute. The accent and theme are filtered attributes. The valid reference for the accent filter is the configuration parameter in IAcssConfigration, and the valid reference for the theme filter is Avalonia's ThemeVariant.&#x20;



**Note**&#x20;

We plan to provide more filtering attributes for Acss' [filtering control](../cong-zhe-li-kai-shi/guan-yu-acss.md#4.-filtering-control.-under-development...) in the future.
{% endhint %}

## Resource Syntax&#x20;

The syntax of a resource definition takes the following form:

```css
// resource-type(resource-key):value;                // Use value.
// resource-type(resource-key):var(resource-key);    // Use dynamic resource.

// Sample:
color(AccentColor): #0068B5;
brush(Accent): var(AccentColor);
```

## Custom Resources

The custom resource syntax is the same as the built-in resource syntax, and the form is as shown in the [resource syntax](zi-yuan.md#resource-syntax). Custom resources refer to [extended resources](http://127.0.0.1:5000/s/VaQtj4Qpy5KfXXJuKdQp/documentation/zhu-ti-bang-zhu/ru-he-shi-yong-acss/kuo-zhan-zi-yuan).

## Color

Acss supports a wide range of colour expressions, including common colour names, RGB, RGBA, HSL, HSV and more.

```css
::res {
    color(C01): red;                       /* r=f3, g=f4, b=f5 */
    color(C02): #f3f4f5;                   /* r=f3, g=f4, b=f5 */
    color(C03): #f6f3f4f5;                 /* r=f3, g=f4, b=f5, a=f6 */
    color(C04): #345;                      /* r=33, g=44, b=55 */
    color(C05): #f345;                     /* r=33, g=44, b=55, a=ff */
    
    /* Space is not allowed between the comma and value now. */
    color(C06): rgb(13,14,15);             /* r=13, g=14, b=15 */
    color(C07): rgba(13,14,15,16);         /* r=13, g=14, b=15, a=16 */
    color(C08): rgb(13%,14%,15%);          /* r=33, g=36, b=38 */
    color(C09): rgba(13%,14%,15%,16%);     /* r=33, g=36, b=38, a=41 */
    
    /* Hue: 60° (60.000), Saturation: 70% (0.700), Brightness: 50% (0.500) */
    color(C10): hsl(60,70%,50%);
    
    /* Hue: 150° (150.000), Saturation: 100% (1.000), Brightness: 90% (0.900) */
    color(C11): hsv(150,100%,90%);
}
```

Acss supports additional definition of transparency for all expressions of values, which allows us to define colour resources more flexibly. Example:

```css
::res {
    colour(C01): red 50%;             /* red with 50% transparency */
    colour(C02): #f6f3f4f5 30%;       /* final transparency is f6 * 30% */
    colour(C03): hsl(60,70%,50%) 40%; /* hsl value is parsed into colour and 40% transparency is applied */
}
```

{% hint style="danger" %}
The additional transparency of the colour is given as a second parameter, separated from the value expression for the colour by space. **This is the reason why spaces are currently not allowed to exist inside the value expression.**
{% endhint %}

## Brush

Brush supports all value expressions supported by Color, including common colour names, RGB, RGBA, HSL, HSV, etc., as well as additional transparency definitions.

```css
::res {
    brush(B01): red;
    brush(B02): #f3f4f5;
    brush(B03): #f6f3f4f5;
    brush(B04): #345;
    brush(B05): #f345;
    brush(B06): rgb(13,14,15);
    brush(B07): rgba(13,14,15,16);
    brush(B08): rgb(13%,14%,15%);
    brush(B09): rgba(13%,14%,15%,16%);
    brush(B10): hsl(60,70%,50%);
    brush(B11): hsv(150,100%,90%);
    
    brush(B12): red 50%;
    brush(B13): #f6f3f4f5 30%;
    brush(B14): hsl(60,70%,50%) 40%;
}
```

Brushes also support applying dynamic resources to their colour property. Additional transparency property can still be added when applying dynamic resources.

```css
::res {
    color(C01): red;
    brush(B01): var(C01);
    brush(B02): var(C01) 40%;
}
```

## Linear Brush

Currently our support for Linear Brush is not complete. The initial usage is shown in the following code.

{% hint style="danger" %}
Linear Brush's syntax is currently poorly defined and the syntax may be subject to destructive updates.
{% endhint %}

```css
::res {
    linear(LB01): (0% 0% 0% 100%)[
        #ececec 0.8;
        #D2D3D4 1;
    ]
    linear(LB02): (0 0.5 0 1.4)[
        #ececec 0;
        red 0.8;
        var(AccentColor) 0.5 0.9;
    ]
}

/* The LinearBrush defined in the LB01 resource above has StartPoint (0,0) and EndPoint (0,1).
- The LinearBrush defined by the LB01 resource above has a StartPoint of (0,0) and an EndPoint of (0,1).
  It has 2 GradientStop, which are:
      Color = #ececec, Offset = 0.8
      Color = #D2D3D4, Offset = 1
      
- The LinearBrush defined by the LB02 resource above has a StartPoint of (0,0.5) and an EndPoint of (0,1.4).
  It has 3 GradientStop, they are:
      Color = #ececec, Offset = 0
      Color = red, Offset = 0.8
      Color = Key to apply 0.5 transparency to AccentColor's dynamic resources, Offset = 1
*/
```

## Double & Int

```css
::res {
    int(max-length) : 120;
    double(button-height) : 30;
    double(scale-y) : 40%;
}
```

## Thickness

```css
::res {
    thick(button-border-thickness) : 2;
    thick(button-margin) : 2,2,2,2;
    margin(button-margin) : 2;
    padding(button-padding) : 2,2,2,2;
    thickness(button-padding) : 2,2,2,2;
}
```

## Transition

Acss supports defining Transition resources for easy reuse, which Avalonia does not have. Avalonia supports defining Transitions resources, which we don't support at the moment.

{% hint style="danger" %}
Transition's current syntax is poorly defined and the syntax may be subject to destructive updates.
{% endhint %}

```css
::res {
    double(duration): 0.125;
    double(delay): 0.125;

    /* TargetType.Property | Duration | [Delay] | [Ease] */
    transition(trans01):
        Border.BorderThickness 0.2);
    transition(trans02):
        Border.BorderThickness 0.2 var(delay);
    transition(trans03):
        Border.BorderThickness var(duration) 0.4 QuadraticEaseOut);
    transition(trans04):
        Border.BorderThickness var(duration) 0.4 0,0,1,1);
}
```

{% hint style="success" %}
The last parameter of the easing function \[Ease] supports the use of KeySpine to define the easing function, e.g. '.17,.67,.83,.67', which represents the Bezier curve generated by using the points (0, 0) and (1, 1) as the start point, (0.17, 0.67) and (0.83, 0.67) as the control points. See here for KeySpine usage (external link).
{% endhint %}

{% hint style="warning" %}
The last optional parameter of the Transition resource, \[Ease], also supports dynamic resources (var), but currently we don't support resources that define an Easing Function, so it doesn't actually work with dynamic resources at the moment.
{% endhint %}

## BoxShadows

We have imperfect support for shadow resources for the time being, and only support the following forms of definitions.

{% hint style="danger" %}
BoxShadows syntax is currently poorly defined and the syntax may be subject to destructive updates.
{% endhint %}

```css
::res[theme=light][desc=Light Resources] {
    /* x offset | y offset | shadow blur radius | shadow spread radius | shadow colour */
    BoxShadow(popup-none): 0 0 0 0 0 0 #3666;
    BoxShadow(popup-shadow): 0 10 20 0 0 #3666;
}
```

