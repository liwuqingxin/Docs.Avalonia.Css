# Using MessageBox

## Installation

```bash
dotnet add package Nlnet.Avalonia.MessageBox --version 1.0.0-beta.4
```

## Using standalone styled MessageBox

* Import resource.

```xml
<ResourceInclude Source="avares://Nlnet.Avalonia.MessageBox/Assets/Themes.axaml" />
```

* Synchronised use.

```csharp
// WPF Standard: call messagebox synchronous.
private void OnClick(object? sender, RoutedEventArgs e)
{
    var result = MessageBox.Show("Hello, this is Nlnet MessageBox!", 
        "Welcome", Buttons.OkCancel, Images.Info);
    
    TbxResult.Text = result.ToString();
}
```

* Asynchronous use.

```csharp
// Avalonia Standard: call messagebox asynchronous.
private async void OnClick(object? sender, RoutedEventArgs e)
{
    var result  = await MessageBox.ShowAsync("Hello, this is Nlnet MessageBox :)", 
         "Welcome", Buttons.OkCancel, Images.Info);
    
    TbxResult.Text = result.ToString();
}
```

## Using Acss-based MessageBox

The difference with standalone MessageBox styles is that Acss-based styles load different style resources. There is no difference in the way it is called.

```xml
<ResourceInclude 
    Source="avares://Nlnet.Avalonia.MessageBox/Assets/Themes.Acss.axaml" />
```

## Using custom styles

You can still customise the theme and style of the MessageBox, in the same way as overriding the theme style of other Avalonia controls/windows.

## Samples

{% hint style="success" %}
We provide standalone MessageBox example code, visit it on [Github](https://github.com/liwuqingxin/Avalonia.Css/tree/main/samples/Nlnet.Avalonia.MessageBox.Samples).
{% endhint %}
