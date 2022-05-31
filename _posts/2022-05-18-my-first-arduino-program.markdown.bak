---
layout: post
title:  My First Arduino Program
date:   2022-05-18 09:04:12 +0800
categories: arduino
---

 这里使用汉字取模软件后，进行汉字的显示，12864屏。


```
#include <Adafruit_GFX.h>
#include <Adafruit_GrayOLED.h>
#include <Adafruit_SPITFT.h>
#include <Adafruit_SPITFT_Macros.h>
#include <gfxfont.h>

#include <Adafruit_SSD1306.h>
#include <splash.h>

#include <Wire.h>

static const unsigned char PROGMEM hans_peng[] = {
  0x08,0x00,0x08,0x04,0xFF,0x84,0x08,0x08,0x08,0x10,0x7F,0x22,0x00,0x02,0x7F,0x04,
  0x41,0x08,0x41,0x10,0x7F,0x22,0x00,0x02,0x22,0x04,0x17,0x88,0xF8,0x10,0x40,0x60 /*"彭",0*/
};

static const unsigned char PROGMEM hans_long[] = {
  0x04,0x20,0x04,0x10,0x04,0x10,0x04,0x00,0xFF,0xFE,0x04,0x80,0x04,0x88,0x04,0x88,
  0x04,0x90,0x08,0xA0,0x08,0xC0,0x10,0x82,0x11,0x82,0x22,0x82,0x44,0x7E,0x80,0x00/*"龙",1*/
};

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

#define OLED_RESET 4


Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  Serial.println("Start");

  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
}

void loop() {
  // put your main code here, to run repeatedly:
  word_display();
  display.display();
}

void word_display()
{
  display.clearDisplay();
  display.setTextColor(WHITE);

  display.setTextSize(2);

  display.setCursor(0, 0);
  display.print("DSP");

  // 显示文字 (左上角x坐标,左上角y坐标, 图形数组, 图形宽度像素点, 图形高度像素点, 设置颜色)
  display.drawBitmap(20*2,16, hans_peng, 16, 16, 1);
  display.drawBitmap(20*3,16, hans_long, 16, 16, 1);

  /*
  display.setTextSize(1.5);
  display.setCursor(0,20);
  display.print("time: ");
  display.print(millis() /1000);
  display.print(" ");

  display.setTextSize(1.2);
  display.setCursor(0,30);
  display.print("Author: ");
  display.print("BA7LNN");

  display.setTextSize(1.2);
  display.setCursor(0,40);
  display.print("Author: ");
  display.print("K");  */

}
```
