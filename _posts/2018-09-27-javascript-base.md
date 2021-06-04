---
layout: post
title: javascript base user agent and $ prototype function
tags: js
---

<style>
	.highlight-left{margin-left: 0;}
</style>

My javascript learn through practice!

this code define user agent with navigator!

{% highlight javascript %}
var uagent    = navigator.userAgent.toLowerCase();
var is_safari = ((uagent.indexOf('safari') != -1) || (navigator.vendor == "Apple Computer, Inc.") );
var is_opera  = (uagent.indexOf('opera') != -1);
var is_webtv  = (uagent.indexOf('webtv') != -1);
var is_ie     = ((uagent.indexOf('msie') != -1) && (!is_opera) && (!is_safari) && (!is_webtv) );
var is_ie4    = ((is_ie) && (uagent.indexOf("msie 4.") != -1) );
var is_ie5    = (uagent.indexOf("msie 5") != -1) && document.all;
var is_ie6    = (uagent.indexOf("msie 6") != -1) && document.all;
var is_moz    = ((navigator.product == 'Gecko')  && (!is_opera) && (!is_webtv) && (!is_safari) );
var is_ns     = ((uagent.indexOf('compatible') == -1) && (uagent.indexOf('mozilla') != -1) && (!is_opera) && (!is_webtv) && (!is_safari) );
var is_ns4    = ((is_ns) && (parseInt(navigator.appVersion) == 4) );
var is_kon    = (uagent.indexOf('konqueror') != -1);

var is_win    = ((uagent.indexOf("win") != -1) || (uagent.indexOf("16bit") !=- 1) );
var is_mac    = ((uagent.indexOf("mac") != -1) || (navigator.vendor == "Apple Computer, Inc.") );
var ua_vers   = parseInt(navigator.appVersion);

//
var IE_all_cache = {};
if (document.all && !document.getElementById) {
	$ = function(id) {
		if (IE_all_cache[id] === null) {
			IE_all_cache[id] = document.all[id];
		}
		return IE_all_cache[id];
	};
}else{
	$ = function(id){return document.getElementById(id)};
}

// prototype
String.prototype.trim  = function(){return this.replace(/^\s+|\s+$/g,"")};
//String.prototype.hesc  = function(){return this.replace(/</g,"&lt;").replace(/>/g,"&gt;")};
String.prototype.nl2br = function(){return this.replace(/\n/g,"<br>")};
String.prototype.regEscape = function(){return this.replace(/([\\\/\.\?\+\*\(\)\[\]\{\}\^\$\|\=\!\:])/g, '\\$1')};
//String.prototype.toColor = function(){var ocp = document.createElement("body"); ocp.bgColor = this.toString(); return ocp.bgColor};
//String.prototype.len = function() { var ca = this.match(/[^\x00-\xff]/ig); return this.length + (ca == null ? 0 : ca.length)};
{% endhighlight javascript %} {: .highlight-left }

