---
layout: post
title: javascript tabpage
tags: js
---

<style>
	.highlight-left{margin-left: 0;}
</style>

My javascript learn through practice!


{% highlight javascript %}

tab={
panel:document.createElement("div"),
pagesPanel:document.createElement("div"),
pages:[],
tabsPanel:document.createElement("div"),
tabs:[],
create:function(elm,evt){
	this.elm = elm;
	this.elm.style.display = "none";
	this.id = (arguments[2] == undefined) ? 0 : arguments[2];
	this.tabsPanel.className = "tabs";
	this.pagesPanel.className = "pages";
	var cs = this.elm.childNodes;
	for (var i = 0; i < cs.length; i++) {
		if(cs[i].nodeType != 3){
			if(cs[i].nodeName == 'DT'){
					var newTab = this.createTab(cs[i].innerHTML.replace(/^\s+|\s+$/g,""));
					newTab.id = this.tabs.length;
					if(evt && typeof evt[newTab.id] == "function") newTab.evtHandle = evt[newTab.id];
					this.tabs.push(newTab);
					this.tabsPanel.appendChild(newTab);
			}else{
					var newPage= this.createPage(cs[i].innerHTML);
					newPage.id = this.pages.length;
					this.pages.push(newPage);
					this.pagesPanel.appendChild(newPage);
			}
			cs[i].style.display = "none";
		}
	}
	this.pages[this.id].style.display = 'block';
	if(evt && typeof evt[this.id] == "function") evt[this.id](this.pages[this.id]);
	this.tabs[this.id].className = "tab_selected";
	this.panel.appendChild(this.tabsPanel);
	this.panel.appendChild(this.pagesPanel);
	this.elm.parentNode.replaceChild(this.panel,this.elm);
},
createPage:function(w){
	page = document.createElement("div");
	page.innerHTML = w;
	page.style.display="none";
	return page;
},
createTab:function(w){
	var _tb = document.createElement("A");
	_tb.innerHTML = w;
	_tb.className = "tab";
	_tb.master = this;
	_tb.onclick = function (){
		this.master.tabs[this.master.id].className = "tab";
		this.className = "tab_selected";
		this.master.pages[this.master.id].style.display = "none";
		this.master.pages[this.id].style.display = "block";
		this.master.id = this.id;
		this.evtHandle(this.master.pages[this.id]);
		};
	return _tb;}
}

{% endhighlight javascript %} {: .highlight-left }

html code

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<html>
<head>
<title> New Document </title>
<meta name="Generator" content="EditPlus">
<meta name="Author" content="">
<meta name="Keywords" content="">
<meta name="Description" content="">
</head>
<style>
/* TAB */
.tabs{padding:7px;padding-bottom:0px;border-bottom:1px solid #e8e8e8}
.tab{margin:3px; padding:3px; padding-bottom:0px; background:#FF9933; border:1px solid #FFFFCC; border-bottom:none; cursor:pointer; }
.tab_selected{margin:3px; padding:3px; padding-bottom:0px; background:white; border:1px solid #6600CC; border-bottom:none; cursor:pointer;}
.pages{padding:3px;}
</style>
<body>
	<script language="JavaScript" src="tab.js"></script>
	<dl id="foo">
		<dt>PAGE ONE
		<dd>PAGE ONE CONTENT
		<dt>PAGE TWO
		<dd>PAGE TWO CONTENT
	</dl>
	<script language="JavaScript">
		tab2 = tab;
		tab.create(document.getElementById("foo"),[function(elm){elm.innerHTML = 'what?'},function(elm){elm.innerHTML = 'where?'}],1);
	</script>
	 <br><br><br><hr><br><br><br>
	<dl id="foo2">
		<dt>PAGE ONE
		<dd>PAGE ONE CONTENT
		<dt>PAGE TWO
		<dd>PAGE TWO CONTENT
	</dl>
	<script language="JavaScript">
		tab2.create(document.getElementById("foo2"),[function(elm){elm.innerHTML = 'what?'},function(elm){elm.innerHTML = 'where?'}],1);
	</script>

</body>
</html>
```
