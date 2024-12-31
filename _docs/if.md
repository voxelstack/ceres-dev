---
layout: docs
section: Directives
title: $if
---

To render something conditionally, wrap it in an `$if` directive. You can use the returned value to chain calls to `$elseif` or `$else`.

{% highlight typescript %}
const temperature = $store(-10);

$if(temperature.derive((t) => t > 15), $element("span", {}, "this is hell!")).
$elseif(temperature.derive((t) => t >= -5), $element("span", {}, "comfy!")).
$else($element("span", {}, "I still like it but that's pretty cold!"))
.mount(document.body);
{% endhighlight %}

The condition can also be a literal (but of course that won't be reactive), or a mix of stores and literals.
