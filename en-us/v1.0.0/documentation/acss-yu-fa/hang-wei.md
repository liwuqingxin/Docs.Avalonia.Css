# Behavior

## Behavior Syntax

The [definition of behavior](../zhu-ti-bang-zhu/ru-he-shi-yong-acss.behaviors.md) has already been mentioned.&#x20;

Behaviours are simple to use, similar to Setters, using the name of the behavior set as the attribute and the name of the behavior as the value.&#x20;

For example, the behavior `window.esc.close` belongs to the built-in behaviour set acss, and is used in the following way:

```css
Window{
    Background: red;
    .acss:window.esc.close;
}
```

If you customise the behaviour set xbeh, which defines the behaviour window.xxx.xxx within it, use it as follows:

```
Window{
    Background: red;
    .acss:window.esc.close;
    .xbeh:window.xxx.xxx;
}
```
