---
layout: docs
section: Special renderables
title: $body
---

The `$body` element lets you add styles or event listeners to the body.

{% highlight typescript %}
$body({
    style: {
        position: "fixed",
        top: 0,
        left: 0,
        width: "100vw",
        height: "100vh",
    },
    on: { mouseleave: () => console.log("Noooo don't leave me!!!") }
})    
.mount(document.body);
{% endhighlight %}
