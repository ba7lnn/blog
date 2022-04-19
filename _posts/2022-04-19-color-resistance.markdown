---
layout: post
title:  COLOR RESISTANCE 色环电阻及使用
date:   2022-04-19 08:45:22 +0800
categories: electronics
---

Schematic Diagram(原理图)

```
   *----[-- R1 --]-------*
   |                 |   |
Ui =              [R2]   ~ Uo
   |                 |   |
   *---------------------*
```

 公式：Uo=Ui * R1/(R1+R2)

1) Ui = 5V， R1=10K,  R2=10K，
那么：Uo= 10K / (10K+10K)  
所以：5*0.5=2.5V

2) 5V变成3V, 则需R1=3.3K和R2=4.7K =>3V (2.9375)



关于功率：

0.25W(1/4W) 金色，金色5%
0.5W(1/2W) 蓝色，银色10%
1W(1/2W) 灰色

P = I2R 或 P = U2 / R