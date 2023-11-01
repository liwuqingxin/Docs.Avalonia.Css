---
description: Acss Editor
---

# Configuring the development environment

Acss is an interpreted language and supports editing it using any text editor. Most simply, you can edit it in Notepad. However, we recommend using an IDE for a better authoring experience.

## Using Rider

{% hint style="warning" %}
Due to the heavy development workload, the Acss language support plugin for Rider is not available at this time, and it is uncertain when it will be available. If you are interested and would like to join our development efforts, you are more than welcome to contact us at yangqi1990917@163.com.
{% endhint %}

For now, for Rider, we provide rudimentary usage. We need to execute the following code after the IAcssContext has been built to generate a Rider configuration file for the current environment. We will try to place it automatically in the Rider installation directory. If the execution fails, please check for exceptions via parameters and handle the creation of the configuration file yourself.

{% code lineNumbers="true" %}
```csharp
var riderBuilder = AcssContext.Default.GetService<IRiderSettingsBuilder>();
riderBuilder.TryBuildRiderSettingsForAcss(out _, out _, null);
```
{% endcode %}

{% hint style="info" %}
The directory for the Rider configuration file is typically:

C:/Users/**{user}**/AppData/Roaming/JetBrains/**Rider{version}**/filetypes/**Acss.xml**
{% endhint %}

{% hint style="info" %}
The Rider configuration file relies on AcssContext, and different context environments produce different configurations, mainly in the sense that different types are registered to the type resolution service, which produces different type code highlighting and hints.
{% endhint %}

{% hint style="warning" %}
The Rider's configuration file is not hot-loaded, and a restart of the Rider is required for this configuration update to take effect. After restarting, the code highlighting indicates that the file is in effect.
{% endhint %}

## Using Visual Studio Code

We provide a rudimentary Acss language plugin for Visual Studio Code. It can be downloaded and installed here, or by searching for 'avalonia-css-extension' in the VSCode app.&#x20;

Like Rider, there are no updates planned for this plugin at this time, and the results are mediocre.😥😥😥

{% hint style="warning" %}
Rider and Visual Studio Code trigger two file saves when saving a file, which we have not filtered for in the current version.
{% endhint %}
