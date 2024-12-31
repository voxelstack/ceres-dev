---
layout: docs
section: Styling
title: Inline styles
---

Passing a value to the `style` prop will set inline styles on the created element. These styles can be literals or stores.

{% highlight typescript %}
const left = $store(0);
setInterval(() => ++left.value, 2);

$element("span", { style: {
    display: "block",
    position: "absolute",
    backgroundColor: "black",
    color: "white",
    left: $format`${left}px`,
}}, "weeee")
.mount(document.body);
{% endhighlight %}
