---
layout: post
title: how to get the 1 Wire hook-up wire cost price
tags: cost
---

fellow this step：

step 1: part of conductor: 
```
0.7854 × d × d × 1.02 × G × 0.3048
```
0.7854 = 𝔫 ÷ 4 ， d：part of single-bunch， N：conductor count， 1.02: extra compensation， G： proportion， 0.3048: The 1000Ft length
(proportion：Gold:19.3, Sivler：10.5, Copper：8.89, Aluminium： 2.7, Aluminium magnesium(mg) alloy： 2.75)

2: part of insulation：

```
0.7854 ×（D^2 - d^2×0.85) × G × 0.3048
```
D: Insulation of diameter， d：bunch stranded of Over diameter， G： PVC proportion

Memo： Calculator：sqrt(N) × 1.155 × d
(N: cunductor count， 1.155, constant， d：copper diameter)
(Material Proportion：PVC(SR-PVC): 1.45, HDPE(PP): 0.97, LDPE: 0.94, FPE: 0.7, XL-PE:1.55, TPE: 1.2 PU: 1.3, FEP:2.16)

3: Spiral shielded：
```
0.7854×d2×N×1.04×G×0.3048
```
d=copper diameter,N=copper count,1.04=extra compenstation,G=Copper proportion

4: Braid Shielded：
```
0.7854×d2×N×1.05×G×0.3048
```
d=copper diameter,N=copper count,1.05=extra compenstation,G=Copper proportion
(65%~65%=1.05, 65%~85%=1.08, 85%=1.10)

5: Outer Jacket：
```
0.7854×(D2-d2×0.85)×G×0.3048 (substantial extruded)
```
```
0.7854×(D2-d2)×G×0.3048 (haft pipe extruded)
```
D=outer jacket diameter,d=haft wire stranded diameter,0.85=constant,G=material proportion
