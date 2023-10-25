# 如何使用 Acss.Controls

* 安装依赖库。

```bash
dotnet add package Nlnet.Avalonia.Css.Controls --version 1.0.0-beta.4
```

* 引用资源和样式。

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

* 可选扩展。
  * TemplatedControlExtension 使用后，会对所有 ControlTemplate 内部的控件加上 `:is-part` 伪类。
  * SelectionDetailExtension 使用后，会对所有 SelectingItemsControl 对象加上切换选中项的具体状态伪类，包括&#x20;
    * `:sel-detail-leave-smaller`
    * `:sel-detail-leave-lager`
    * `:sel-detail-enter-lager`
    * `:sel-detail-enter-lager`

```csharp
TemplatedControlExtension.Use();
SelectionDetailExtension.Use();
```

* 使用 Acss 定义自己的样式库。&#x20;

{% hint style="info" %}
建议：自定义样式库中加上主题对象，参考 Nlnet.Avalonia.Css.Fluent 中的 AcssFluentTheme。
{% endhint %}
