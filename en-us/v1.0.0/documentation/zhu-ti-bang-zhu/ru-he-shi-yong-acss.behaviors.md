# Using Acss.Behaviors

As examples, we provide two built-in behaviours in the current version. Please refer to the [behavior](../acss-yu-fa/hang-wei.md) for details on how to use them.

* **combobox.popup.align**. This behaviour aligns the selected items of a drop-down list to a drop-down box.&#x20;
* **window.esc.close**. This behaviour allows the window to support the corresponding Esc key to close the window.

{% hint style="success" %}
More inbuilt behaviours are being planned. If you have any requests for this, feel free to let us know [here](https://github.com/liwuqingxin/Avalonia.Css/issues/new).
{% endhint %}

## Installation

```bash
dotnet add package Nlnet.Avalonia.Css.Behaviors --version 1.0.0-beta.4
```

## Custom behaviours

* Import Nlnet.Avalonia.Css.CompileGenerator.

```bash
dotnet add package Nlnet.Avalonia.Css.CompileGenerator --version 1.0.0-beta.4
```

* Creates a behaviour declaration class, inheriting the AvaloniaObject class and the IBehaviorDeclarer interface.

```csharp
public partial class CustomA : AvaloniaObject, IBehaviorDeclarer
{
        
}
```

* Provides extension classes that use the behaviour declaration class CustomA.

```csharp
/// <summary>
/// Use customA behavior feature for default css context.
/// </summary>
/// <param name="builder"></param>
/// <returns></returns>
public static AppBuilder UseCustomABehaviorForDefaultContext(this AppBuilder builder)
{
    AcssBuilder.Default.BehaviorResolverManager.LoadResolver(new GenericBehaviorResolver<CustomA>());
    AcssBuilder.Default.BehaviorDeclarerManager.RegisterDeclarer<CustomA>(nameof(CustomA).ToLower());
    AcssBuilder.Default.BehaviorDeclarerManager.RegisterDeclarer<CustomA>(nameof(CustomA));
    return builder;
}
```

* Create a custom behaviour class that hangs under the behaviour declaration class CustomA.

```csharp
[Behavior("window.esc.close", typeof(CustomA))]
public class WindowEscCloseBehavior: AcssBehavior<WindowEscCloseBehavior>
{
    protected override void OnAttached(AvaloniaObject target)
    {
        if (target is not Window window)
        {
            return;
        }

        window.KeyDown -= WindowOnKeyDown;
        window.KeyDown += WindowOnKeyDown;
    }
    
    protected override void OnDetached(AvaloniaObject target)
    {
        if (target is not Window window)
        {
            return;
        }

        window.KeyDown -= WindowOnKeyDown;
    }
    
    private static void WindowOnKeyDown(object? sender, KeyEventArgs e)
    {
        if (sender is not Window window)
        {
            return;
        }
        if (e.Key == Key.Escape)
        {
            window.Close();
        }
    }
}
```

* Use the CustomA behaviour to declare classes in your program.

```csharp
private static AppBuilder BuildAvaloniaApp()
{
    return AppBuilder.Configure<App>()
        .UsePlatformDetect()

        // Use avalonia css stuff.
        .UseAcssDefaultBuilder()

        // [Optional] Use acss behavior.
        .UseAcssBehaviorForDefaultBuilder();
    	
    	// [Optional] Use customA behavior.
    	.UseCustomABehaviorForDefaultBulder();
}
```

* Using behavioural classes in Acss code.

```css
ComboBoxPage ComboBox#C1{
    .acss:combobox.popup.align;
}

NtWindow#RootWindow{
    .acss:window.esc.close;
}
```
