---
layout: docs
section: Introduction
title: Basic rendering
---

Rendering elements with ceres is very similar to hyperscript.

## Elements
Calling `$element` describes a renderable that can be mounted onto the DOM.
The simplest renderable only takes the name of the tag that will be created:

{% highlight typescript %}
$element("div")
.mount(document.body);
{% endhighlight %}

## Props
Attributes can be set by passing an object as the second argument of `$element`.

{% highlight typescript %}
$element("input", { type: "text" })
.mount(document.body);
{% endhighlight %}

## Children
Any arguments passed after the props will be rendered as children of the created element.

{% highlight typescript %}
$element("div", {},
    $element("ul", {},
        $element("li", {}, "ceres"),
        $element("li", {}, "watson"),
        $element("li", {}, "ouro")
    )
)
.mount(document.body);
{% endhighlight %}

### Fragments
To render multiple children as siblings, use a `$fragment`.

{% highlight typescript %}
$fragment(
    $element("span", {}, "ceres"),
    $element("span", {}, "watson"),
    $element("span", {}, "ouro")
)
.mount(document.body);
{% endhighlight %}

## Events
The `on` prop lets you attach event listeners. Just pass the handler to the corresponding property.

{% highlight typescript %}
const time = $store(0);

$element(
    "button", {
        on: {
            mousedown: () => time.value = Date.now(),
            mouseup: () => {
                if (Date.now() - time.value === 35) {
                    alert("Stanley was happy");
                }
            }
        }
    }, "Press me for 35ms"
)
.mount(document.body);
{% endhighlight %}

### Configured listeners
To customize the event listener, wrap it in a `$handler`.

{% highlight typescript %}
$element(
    "button", {
        on: {
            click: $handler(() => alert("Congratulations, you destroyed everything."), { once: true })
        }
    }, "SCP-001-J"
)
.mount(document.body);
{% endhighlight %}
