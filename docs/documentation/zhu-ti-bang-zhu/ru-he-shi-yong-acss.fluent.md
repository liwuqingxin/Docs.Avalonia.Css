# 如何使用 Acss.Fluent

* 安装依赖库。

```bash
dotnet add package Nlnet.Avalonia.Css.Fluent --version 1.0.0-beta.4
```

* 使用主题。

```xml
<Application.Styles>
    <fluent:AcssFluentTheme 
        AutoExportSourceToLocal="True"
        PreferLocalPath="{x:Static app:DebugThing.PreferLocalPath}"
        UseRecommendedPreferSource="True" />
</Application.Styles>
```

```csharp
    public static class DebugThing
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

## AcssFluentTheme

AcssFluentTheme 有以下属性。

<table><thead><tr><th width="303.3333333333333">属性</th><th>描述</th></tr></thead><tbody><tr><td>AutoExportSourceToLocal</td><td><p>是否自动导出到本地。当设置了优先源时生效。</p><p>生效后会在初始化阶段尝试导出代码资源到本地。</p><p>如果文件已经存在则跳过。</p></td></tr><tr><td>PreferLocalPath</td><td>优先源的本地路径。设置后会提供该路径的优先源。</td></tr><tr><td>UseRecommendedPreferSource</td><td>使用推荐的优先源。其本地路径为嵌入的资源的绝对路径作为相对路径，相对于当前程序目录的路径。</td></tr></tbody></table>
