# Type Resolution

The service for type resolution is called ITypeResolverManager. It manages multiple Acss built-in and user-registered type resolvers (ITypeResolver).

In addition to the built-in type parsing of Acss, you need to register all the used types into AcssContext by yourself. Unregistered types will not be recognised and will be skipped. We provide several ways to register types for parsing.

{% hint style="info" %}
Acss has built-in parsers for all AvaloniaObject subclass types in the Avalonia.Controls and Avalonia.Base assemblies, as well as for some of the necessary types.
{% endhint %}

{% hint style="danger" %}
**NOTE**&#x20;

Currently we don't do namespace mapping, so there is a higher chance of type renaming. If this is the case, please manage your own aliases for the renamed types to avoid conflicts.
{% endhint %}

## Assembly Type Resolver

We provide a very handy assembly type resolver GenericTypeResolver. It can add all the subclasses of the AvaloniaObject type in its assembly to the resolver by the anchored type. For example:

```csharp
// Add all types, which are derived from AvaloniaObject, in the Avalonia.Controls.dll
// to the type resolver.
var resolver = new GenericTypeResolver<Button>();
```

## Custom Type Resolver

Users can inherit from the default implementation class Resolver, or implement the type parser interface IResolver to define a type parser. Example:

```csharp
// #1 Use Resolver.
public class CustomTypeResolver1 : Resolver
{
    // Nothing to do.
}

// #2 Implement IResolver.
public class CustomTypeResolver2 : IResolver
{
    protected Dictionary<string, Type> Types;

    protected Resolver()
    {
        Types = new Dictionary<string, Type>();
    }

    public bool TryAddType(string name, Type type)
    {
        if (Types.ContainsKey(name))
        {
            return false;
        }

        Types.Add(name, type);

        return true;
    }

    public bool TryAddType<T>(string name)
    {
        if (Types.ContainsKey(name))
        {
            return false;
        }

        Types.Add(name, typeof(T));

        return true;
    }

    public bool TryGetType(string name, out Type? type)
    {
        return Types.TryGetValue(name, out type);
    }

    public IEnumerable<Type> GetAllTypes()
    {
        return Types.Values;
    }
}
```

## Adding Types to the Resolver Service

Once you have a type parser, you can add types to that parser.

```csharp
IResolver resolver = new CustomTypeResolver1();

// Map 'Button' to Button.
resolver.TryAddType("Button", typeof(Button));

// Map 'TextBlock' to TextBlock.
resolver.TryAddType<TextBlock>("TextBlock");

// map 'text' to TextBlock.
resolver.TryAddType<TextBlock>("text");
```

{% hint style="info" %}
**NOTE**&#x20;

* A type can be registered with multiple aliases to simplify its use. For example, in the above code, both 'text' and 'TextBlock' can refer to TextBlock.&#x20;
* Acss type parsing is case sensitive.
{% endhint %}

## Single Type

In the current version, we don't support simple registration of individual types, you have to use ITypeResolver, and we'll consider providing a simpler type registration API later.

## Registering a type parser to Managed Services

As shown in the code below.

```csharp
var typeResolverManager = AcssContext.Default.GetService<ITypeResolverManager>();

IResolver resolver = new CustomTypeResolver1();

typeResolverManager.LoadResolver(resolver);
typeResolverManager.LoadResolver(new GenericTypeResolver<App>());
```

## Registering types when building Avalonia services

As shown in the code, after adding the default AcssContext, you can add a type parser directly against the assembly.

```csharp
private static AppBuilder BuildAvaloniaApp()
{
    return AppBuilder.Configure<App>()
        .UsePlatformDetect()

        // Use avalonia css stuff.
        .UseAcssDefaultContext()
        
        // Type resolver for Nlnet.Avalonia.Svg
        .WithTypeResolverForAcssDefaultBuilder(new GenericTypeResolver<Icon>())
        
        // Type resolver for Nlnet.Avalonia.SampleAssistant
        .WithTypeResolverForAcssDefaultBuilder(new GenericTypeResolver<Case>());
}
```
