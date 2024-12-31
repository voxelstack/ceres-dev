---
layout: docs
section: Directives
title: $key
---

Renderables inside a `$key` directive will be destroyed and rerendered when the key changes.

{% highlight typescript %}
const timer = $store(0);
setInterval(() => ++timer.value, 1000);

$key(
    timer,
    $element("span", {}, timer)
)
.mount(document.body);
{% endhighlight %}
