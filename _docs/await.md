---
layout: docs
section: Directives
title: $await
---

With the `$await` directive you can choose what to render based on the current state of a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).

{% highlight typescript %}
const query = fetch("https://imissfauna.com/api/v2/past_stream").then((res) => res.json());

$await(query, $element("span", {}, "Loading...")).
$then((result: object) => $element("pre", {}, JSON.stringify(result, null, 8))).
$catch(() => $element("span", {}, "ohno"))
.mount(document.body);
{% endhighlight %}

Every renderable is optional, so you can omit any states you don't care about.

{% highlight typescript %}
const query = fetch("https://imissfauna.com/api/v2/past_stream").then((res) => res.json());

$await(query).
$catch(() => $element("span", {}, "I knew it wouldn't work"))
.mount(document.body);
{% endhighlight %}

The promise can be a store which makes it really easy to retry a failed promise.

{% highlight typescript %}
const makeQuery = () => fetch("https://imissfauna.com/api/v2/past_stream").then((res) => res.json());
const query = $store(makeQuery());

$await(query, $element("span", {}, "Loading...")).
$then((result: object) => $element("pre", {}, JSON.stringify(result, null, 8))).
$catch(() => $element("button", { on: { click: () => query.value = makeQuery() } }, "Retry"))
.mount(document.body);
{% endhighlight %}
