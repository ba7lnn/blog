---
layout: post
title: javascript core AJAX and COOKIE
tags: js
---

<style>
	.highlight-left{margin-left: 0;}
</style>

My javascript learn through practice!

this code define core function AJAX dnd cookie!

{% highlight javascript %}
//\\COOKIE//\\
cookie = {
	set:function(name, value, expires/*days*/, path, domain, secure){
		var expDate = new Date(); 
		expDate.setTime (expDate.getTime() + (1000 * 60 * 60 * 24 * expires));
		document.cookie = name + "=" + escape(value) + ((expires) ? "; expires=" + expDate.toGMTString() : "") +  ((path) ? "; path=" + path : "") +  ((domain) ? "; domain=" + domain : "") +  ((secure) ? "; secure" : "");
	},
	get:function(name){
		var aCookie = document.cookie.split("; ");
		for (var i=0; i < aCookie.length; i++){
			var aCrumb = aCookie[i].split("=");
			if (name == aCrumb[0]) return unescape(aCrumb[1]);
		}
		return null;},
	del:function(name, path, domain){
		if (this.get(name)){
			document.cookie = name + "=" + ((path) ? "; path=" + path : "") + ((domain) ? "; domain=" + domain : "") + "; expires=Thu, 01-Jan-70 00:00:01 GMT";
		}}
};
//\\AJAX BASE//\\
ajax = {
	//public
	request:function(url,handler,container,data){
		var xmlhttp = this.createXmlHttpReq();
		if(xmlhttp == null) return false;
		xmlhttp.onreadystatechange = function() {
			if (container != null && xmlhttp.readyState == 1){
				container.innerText = "Loading...";
			}else if (xmlhttp.readyState == 4 && xmlhttp.status == 200){
				if(handler == null){
					if(container != null) container.innerText = "Done.";
				}else{handler(xmlhttp);}
			}
		};
		var method = (data == null) ? "GET" : "POST";
		xmlhttp.open(method, url + "&rand=" + this.counter(), true);
		if(method == "POST"){
			xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
		};
		xmlhttp.send(data)},
	//private
	uniqnum:(new Date).getTime(),
	counter:function(){return this.uniqnum++},
	createXmlHttpReq:
		function(){
			return (is_ie) ? new ActiveXObject((is_ie5) ? "Microsoft.XMLHTTP" : "Msxml2.XMLHTTP") : new XMLHttpRequest();
		}
}

//\\HIGHTLIGHT//\\
function light(elm){
	if(HIGHTLIGHT.length==0) return;
	k = HIGHTLIGHT.replace(/ ( *)/g, ' ').replace(/(^ * )?(.*)[ ]$/g,'$2').split(' ');
	h = $(elm).outerHTML;
	for(var i in k)
		h = h.replace(/([^>]*)([^<]*)([^>]*)/gim, function($0,$1,$2,$3){return($1+$2.replace(eval('/'+k[i]+'/gim;'),'<font class="hilight">'+k[i]+'<\/font>')+$3)});
	$(elm).outerHTML = h;
}

//\\ADD EVENT//\\
//ex: addEvent(window, 'load', function(){alert("window load")});
function addEvent(obj, evType, fn){
	if(obj.addEventListener){
		obj.addEventListener(evType, fn, true);
		return true;
	}else if(obj.attachEvent){
		return obj.attachEvent("on"+evType, fn);
	}else if(obj["on"+evType] != null){
		var oldEvt = obj["on"+evType];
		obj["on"+evType] = function(e){
			oldEvt(e);
			fn();
		}
	}else{
		obj["on"+evType] = fn;
	}
}
//\\Father node//\\
function fatherNode(elm,n){
	for(;n>0;n--) elm = elm.parentNode;
	return elm;
}
{% endhighlight javascript %} {: .highlight-left }

