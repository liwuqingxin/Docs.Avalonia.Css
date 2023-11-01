# About Acss

## &#x20;Acss

Avalonia Css does not follow the standard of CSS (Cascading Style Sheets). It is used to seperate the structure and style of Avalonia UI. We named it Acss as it has the similar concepts and syntax to CSS.

## Advantages of Acss

With Acss we split Avalonia's axaml into structures and styles. Structure defines the bones of the UI, similar to the role of Html. Styles define the look and feel of the UI, similar to the role of CSS. This has the following benefits:

### 1. Structure Separation.&#x20;

Developers do not need to consider the style of the UI when writing page logic and page functionality, so they can focus on functionality; and when modifying the style, they can also focus on the theme style independently and continuously.&#x20;

{% hint style="success" %}
Continuous focus makes our work smoother, more elegant, and more effective.
{% endhint %}

### 2. Structure reuse.&#x20;

The structure of UI can be reused for different themes without rewriting the structure of the code, which is efficient and safe.&#x20;

{% hint style="info" %}
For a highly reusable structure, see [ru-he-ding-yi-liang-hao-de-kong-jian-mo-ban.md](../zui-jia-shi-jian/ru-he-ding-yi-liang-hao-de-kong-jian-mo-ban.md "mention").
{% endhint %}

### 3. Dynamic loading.&#x20;

Acss supports hot updating (hot reloading) of the code source. The update takes effect immediately.

### 4. Filtering control. (Under development...)&#x20;

Acss can do more complex filters, including property filter, style filter, resource filter, animation filter, theme filter and so on. For example, we can control whether to display animations and transitions according to the specified conditions, and filter the effective styles and resource.

### 5. Behavioral extensions.&#x20;

We have built some general behaviors into Acss. For example, "Esc key to close window" and so on. You can also extend your own behaviors.&#x20;

{% hint style="info" %}
We don't use the Avalonia.Xaml.Interactivity component. You can still use Acss behaviors and the Avalonia behavior library independently without worrying about conflicts between them.
{% endhint %}

### 6. Syntax extensions.&#x20;

Based on Acss, we can extend some syntax that is not supported by axaml, such as style inheritance and cross-type reuse.&#x20;

{% hint style="success" %}
Acss styles can inherit from multiple style templates (multi-inheritance) and can inherit from generic style templates (cross-type inheritance).
{% endhint %}

### 7. Style debugging. (planned...)&#x20;

We plan to provide a style debugging tool to make it easy to navigate through everything related to Acss styles, resources, etc. during development.

### 8. Custom drawing. (planned...)&#x20;

We plan to make Acss support custom drawing operations on any control.
