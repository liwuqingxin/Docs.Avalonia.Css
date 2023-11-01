# Performance Evaluation

Acss requires dynamic loading of style files and dynamic generation of objects such as Avalonia resources and styles.Therefore, theoretically, programs using Acss **have more memory and startup performance consumption** than normal Avalonia programs, and **the performance after running is relatively flat**.&#x20;

In addition, Acss uses a lot of reflection and does not support AOT, please be aware of this.

{% hint style="warning" %}
Detailed performance evaluation is not yet complete.
{% endhint %}
