# Using Acss

{% hint style="info" %}
**NOTE**

If there is no need for custom extensions, you can use Acss by following the examples on this page.&#x20;

This page provides the most basic examples of Acss usage. For other complex usage or customisation scenarios, please refer to the other chapters within this section.
{% endhint %}

## Installation

```bash
dotnet add package Nlnet.Avalonia.Css --version 1.0.0-beta.4
```

## Registering Acss and Type Resolution

This process can also be deferred until the initialisation is done together.

{% hint style="danger" %}
The current version does not support the use of multiple AcssContexts, only the default AcssContext.Default.
{% endhint %}

```csharp
private static AppBuilder BuildAvaloniaApp()
{
    return AppBuilder.Configure<App>()
        ...
        
        // Use avalonia css stuff.
        .UseAcssDefaultContext()
        
        // Type resolver for 'Your.Lib'. The GenericTypeResolver<TSink> will load all
        // types those belong to the assembly who contains the T class.
        .WithTypeResolverForAcssDefaultContext(new GenericTypeResolver<TSink>())
        ;
}
```

## Initializing AcssContext

You can do this optionally in the Application.Initialize() function. Of course, other timing is not affected.

* Use Acss.&#x20;
* Configuration Parameters.&#x20;
* Registering Types to the Type Interpretation Service.&#x20;
* Create a Rider configuration. If you are using Rider as a development tool.&#x20;
* Load the Acss source.

At this point, Acss is normally in effect.

```csharp
private class void Initialize()
{
    ...
    
    // [Optional] Use default css builder. It has same effect to 
    // AcssExtension.UseAvaloniaCssDefaultBuilder().
    AcssContext.UseDefaultContext();
	
    // [Optional] Set the current accent and other settings.
    var cfg = AcssContext.Default.GetService<IAcssConfiguration>();
    cfg.Accent = "blue";
    cfg.EnableTransitions = true;
    
    // [Optional] Create rider settings for this Acss builder.
    var riderBuilder = AcssContext.Default.GetService<IRiderSettingsBuilder>();
    riderBuilder.TryBuildRiderSettingsForAcss(out _, out _, null);
    
    // Load acss files to Application.Current.Styles. 
    // You can keep the acssFile for more operations.
    var loader = AcssContext.Default.GetService<IAcssLoader>();
    var acssFile = loader.Load(Application.Current.Styles, new FileSource("Acss/Case.acss"));
    
    // Or load acss files from a folder.
    loader.LoadCollection(Application.Current.Styles, new FileSourceCollection("Acss/"));
    
    ...
}
```
