# Animation

## Animation Syntax

Acss animations will be parsed as Avalonia animations, only the syntax is different.

{% hint style="info" %}
Animations are placed in the Children section of the style, just like child styles.
{% endhint %}

The syntax of the animation is as follows:

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
Animation is currently poorly defined and the syntax may be subject to destructive updates.
{% endhint %}
