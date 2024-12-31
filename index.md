---
layout: default
title: "Home"
---

ceres is a UI framework for the web built around observables.

It works by making small changes to the DOM whenever your data changes, and it feels like writing svelte with hyperscript.


{% highlight typescript %}
const Greeter = $createComponent<{ greeting: string }>(($props) => {
    const name = $store("there");

    return $element("div", {},
        $element("input", { type: "text", bind: { value: name } }),
        $element("button", { on: { click: () => name.value = "ceres" } }, "Greet myself"),
        $element("span", {}, $format(`${$props.greeting} ${name}!`))
    );
});

const app = Greeter({ greeting: "Hey" });
app.mount(document.body);
{% endhighlight %}

