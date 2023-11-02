# Configuration

Acss supports some configurations, which are placed in the configuration service (IAcssConfiguration). You can read and write the configuration through AcssContext.

```csharp
var cfg = AcssContext.Default.GetService<IAcssConfiguration>();

cfg.Accent = "green";
cfg.EnableTransitions = true;
```

{% hint style="info" %}
Acss Configuration currently only supports setting the theme colour (Accent), whether to enable transitions (EnableTransitions) and the theme. The first two are defined in IAcssConfiguration. The last one, Theme, is affected by Avalonia's ThemeVariant.&#x20;

Other configurations to come, feel free to provide new scenarios and requirements [here](https://github.com/liwuqingxin/Avalonia.Css/issues).
{% endhint %}
