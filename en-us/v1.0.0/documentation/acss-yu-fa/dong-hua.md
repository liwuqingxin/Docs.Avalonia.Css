# 动画

## 动画语法

Acss 的动画会解析为 Avalonia 动画，仅仅语法不一样。

{% hint style="info" %}
动画和子样式一样，放在样式的 Children 当中。
{% endhint %}

动画的语法如下：

```css
^Button{
    Background: Green;
    [[
        :pointerover{
            Background: Blue;
            [[
                ::animation {
                    Duration:'0:0:.2';      // Must exist.
                    FillMode:Forward;       // Optional.
                    Delay: 0.3;             // Optional.
                    // Other Properties.    // Optional.
                    KeyFrames:[             // Write 'KeyFrames' or 'Children'. 
                        KeyFrame:(0% 0,0,1,1)[             // Cue=0%, KeySpine=0,0,1,1.
                            ScaleTransform.ScaleY: 1;
                            Opacity: 1;
                        ]
                        KeyFrame:(0:0:1 0,0,1,1)[          // KeyTime=0:0:1, KeySpine=0,0,1,1.
                            ScaleTransform.ScaleY: 1.7;
                            Opacity: 1;
                        ]
                        KeyFrame:(100%)[
                            ScaleTransform.ScaleY: 1.7;
                            Opacity: var(Uni-Opacity);     // Use Dynamic Resource.
                        ]
                    ]
                }
            ]]
        }
    ]]
}
```

{% hint style="warning" %}
动画目前语法定义不完善，语法可能会发生破坏性更新。
{% endhint %}
