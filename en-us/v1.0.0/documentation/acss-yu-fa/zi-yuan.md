# resource

## Resource collection syntax

Resources are defined in resource collections. The syntax for a resource collection is as follows:

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
For resource collections, we currently only support three attributes: "description" (desc), "theme color" (accent) and "theme" (theme). Among them, desc is a common attribute, accent and theme are filtering attributes. The effective reference value for accent filtering is in IAcssConfigration. Please refer to [Configuration Parameters](../zhu-ti-bang-zhu/ru-he-shi-yong-acss/ pei-zhi-can-shu.md), the valid reference value for theme filtering is Avalonia's ThemeVariant.



**Special Note**

We later plan to [Filter Control] for Acss(../cong-zhe-li-kai-shi/guan-yu-acss.md#4.-guo-lv-kong-zhi-kai-fa-zhong- ...) feature, providing more filtering properties.
{% endhint %}

## Resource syntax

The syntax for resource definition is as follows:

```css
// resource-type(resource-key):value; // Use value.
// resource-type(resource-key):var(resource-key); // Use dynamic resource.

// Sample:
color(AccentColor): #0068B5;
brush(Accent): var(AccentColor);
```

## Custom resources

The custom resource syntax is consistent with the built-in resource syntax, and the format is also as shown in [Resource Syntax](zi-yuan.md#zi-yuan-yu-fa). Custom resource reference [Extended Resources](../zhu-ti-bang-zhu/ru-he-shi-yong-acss/kuo-zhan-zi-yuan.md).

## Color

Acss supports multiple color expressions, including common color names, RGB, RGBA, HSL, HSV, etc.

```css
::res {
     color(C01): red; /* r=f3, g=f4, b=f5 */
     color(C02): #f3f4f5; /* r=f3, g=f4, b=f5 */
     color(C03): #f6f3f4f5; /* r=f3, g=f4, b=f5, a=f6 */
     color(C04): #345; /* r=33, g=44, b=55 */
     color(C05): #f345; /* r=33, g=44, b=55, a=ff */
    
     /* Space is not allowed between the comma and value now. */
     color(C06): rgb(13,14,15); /* r=13, g=14, b=15 */
     color(C07): rgba(13,14,15,16); /* r=13, g=14, b=15, a=16 */
     color(C08): rgb(13%,14%,15%); /* r=33, g=36, b=38 */
     color(C09): rgba(13%,14%,15%,16%); /* r=33, g=36, b=38, a=41 */
    
     /* Hue: 60° (60.000), Saturation: 70% (0.700), Brightness: 50% (0.500) */
     color(C10): hsl(60,70%,50%);
    
     /* Hue: 150° (150.000), Saturation: 100% (1.000), Lightness: 90% (0.900) */
     color(C11): hsv(150,100%,90%);
}
```

Acss supports additional definition of transparency for all value expressions, which allows us to define color resources more flexibly. For example:

```css
::res {
     color(C01): red 50%; /* 50% transparency on the red band */
     color(C02): #f6f3f4f5 30%; /* The final transparency is f6 * 30% */
     color(C03): hsl(60,70%,50%) 40%; /* After parsing the hsl value into Color, apply 40% transparency */
}
```

{% hint style="danger" %}
The color's additional transparency as the second argument, separated from the color's value expression by a space. This is also the reason why spaces are currently not allowed inside value expressions.
{% endhint %}

## Brush

Brush supports all value expressions supported by Color, including common color names, RGB, RGBA, HSL, HSV, etc., as well as additional transparency definitions.

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

At the same time, brushes support the application of dynamic resources to their Color properties. It is still possible to add additional transparency properties when applying dynamic resources.

```css
::res {
     color(C01): red;
     brush(B01): var(C01);
     brush(B02): var(C01) 40%;
}
```

## Linear Brush

Our support for Linear Brush is currently incomplete. The initial usage is shown in the following code.

{% hint style="danger" %}
The current syntax of Linear Brush is not well defined, and destructive updates to the syntax may occur.
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
         #D2D3D4 1;
     ]
}

/*
- The StartPoint of the LinearBrush defined by the LB01 resource above is (0,0) and the EndPoint is (0,1).
   It has 2 GradientStops, namely:
       Color = #ececec, Offset = 0.8
       Color = #D2D3D4, Offset = 1
      
- The LB02 resource defined above has a StartPoint of (0,0.5) and an EndPoint of (0,1.4) for the LinearBrush.
   It has 3 GradientStops, namely:
       Color=#ececec,Offset=0
       Color = red, Offset = 0.8
       Color = #D2D3D4, Offset = 1
*/
```

## Double & Int

```css
::res {
     int(max-length): 120;
     double(button-height): 30;
     double(scale-y): 40%;
}
```

## Thickness

```css
::res {
     thick(button-border-thickness) : 2;
     thick(button-margin) : 2,2,2,2;
     margin(button-margin) : 2;
     padding(button-padding): 2,2,2,2;
     thickness(button-padding) : 2,2,2,2;
}
```

## Transition

Acss supports defining Transition resources for easy reuse. This is something Avalonia itself does not have. Avalonia supports defining Transitions resources, but we do not support it yet.

{% hint style="danger" %}
Transition's current syntax is poorly defined, and destructive updates to the syntax may occur.
{% endhint %}

```css
::res {
     double(duration): 0.125;
     double(delay): 0.125;

     /* TargetType | Property | Duration | [Delay] | [Ease] */
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

{% hint style="warning" %}
The last optional parameter \[Ease] of the Transition resource also supports dynamic resources (var), but currently we do not support resources that define easing functions (Easing Function), so it is actually currently unable to use dynamic resources.
{% endhint %}

## BoxShadows

Our support for shadow resources is currently incomplete and only supports the following definition form.

{% hint style="danger" %}
BoxShadows' syntax is currently poorly defined, and destructive updates to the syntax may occur.
{% endhint %}

```css
::res[theme=light][desc=light resource] {
     /* x offset | y offset | shadow blur radius | shadow diffusion radius | shadow color */
     BoxShadow(popup-none): 0 0 0 0 #3666;
     BoxShadow(popup-shadow): 0 10 20 0 #3666;
}
```