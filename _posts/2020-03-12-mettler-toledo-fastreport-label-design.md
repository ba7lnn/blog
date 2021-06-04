---
layout: post
title: 托利多称重系统fastreport报表设计，打印轴标签
tags: mcu
---

东莞市蓝斯铜精密线材有限公司
电话：0769-85251250 传真：0769-85251263
地址：东莞市虎门镇树田社区现代路5号

[frxDBDataset_M."料号名称"]
[frxDBDataset_M."规格名称"]
[frxDBDataset_M."毛重"]
[frxDBDataset_M."净重"]
[frxDBDataset_M."日期"]
[copy('0000'+inttostr(<frxDBDataset_M."流水号">),length('0000'+inttostr(<frxDBDataset_M."流水号">))-3,4)]
[frxDBDataset_M."作业员"]

页面大小：6.8cm*9.7cm 