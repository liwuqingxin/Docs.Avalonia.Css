# Extending Resources

Acss's resources (AcssResource) support extensions. Currently we have some common resource types built-in, and you can also define your own resource types.

## Built-in resource types

1. Brush
2. Color
3. Double
4. Int
5. LinearBrush
6. Thickness
7. Transition
8. BoxShadows

{% hint style="success" %}
More built-in resource types will be added later in the programme.
{% endhint %}

## Custom Resource Types

Acss's resources all inherit from the abstract class AcssResourceBaseAndFac, defined as follows:

```csharp
public abstract class AcssResourceBaseAndFac<T> : AcssResource, IResourceFactory 
    where T : AcssResource, new()
```

You can inherit this class to extend your own resource types, for example in the following code we extend the resource type that defines Thickness.&#x20;

The aliases for this type are "Thickness", "Thick", "StrokeThickness", " Margin", "Padding". Any of these aliases will be resolved to a Thickness resource in Acss code.

{% hint style="warning" %}
Note that aliases are <mark style="color:red;">**not case insensitive**</mark>. thick and thick refer to the same resource type, Thickness.
{% endhint %}

```csharp
[ResourceType(nameof(Thickness))]
[ResourceType("Thick")]
[ResourceType("StrokeThickness")]
[ResourceType("Margin")]
[ResourceType("Padding")]
internal class ThicknessResource : AcssResourceBaseAndFac<ThicknessResource>
{
    protected override object? BuildValue(IAcssContext context, string valueString)
    {
        return valueString.TryParseThickness();
    }
}
```
