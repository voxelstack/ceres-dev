---
layout: docs
section: Special renderables
title: $head
---

The head element `$head` lets you add children to the head.

{% highlight typescript %}
$head(
    $element("title", {}, "ceres")
)
.mount(document.body);
{% endhighlight %}
