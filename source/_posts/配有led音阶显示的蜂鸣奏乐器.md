---
title: 配有LED音阶显示的Arduino蜂鸣奏乐器
tags:
  - Arduino
  - DIY
id: '738'
categories:
  - 技术杂谈
  - Raspberry Pi & Arduino
date: 2021-01-01 15:47:06
---

本文将展示我最近用Arduino DIY的一个配有LED音阶显示小灯的蜂鸣奏乐器。
<!-- more -->
# **一、效果展示**

**\----------------------------------------------------------------------------------------**

知乎：

[月亮代表我的心](https://www.zhihu.com/zvideo/1325378495463915520)

[DIY蜂鸣器演奏《牵手》](https://www.zhihu.com/zvideo/1325379476785008640) 

YouTube:

https://youtu.be/Up8Yi3Mj7tw

https://youtu.be/\_QSvo-1RP8g

# **二、所需器材**

**\----------------------------------------------------------------------------------------**

1.  **Arduino 板**
2.  **无源蜂鸣器**
3.  **LED灯7个**
4.  **串并行寄存器**
5.  **220欧姆电阻器7个**
6.  **杜邦线若干**
7.  **面包板1个**

# **三、插线连接**

**\----------------------------------------------------------------------------------------**

**连接示意图和真实连接图如下：**

**注意：因为本实验中只用了7个LED灯来代表音阶中的1、2、3、4、5、6、7，所以第八个LED被划掉了。**

![](https://pic1.zhimg.com/80/v2-148d2c9d95b10ff60f11ee8e14c3f7b0_1440w.jpg)

![](https://pic3.zhimg.com/80/v2-c73033343912c3c9c8e1b966b12e292e_1440w.jpg)

![](https://pic1.zhimg.com/80/v2-6dcd74bd881952e0a78a487db8e95510_1440w.jpg)

# **四、蜂鸣器演奏音乐**

**\----------------------------------------------------------------------------------------**

音乐的旋律是由声音的频率和节拍来决定的，如果我们能够控制好频率和节拍，那就有可能演奏出动听的音乐。因此，我们首先需要搞清楚各音调的频率，我从网上找到了这张对应的表格：

低音：

![](https://pic2.zhimg.com/80/v2-a9abaa4b71fcde336b36ba0f9414f65d_1440w.jpg)

中音：

![](https://pic4.zhimg.com/80/v2-0765fb91c3eb7d8342be4826a94450b7_1440w.jpg)

高音：

![](https://pic4.zhimg.com/80/v2-89c5808cd3982cb6b8f18c5b850cf86f_1440w.jpg)

我们知道了音调的频率后，下一步就是控制音符的演奏时间。每个音符都会播放一定的时间，这样才能构成一首优美的曲子，而不是生硬的一个调的把所有的音符一股脑的都播放出来。音符节奏分为一拍、半拍、1/4拍、1/8拍，我们规定一拍音符的时间为1；半拍为0.5；1/4拍为0.25；1/8拍为0.125……，所以我们可以为每个音符赋予这样的拍子播放出来，音乐就成了。但具体的音阶根频率之间的关系，可能需要再进一步调节，我们后面会讲到。

首先，我们看《月亮代表我的心》的简谱：

![](https://pic4.zhimg.com/80/v2-d428fb2fc1ca05b7a7998bbc05fcf783_1440w.jpg)

这里你可以选择C、D、E任意大调，然后在程序中定义相应的频率即可。另外，该音乐为四分之四拍，每个对应为1拍。几个特殊音符说明如下：

1）普通音符，占1拍。

2）带下划线音符，表示0.5拍。

3）有的音符后带一个点，表示多加0.5拍。

4）有的音符后带一个—，表示多加1拍。

5）有的两个连续的音符上面带弧线，表示连音。如果后面那个音与前面的相同，可以只发一次音，然后将节拍相加。也可以稍微改下连音后面那个音的频率，比如减少或增加一些数值（需自己调试），这样表现会更流畅。

# **五、代码**

**\----------------------------------------------------------------------------------------**

```
//定义音阶频率
#define NTE0 -1
#define NTE1 330
#define NTE2 370
#define NTE3 410
#define NTE4 441
#define NTE5 495
#define NTE6 556 
#define NTE7 624

#define NTEL1 165
#define NTEL2 175
#define NTEL3 196
#define NTEL4 221
#define NTEL5 248
#define NTEL6 278
#define NTEL7 312

#define NTEH1 661
#define NTEH2 740
#define NTEH3 820
#define NTEH4 882
#define NTEH5 990
#define NTEH6 1112
#define NTEH7 1248
  
  //根据简谱列出《月亮代表我的心》的各频率
int tune2[] = {
  NTE5, NTE3, NTE2, NTE1,
  NTE3,
  NTE6,NTE4, NTE2, NTE1,
  NTEL7,NTE0, NTEL5, 
  NTE1, NTE3, NTE5, NTE1,
  NTEL7, NTE3, NTE5, NTE0, NTE5,
  NTE6, NTE7, NTEH1,  
  NTE6, NTE5, NTE3, NTE2,
  NTE1, NTE1, NTE1, NTE3, NTE2,
  NTE1,NTE1, NTE1, NTE2, NTE3,
  NTE2, NTE1, NTEL6, NTE2, NTE3,
  NTE2, NTE0, NTEL5,
  
  NTE1, NTE3, NTE5, NTE1,
  NTEL7, NTE3, NTE5, NTE0, NTE5,
  NTE6, NTE7, NTEH1, 
  NTE6, NTE5, NTE3, NTE2,
  NTE1, NTE1, NTE1, NTE3, NTE2,
  NTE1,NTE1, NTE1, NTE2, NTE3,
  
  NTE2, NTEL6, NTEL7, NTE1, NTE2,
  NTE1, NTE3, NTE5, 
  NTE3, NTE2, NTE1, NTE5,
  NTEL7, NTEL6, NTEL7,
  NTEL6, NTEL7, NTEL6, NTEL5,
  NTE3, NTE5,
  NTE3, NTE2, NTE1, NTE5,
  NTEL7, NTEL6, NTEL7,
  
  NTE1, NTE1, NTE1, NTE2, NTE3, 
  NTE2, NTE0, NTEL5,
  NTE1, NTE3, NTE5, NTE1,
  NTEL7, NTE3, NTE5, NTE5,
  NTE6, NTE7, NTEH1, 
  NTE6, NTE5, NTE3, NTE2,
  NTE1, NTE1, NTE1, NTE3, NTE2,
  NTE1, NTE1, NTE1, NTE2, NTE3, 
  NTE2, NTEL6, NTEL7, NTE1, NTE2,
  NTE1, NTE0
  
};

  //根据简谱列出《月亮代表我的心》的各节拍
float durt2[]={
  1.5, 0.5, 0.5, 0.5,
  4,
  1.5, 0.5, 0.5, 0.5,
  3, 0.5, 0.5,
  1.5, 0.5, 1, 0.5, //问我爱你
  1.5, 0.5, 1, 0.5, 0.5,
  1.5, 0.5, 1.5,
  1, 1.5, 0.5, 0.5,
  1.5, 0.5, 1, 0.5, 0.5,
  1.5, 0.5, 1, 0.5, 0.5,
  1.5, 0.5, 1, 0.5, 0.5,
  3, 0.5, 0.5,//心。你
  
  1.5, 0.5, 1, 0.5, //问我爱你
  1.5, 0.5, 1, 0.5, 0.5,//有多深，我
  1.5, 0.5, 1.5,//爱你有
  1, 1.5, 0.5, 0.5,//几分？我的
  1.5, 0.5, 1, 0.5, 0.5,//情不移，我的
  1.5, 0.5, 1, 0.5, 0.5,//爱不变，月亮
  
  1.5, 0.5, 1, 0.5, 0.5,//代表我的
  3, 0.5, 0.5,//心。轻
  1.5, 0.5, 1, 1, //轻的一个
  3, 0.5, 0.5,//吻，已经
  1.5, 0.5, 1.5,0.5,//打动我的
  3, 1,//心，深
  1.5,0.5, 1, 1,//深的一段
  3, 0.5, 0.5,//情，教我
  1.5, 0.5, 1, 0.5, 0.5,//思念到如
  3, 0.5, 0.5,//
  1.5, 0.5, 1.5, 0.5,
  1.5, 0.5, 1.5, 0.5,
  1.5, 0.5, 1.5,
  1, 1.5, 0.5, 0.5,
  1.5, 0.5, 1, 0.5, 0.5,
  1.5, 0.5, 1, 0.5, 0.5,
  1.5, 0.5, 1, 0.5, 0.5,
  3, 1
};

int length2;
int tonePin = 6; //用6号接口


int latchPin = 11;
int clockPin = 9;
int dataPin = 12;
//控制七个LED灯的排列组合来模拟七个音阶
byte LED1s = 0b11111110;
byte LED2s = 0b11111101;
byte LED3s = 0b11111011;
byte LED4s = 0b11110111;
byte LED5s = 0b11101111;
byte LED6s = 0b11011111;
byte LED7s = 0b10111111;
byte LED0s = 0b11111111;

void setup() {
  Serial.begin(9600);
  pinMode(tonePin, OUTPUT);
  pinMode(latchPin,OUTPUT);
  pinMode(dataPin,OUTPUT);
  pinMode(clockPin,OUTPUT);
  length2 = sizeof(tune2)/sizeof(tune2[0]); //计算长度
}

void loop() {
  
  for (int x = 0; x<length2; x++){
    tone(tonePin, tune2[x]);
    delay(300*durt2[x]);
    
    digitalWrite(latchPin, LOW);
    shiftOut(dataPin,clockPin,LSBFIRST,LED0s);
    digitalWrite(latchPin,HIGH);
    
    if(tune2[x] == NTE0){
      digitalWrite(latchPin, LOW);
      shiftOut(dataPin,clockPin,LSBFIRST,LED0s);
      digitalWrite(latchPin,HIGH);
      Serial.println(0);
      delay(200*durt2[x]);
    }
    if(tune2[x] == NTEH1  tune2[x] == NTE1  tune2[x] == NTEL1){
      digitalWrite(latchPin, LOW);
      shiftOut(dataPin,clockPin,LSBFIRST,LED1s);
      digitalWrite(latchPin,HIGH);
      Serial.println(1);
      delay(200*durt2[x]);
    }
    if(tune2[x] == NTEH2  tune2[x] == NTE2  tune2[x] == NTEL2){
      digitalWrite(latchPin, LOW);
      shiftOut(dataPin,clockPin,LSBFIRST,LED2s);
      digitalWrite(latchPin,HIGH);
      Serial.println(2);
      delay(200*durt2[x]);
    }
    if(tune2[x] == NTEH3  tune2[x] == NTE3  tune2[x] == NTEL3){
      digitalWrite(latchPin, LOW);
      shiftOut(dataPin,clockPin,LSBFIRST,LED3s);
      digitalWrite(latchPin,HIGH);
      Serial.println(3);
      delay(200*durt2[x]);
    }
    if(tune2[x] == NTEH4  tune2[x] == NTE4  tune2[x] == NTEL4){
      digitalWrite(latchPin, LOW);
      shiftOut(dataPin,clockPin,LSBFIRST,LED4s);
      digitalWrite(latchPin,HIGH);
      Serial.println(4);
      delay(200*durt2[x]);
    }
    if(tune2[x] == NTEH5  tune2[x] == NTE5  tune2[x] == NTEL5){
      digitalWrite(latchPin, LOW);
      shiftOut(dataPin,clockPin,LSBFIRST,LED5s);
      digitalWrite(latchPin,HIGH);
      Serial.println(5);
      delay(200*durt2[x]);
    }
    if(tune2[x] == NTEH6  tune2[x] == NTE6  tune2[x] == NTEL6){
      digitalWrite(latchPin, LOW);
      shiftOut(dataPin,clockPin,LSBFIRST,LED6s);
      digitalWrite(latchPin,HIGH);
      Serial.println(6);
      delay(200*durt2[x]);
    }
    if(tune2[x] == NTEH7  tune2[x] == NTE7  tune2[x] == NTEL7){
      digitalWrite(latchPin, LOW);
      shiftOut(dataPin,clockPin,LSBFIRST,LED7s);
      digitalWrite(latchPin,HIGH);
      Serial.println(7);
      delay(200*durt2[x]);
    }
    
    noTone(tonePin);
  }  
  delay (2000);
}
```

注意，代码中定义的音阶频率跟上表略有不同，因为不同的蜂鸣器可能振动频率略有差别，所以需要根据实际情况来进行一些调节。

根据同样的原理，可以用蜂鸣器和LED灯来播放任意你喜欢的歌曲。

* * *

