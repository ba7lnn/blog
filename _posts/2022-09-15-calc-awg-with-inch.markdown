---
layout: post
title:  AWG与英寸的关系如下
date:   2022-09-15 09:24:02
---


AWG与英寸的关系如下： 

AWG = A lg inch -b

其中

A=-19.93156857,

B=9.73724 

通常AWG(American Wire Gauge美国线规)或者密耳目圆(Circular Mil,直径1mil即0.001inch=0.0254mm的导体截面积)

1 cmil=(1mil)^2 例如：36AWG=5mils是指直径为5密耳，

而不是指5个密耳圆的面积 4/0AWG=460mils

问: 怎样由AWG换算出mils? 


管: 每AWG之间直径呈等比数列递增， 
等比"X"算法： x=(460/5)的1/39次方=1.122932mils, 

其中39为4/0AWG与36AWG之间的AWG数，即直径从5密耳按等比增加到460密耳目的等比系数，
因此线径（密耳）=5*｛X的（36-AWG)次方} 

例： 34AWG=5*1.122932^(36-34)=6.3mils=0.16mm(导体直径)

18AWG=5*1.122932^(36-18)=40.3mils=1.023mm(导体直径) 

在国外规格书常见：UL1007 26WG conductor: 11/34 中的34即为国内使用的：11/0.16BC


__这里提供一个简单的计算工具__

<script>
function go()
{
	//=5*1.122932^(36-34)*0.0254
	//document.getElementById("demo").innerHTML=Date();

	var awg = document.getElementById("awg").value;
	var m = 5*Math.pow(1.122932,(36-awg))*0.0254;
	m = parseFloat(m).toFixed(4);
	document.getElementById("v").innerHTML = m;
}
</script>

<form>
<div id="v"></div>
<input type="text" name="awg" id="awg" value="34"/>
<button type="button" onclick="go()">Go</button>
</form>
