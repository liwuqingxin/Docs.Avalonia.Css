# Code Source

ISource is the source of Acss source code. Currently we provide two types of code sources, local code sources (FileSource) and embedded code sources (EmbeddedSource).

{% hint style="info" %}
Acss code files are suffixed with lowercase acss, independent of the type of code source.
{% endhint %}

## Local Code Source

Local code sources use local files as code input. We support two loading methods, load by file or load by directory.

### 1. Loading by document

```csharp
var loader = AcssContext.Default.GetService<IAcssLoader>();

var source1 = new FileSource("Acss/Case.acss");
loader.Load(Application.Current.Styles, source1);

// Use prefer path $"{debugRelative}Acss/Case.acss" if it is valid. 
// Or use the path "Acss/Case.acss".
var source2 = new FileSource("Acss/Case.acss", $"{debugRelative}Acss/Case.acss");
loader.Load(Application.Current.Styles, source2);
```

### 2. Load from folder

```csharp
var loader = AcssContext.Default.GetService<IAcssLoader>();

loader.LoadCollection(this, new FileSourceCollection("Acss/"));
loader.LoadCollection(this, new FileSourceCollection("Acss/", "../../Acss/"));
```

{% hint style="success" %}
Priority paths are supported whether loading by file or by directory. Priority paths are used first when they are valid. This is useful in a debugging environment.&#x20;

We can use the directory where the source code is located first, and the directory under the current application as a second choice.
{% endhint %}

{% hint style="info" %}
Note that loading by directory does not recurse through directories, it only loads all \*.acss files in the current directory of the specified directory.
{% endhint %}

## Embedded Code Source

We also support embedding code sources into applications as Avalonia resources; see [How to use Acss.Fluent](../ru-he-shi-yong-acss.fluent.md) for the usage of the three EmbeddedSource parameters.

{% hint style="success" %}
The embedded resource approach is much more friendly to the way individual files are packaged.
{% endhint %}

### 1. Single Uri loading

```csharp
var loader = AcssContext.Default.GetService<IAcssLoader>();

loader.Load(
    Application.Current.Styles, 
    new EmbeddedSource(
        new Uri("avares://Nlnet.Avalonia.Css.Fluent/Acss/AccentColor.acss"), 
        PreferLocalPath, 
        UseRecommendedPreferSource, 
        AutoExportSourceToLocal));
```

### 2. Batch Load by Uri

```csharp
var loader = AcssContext.Default.GetService<IAcssLoader>();

loader.LoadCollection(
    Application.Current.Styles, 
    new EmbeddedSourceCollection(
        new Uri("avares://Nlnet.Avalonia.Css.Fluent/Acss/"), 
        PreferLocalPath, 
        UseRecommendedPreferSource, 
        AutoExportSourceToLocal));
```
