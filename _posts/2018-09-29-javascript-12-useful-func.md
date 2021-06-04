---
layout: post
title: javascript 12 useful function
tags: js
---

<style>
	.highlight-left{margin-left: 0;}
</style>

My javascript learn through practice!

some useful function

{% highlight javascript %}

/*
* ZOOM PICTURE;
function zooom(){
	o=window.event.srcElement;
	o.z = o.z ? o.z : 10;
	event.wheelDelta>=100 ? o.z++ : o.z --;
	if(o.z > 20)o.z = 20;
	if(o.z < 1)o.z = 1;
	o.style.zoom = o.z + '0%';
	return false;
}
document.getElementsByClassName = function(className) { 
	var children = document.getElementsByTagName('*') || document.all;
	var elements = [];
	for (var i = 0; i < children.length; i++) {
		var child = children[i];
		var classNames = child.className.split(' ');
		for (var j = 0; j < classNames.length; j++) {
			if (classNames[j] == className) {
				elements.push(child);
				break;
			}
		}
	}
	return elements; 
}
printView = function(){
	var nodes = document.getElementsByClassName("noprint");
	for(var x in nodes){
		sh(nodes[x]);
	}
	if(window.event.srcElement){
		window.event.srcElement.src = nodes[x].style.display=="" ? "res/printer.gif" : "res/printer2.gif";
	}
}
*/
function sh(e){
	e.style.display = (e.style.display=="") ? "none" : "";
}

// get real position of elm
function realpos(elm) {
	if(is_ie){
		return elm.getBoundingClientRect();
	}
	var pos = {left:elm.offsetLeft,top:elm.offsetTop};
	while((elm = elm.offsetParent) !== null) {
		pos.left += elm.offsetLeft;
		pos.top += elm.offsetTop;
	}
	return pos;
}
// preload images.
function preloadImages(){
	if(!document.PIMAGES){
		document.PIMAGES = [];
		n = arguments.length-1;
		for(var i=0;i<=n;i++){
			document.PIMAGES[i] = new Image();
			document.PIMAGES[i].src = arguments[i];
		}
	}
}
////
//function jump2(_url){
//	setTimeout("window.location.href = '"+_url+"';",3000);
//}
/**
 * visibility function
 */
function tog(){
	display = arguments[0];
	for( var i=1; i<arguments.length; i++ ) {		
		var x = $(arguments[i]);
		if (x.style.display == "none" || x.style.display === ""){
			x.style.display = display;
		}else{
			x.style.display = "none";
		}
	}
	var e = is_ie ? window.event : this;
	if (e) {
		if (is_ie) {
			e.cancelBubble = true;
			e.returnValue = false;
		}
		return false;
	}
}
/*
function tog2(){
	$(arguments[0]).style.visibility = arguments[1];
	if(arguments[1]=="hidden") $(arguments[0]).style.display = "none";
	else $(arguments[0]).style.display = "block";
}*/
// get block visible from cookie
function gcv(){
	for( var i=0; i<arguments.length; i++ ) {
		v = cookie.get(arguments[i] + "_v");
		$(arguments[i]).style.display = (v===null) ? 'block' : v;
	}
}
// save block visible to cookie
function scv(){
	for( var i=0; i<arguments.length; i++ ){
		cookie.set(arguments[i] + "_v",$(arguments[i]).style.display);
	}
}

//
var tags=[];
function dsp(o){
	e = is_ie ? window.event : this;
	o = o ? o : e.srcElement; // e.target ?? IE?
	var d = $("flt_" + o.id);
	if(d === null){
		isp = function(){
			d = document.createElement('DIV');
			d.style.cssText = "position: absolute;border: " + DARK_COLOR + " 1px solid;padding:1px;";
			p = realpos(o);
			d.style.top = p.top + o.offsetHeight;
			d.style.left = p.left;
			d.id = 'flt_' + o.id;
			document.getElementsByTagName("body")[0].appendChild(d);
		};
		csp = function(){
			d.style.visibility = "hidden";
			d.innerHTML = "";
		};
		asp = function(w){
			d.innerHTML += "<div style=\"cursor:pointer\" class=\"lightBlock2\" onclick=\"$('"+o.id+"').value = this.innerHTML;csp();\">"+ w + "</div>";
			d.style.visibility = "";
		};	
		isp();
	}
	csp();
	v = o.value.substr(o.value.lastIndexOf(',')+1);
	l = v.length;
	if(l === 0){
		return;
	}
	if(e.keyCode != 13 && e.keyCode !=9 ){
		for(var x in tags){
			if(tags[x].substr(0,l) == v){
				asp(tags[x]);
			}
		}
	}
}

// no useful at all.
toglinks = function(){
	for(x in document.links){
		document.links[x].disabled = arguments[0];
	}
}

 /*
function clearTbody(tbody){
	for(var i=tbody.rows.length;i>0;i--){
			tbody.deleteRow(i-1);
	}
}*/




// Expand links for printing
/*
String.prototype.hasClass = function(classWanted)
{
	var classArr = this.split(/\s/);
	for (var i=0; i<classArr.length; i++)
	  if (classArr[i].toLowerCase() == classWanted.toLowerCase()) return true;
	return false;
}

var expandedURLs;
onbeforeprint = function() { 
	expandedURLs = [];
	var contentEl = $("rn");

	if (contentEl)
	{
	  var allLinks = contentEl.getElementsByTagName("a");

	  for (var i=0; i < allLinks.length; i++) {
		  if (allLinks[i].className.hasClass("external") && !allLinks[i].className.hasClass("free")) {
			  var expandedLink = document.createElement("span");
			  var expandedText = document.createTextNode(" (" + allLinks[i].href + ")");
			  expandedLink.appendChild(expandedText);
			  allLinks[i].parentNode.insertBefore(expandedLink, allLinks[i].nextSibling);
			  expandedURLs[i] = expandedLink;
		  }
	  }
   }
}

onafterprint = function()
{
	for (var i=0; i < expandedURLs.length; i++)
		if (expandedURLs[i])
			expandedURLs[i].removeNode(true);
}
 */


/*
 * FIX IE PNG
 */
if(is_ie){
	addEvent(window,'load', function(){
		for(var i=0; i<document.images.length; i++){
			var img = document.images[i];
			var lw=img.width;
			var lh=img.height;
			var imgName = img.src.toUpperCase();
			if (imgName.substring(imgName.length-3, imgName.length) == "PNG"){
				img.style.filter+="progid:DXImageTransform.Microsoft.AlphaImageLoader(src="+img.src+", sizingmethod=scale);";
				img.src= "res/dot.gif";
				img.width=lw;
				img.height=lh;
			}
		}
	});
}

{% endhighlight javascript %} {: .highlight-left }