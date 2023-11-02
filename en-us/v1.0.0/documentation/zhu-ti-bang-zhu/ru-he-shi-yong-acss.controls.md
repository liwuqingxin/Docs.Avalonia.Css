# Using Acss.Controls

## Installation

```bash
dotnet add package Nlnet.Avalonia.Css.Controls --version 1.0.0-beta.4
```

## Import resources and styles

```xml
<Styles.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceInclude Source="avares://Nlnet.Avalonia.Css.Controls/Assets/Themes.Acss.axaml" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Styles.Resources>

<StyleInclude Source="avares://Nlnet.Avalonia.Css.Controls/Assets/Styles.Acss.axaml" />
```

## Optional extensions

* TemplatedControlExtension, when used, adds the :is-part pseudo-class to all controls inside the ControlTemplate.
* SelectionDetailExtension will add state specific pseudo-classes for toggling selected items to all SelectingItemsControl objects.
  * `:sel-detail-leave-smaller`
  * `:sel-detail-leave-lager`
  * `:sel-detail-enter-lager`
  * `:sel-detail-enter-lager`

```csharp
TemplatedControlExtension.Use();
SelectionDetailExtension.Use();
```

## Define your own style library with Acss

Nlnet.Avalonia.Css.Controls defines a clean set of control templates for all of Avalonia's native controls, with friendly support for using the Acss Extended Style Library.

{% hint style="info" %}
It is planned to provide debugging tools to support quick look at the structure of the control library. Currently, you can only see the internal structure and some classes, names, etc. by looking at the source code.
{% endhint %}

{% hint style="danger" %}
**Best Practice**

Please note that when you use Acss code, please try to put it under a path with a name specific to you, to avoid it being placed in the same directory as Acss files from other libraries when they co-exist, which can lead to file conflicts or incorrect loading.

For example, the base path of the Acss code file for the Nlnet.Avalonia.Css.Fluent library is:

> "/Acss/Nlnet.Avalonia.Css.Fluent/"
{% endhint %}
