---
layout: docs
section: Special props
title: "bind:"
---

Normally data flows from stores to renderables: when the value of a store updates, the page is updated. Bound data flows the other way: changes to the page will update the store.

Subscribing to the store will run a callback whenever a bound value changes. The callback receives the new value and the previous one.
Using `subscribe` will only notify you *when the value changes*. To be notified of the *initial value* as well, use `watch` instead. 

Normal reactivity still happens, so changing the store will still update the page (though some binds are readonly).

{% highlight typescript %}
const tooSlow = $store(false);
const value = $store("");
value.subscribe(console.log);

$element("input", { type: "text", disabled: tooSlow, bind: { value } })
.mount(document.body);

setTimeout(() => {
    // Updating a bound store will still update the renderable.
    value.value = "Time's up!";
    tooSlow.value = true;
}, 5000);
{% endhighlight %}

## Input value
The `value` property of inputs can be bound using `bind: { value }`. That's what we used on the first example. Input values get bound as a string.

{% highlight typescript %}
const value = $store("");
value.subscribe(console.log);

$element("input", { type: "text", bind: { value } })
.mount(document.body);
{% endhighlight %}

## Input checked
Binding to checkboxes can be done with `bind: { checked }`, and the value gets bound as a boolean.

{% highlight typescript %}
const checked = $store(true);
checked.subscribe(console.log);

$element("input", { type: "checkbox", bind: { checked } })
.mount(document.body);
{% endhighlight %}

## Select value
You can also bind the value of a select element using `bind: { value }`.

{% highlight typescript %}
const tubers = [
    { label: "Ceres Fauna", value: "ceres_fauna" },
    { label: "Gawr Gura", value: "gawr_gura" },
    { label: "Mori Calliope", value: "mori_calliope" },
    { label: "Ouro Kronii", value: "ouro_kronii" },
    { label: "Tsukumo Sana", value: "tsukumo_sana" }
];
const value = $store(tubers[0].value);
value.subscribe(console.log);

$element("select", { bind: { value }},
    $each(tubers, ({ label, value }) => $element("option", { value }, label))
)
.mount(document.body);
{% endhighlight %}

## Dimension binds
The dimensions of an element can be bound using one of the following:
- `clientWidth`
- `clientHeight`
- `contentWidth`
- `contentHeight`

These binds are *readonly*.

{% highlight typescript %}
const n = $store(0);
const factorial = n.derive((n) => fact(n));

const clientWidth = $store(1);
clientWidth.subscribe(console.log);

function fact(n: number): number {
    return n === 0 ? 1 : n * fact(n - 1);
}
setInterval(() => ++n.value, 1000);

$element("span", {
    style: { display: "inline-block" },
    bind: { clientWidth }
}, $format`${n}! = ${factorial}`)
.mount(document.body);
{% endhighlight %}

## this
Binding to `this` sets the bound store to the DOM node created by the renderable. If the element is not on the DOM, the store gets set to `null`.

{% highlight typescript %}
const canvas = $store<HTMLCanvasElement | null>(null);
canvas.subscribe((next) => {
    const ctx = next?.getContext("webgpu");
    console.log("Fancy 3D graphics coming soon...", ctx);
});

$element("canvas", { bind: { this: canvas }})
.mount(document.body);
{% endhighlight %}

## Bind transformers
A bound store will receive values as they come from the page. You can transform the value by using a bind object instead. Bind objects have a store and two transformers:
- `toBind`: Transforms data from the page before updating the bound store
- `fromBind`: Transforms data from the bound store before updating the page

For example you could transform an input bind so the value gets turned to uppercase:
{% highlight typescript %}
const value = $store("");
value.subscribe(console.log);

$element("input", { type: "text", bind: { value: {
    store: value,
    toBind: (next) => next.toUpperCase(),
    fromBind: (next) => next
}}})
.mount(document.body);
{% endhighlight %}

ceres has some builtin transformers that can be applied with the `$transform` wrapper.

### integer
Parses the value of a number input and binds it as a number rather than a string.

{% highlight typescript %}
const value = $store(0);
value.subscribe(console.log);

$element("input", { type: "number", bind: { value: $transform("integer", value) }})
.mount(document.body);
{% endhighlight %}

### multiselect
Binds the value of a select `multiple` to an array of selected values.

{% highlight typescript %}
const tubers = [
    { label: "Ceres Fauna", value: "ceres_fauna" },
    { label: "Gawr Gura", value: "gawr_gura" },
    { label: "Mori Calliope", value: "mori_calliope" },
    { label: "Ouro Kronii", value: "ouro_kronii" },
    { label: "Tsukumo Sana", value: "tsukumo_sana" }
];
const selected = $store([tubers[0].value]);
selected.subscribe(console.log);

$element("select", { multiple: true, bind: { value: $transform("multiselect", selected) }},
    $each(tubers, ({ label, value }) => $element("option", { value }, label))
)
.mount(document.body);
{% endhighlight %}

### checkGroup
Binds the value of multiple checkboxes to a an array of selected values.

{% highlight typescript %}
const tubers = [
    { label: "Ceres Fauna", value: "ceres_fauna" },
    { label: "Gawr Gura", value: "gawr_gura" },
    { label: "Mori Calliope", value: "mori_calliope" },
    { label: "Ouro Kronii", value: "ouro_kronii" },
    { label: "Tsukumo Sana", value: "tsukumo_sana" }
];
const selected = $store([tubers[0].value]);
selected.subscribe(console.log)

$element("fieldset", undefined, $each(
    tubers,
    (entry) => $fragment(
        $element("label", { htmlFor: entry.value }, entry.label),
        $element("input", {
            id: entry.value,
            value: entry.value,
            type: "checkbox",
            bind: { checked: $transform("checkGroup", selected)}})
    )
))
.mount(document.body);
{% endhighlight %}

### radioGroup
Binds the value of a radio group.

{% highlight typescript %}
const tubers = [
    { label: "Ceres Fauna", value: "ceres_fauna" },
    { label: "Gawr Gura", value: "gawr_gura" },
    { label: "Mori Calliope", value: "mori_calliope" },
    { label: "Ouro Kronii", value: "ouro_kronii" },
    { label: "Tsukumo Sana", value: "tsukumo_sana" }
];
const selected = $store(tubers[0].value);
selected.subscribe(console.log)

$element("fieldset", undefined, $each(
    tubers,
    (entry) => $fragment(
        $element("label", { htmlFor: entry.value }, entry.label),
        $element("input", {
            id: entry.value,
            value: entry.value,
            type: "radio",
            bind: { checked: $transform("radioGroup", selected)}})
    )
))
.mount(document.body);
{% endhighlight %}
