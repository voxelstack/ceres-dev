---
layout: docs
section: Introduction
title: Literals and stores
---

There are two ways to store data when working with ceres: literals and stores.

## Literals
Any JavaScript variable *that can be represented as a string* is a literal. Literals can be used to set attributes or as children. When a literal appears as a child of an element, it will be rendered as a text node. It's important to know that literals are *not reactive*, so changing the value of a literal after it has rendered has no effect.

{% highlight typescript %}
const name = "Ceres Fauna";
let graduated = true;

$element("div", {},
    $element("span", {}, name),
    $element("button", { disabled: graduated }, "Watch")
)
.mount(document.body);

// Does nothing.
graduated = false;
{% endhighlight %}

## Stores
Stores hold a literal and can be subscribed to. Whenever the stored value changes all subscribers will be notified. You can create a store by calling `$store` with an initial value. Reading and writing to a store is done through the `value` property. If necessary, you can specify the type of the store at creation.

{% highlight typescript %}
const graduated = $store(true);
graduated.value = false;
console.log(graduated.value);

const nullable = $store<string | null>(null);
{% endhighlight %}

Stores can be used in place of literals, but they are *reactive* and renderables will update when their values change.

{% highlight typescript %}
const name = "Ceres Fauna";
const graduated = $store(true);

$element("div", {},
    $element("span", {}, name),
    $element("button", { disabled: graduated }, "Watch")
)
.mount(document.body);

// Now it works!
setTimeout(() => graduated.value = false, 1000);
{% endhighlight %}

### Embedding stores on strings
Stores may be used to construct reactive strings using the `$format` tag.

{% highlight typescript %}
const pi = $store(3.1415926535897932384);

$element("span", {}, $format`PI = ${pi}`)
.mount(document.body);

setTimeout(() => pi.value = 3, 1000);
{% endhighlight %}

### Derived stores
Stores can be transformed to create a new store that updates whenever the original changes. To create a derived store call `derive` on the original store, and give it a transformation function. The original store is unaffected.

{% highlight typescript %}
const pi = $store(3.1415926535897932384);
const tau = pi.derive((pi) => 2 * pi);

$element("span", {}, $format`TAU = ${tau}`)
.mount(document.body);
{% endhighlight %}

To combine multiple stores, use the `$derive` function with the original stores, along with an aggregator function.

{% highlight typescript %}
const baseDamage = $store(2);
const critMultiplier = $store(1.5);

const critDamage = $derive([baseDamage, critMultiplier], ([base, multi]) => base * multi);
{% endhighlight %}

### Custom stores
You can write your own stores by extending the `Store` class, and ceres will know how to make them reactive. It just works.

{% highlight typescript %}
class Hakos extends Store<number> {
    private stored: number;

    constructor() {
        super();
        setInterval(() => this.value = Math.random(), 1000);
        this.stored = Math.random();
    }

    get value() { return this.stored };
    set value(value) {
        const previous = this.value;
        this.stored = value;
        this.notify(previous);
    }
}

const baelz = new Hakos();
$element("span", {}, baelz).mount(document.body);
{% endhighlight %}
