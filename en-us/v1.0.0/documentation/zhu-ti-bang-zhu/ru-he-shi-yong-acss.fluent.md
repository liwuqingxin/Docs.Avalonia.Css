# Using Acss.Fluent

## Installation

```bash
dotnet add package Nlnet.Avalonia.Css.Fluent --version 1.0.0-beta.4
```

## Use of themes

It is possible to simply use the Acss.Fluent theme.

```xml
<Application.Styles>
    <fluent:AcssFluentTheme />
</Application.Styles>
```

{% hint style="info" %}
AcssFluentTheme uses the embedded Avalonia resource as the code source, all Acss code under the theme will be extracted from the resource, which means it cannot be changed.
{% endhint %}

{% hint style="success" %}
The advantage of using embedded code sources is that there is no need to carry more local code files with you when you publish, and both single-file releases are much more friendly to publish.
{% endhint %}

## Use default local resources

AcssFluentTheme supports giving preference to local resources. You can set the UseRecommendedPreferSource property to allow AcssFluentTheme to preferentially load local files with the AbsolutePath of the embedded resource's Uri as the relative path, and the anchor directory of the relative directory is the current application directory.

{% hint style="warning" %}
Note that the current version does not provide an API for controlling the export of embedded code sources, so when using local resources, you need to set the AutoExportSourceToLocal property to true to allow AcssFluentTheme to try to export all embedded code sources locally.
{% endhint %}

```xml
<Application.Styles>
    <fluent:AcssFluentTheme AutoExportSourceToLocal="True" 
                            UseRecommendedPreferSource="True" />
</Application.Styles>
```

## Using custom local resources

If you do not want to use the default local resource path, you can customise the local resource path by setting PreferLocalPath. Code sources set this way will have a higher priority.&#x20;

For example, if we want to use different local directories for the debugging environment and the release environment, e.g., we want to use the source code directory for the debugging environment and the current application directory for the release environment, we can use it this way:

```xml
<Application.Styles>
    <fluent:AcssFluentTheme AutoExportSourceToLocal="True"
                            UseRecommendedPreferSource="True"
                            PreferLocalPath="{x:Static a:Cdt.PreferLocalPath}" />
</Application.Styles>
```

```csharp
public static class Cdt
{
    public static string? PreferLocalPath { get; set; }

    static DebugThing()
    {
#if DEBUG
        PreferLocalPath = "../../src/Nlnet.Avalonia.Css.Fluent/";
#else
        PreferLocalPath = null;
#endif
    }
}
```

In the above code, in DEBUG mode, PreferLocalPath uses "... /... /src/Nlnet.Avalonia.Css.Fluent/" directory, Acss will try to export the embedded code source to this directory when the application is running, the resource directory structure remains unchanged, and it will be loaded from this directory first.&#x20;

In non-DEBUG mode, PreferLocalPath is null, UseRecommendedPreferSource is in effect, and Acss will use the current directory as the directory for exporting and loading.

{% hint style="success" %}
The advantage of prioritising local files is that they can be modified to change the theme style.
{% endhint %}
