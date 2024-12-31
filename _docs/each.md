---
layout: docs
section: Directives
title: $each
---

The `$each` directive takes an array of and renders a something for each of its elements.

{% highlight typescript %}
const tubers = [
    { label: "Ceres Fauna", value: "ceres_fauna" },
    { label: "Gawr Gura", value: "gawr_gura" },
    { label: "Mori Calliope", value: "mori_calliope" },
    { label: "Ouro Kronii", value: "ouro_kronii" },
];

$element("select", {},
    $each(tubers, ({ label, value }) => $element("option", { value }, label))
)
.mount(document.body);
{% endhighlight %}

You can use the returned value to chain a call to `$else`. The else directive gets rendered if the array is empty.

{% highlight typescript %}
const tubers = [];

$element("select", {},
    $each(tubers, ({ label, value }) => $element("option", { value }, label)).
    $else($element("option", {}, "Touch grass"))
)
.mount(document.body);
{% endhighlight %}
