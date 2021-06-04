---
layout: post
title: javascript simple reporing
tags: js
---

<style>
	.highlight-left{margin-left: 0;}
</style>

My javascript learn through practice!

simple report, add HTML body element and change content use api: innerHTML.

{% highlight javascript %}

reporting = {
	create:function(){
		$d = document.createElement("DIV");
		$d.innerText  = "loading...";
		document.getElementsByTagName("BODY")[0].appendChild($d);
	}


}

reporting.create("loading...");

{% endhighlight javascript %} {: .highlight-left }

add script tage in your html file as below code!

```
<script language="JavaScript" src="reporting.js"></script>
```
