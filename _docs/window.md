---
layout: docs
section: Special renderables
title: $window
---

The `$window` element lets you add event listeners or binds to the window.

{% highlight typescript %}
const online = $store(window.navigator.onLine);
online.subscribe((next) => {
    console.log(next ? "Phew! Back online." : "Oh no! Connection lost...");
});

$window({
    on: { keydown: ({ key }) => key === "f" && console.log(":(") },
    bind: { online }
})
.mount(document.body);
{% endhighlight %}

The available binds are:
- `innerWidth`
- `innerHeight`
- `scrollX`
- `scrollY`
- `online`
- `devicePixelRatio`

All are readonly, except for `scrollX` and `scrollY`.
