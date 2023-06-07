---
layout: post
title:  "Reducing Analog Pin Noise in Arduino"
categories: programming arduino
tag: arduino
image:
  path: https://images.unsplash.com/photo-1586920740099-f3ceb65bc51e?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1631&q=80
  alt: Photo by Harrison Broadbent at Unsplash
---
<head>
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-4816384316039267"
     crossorigin="anonymous"></script>
</head>

Arduino is one of the most popular and versatile microcontrollers with its user ranged from hobbyists to full-scale industry.
The reason it is so widely adopted by many people is because of its low cost and ease of development. However, when I tried the Arduino myself, I encountered some problems. One of which is the analog pin's sensitivity. It is very sensitive. The data I received from the analog pin can be easily skewed by small things such as touching the analog pin with your finger, turning on other electrical appliances in your house, and even mouse movement when I attached the Arduino to my computer through USB.

**This issue can generally be fixed with these two solutions:**

### 1. Using Pull-down Resistor
Using pull-down resistor, you can significantly decrease the noise by just putting a 22k resistor between GND and the analog pin that you're reading from. For example if you are reading from A0 then put a 22k resistor between GND and A0.

> If you use a much lower resistance than 22k, you will be limiting the analog pin's range of reading. For example, if you use a 5.5k resistor between GND and an analog pin, that analog pin now only reads from 0.0v to 3.47v, the normal reading is usually between 0.0v to 5.0v.
{: .prompt-warning }

### 2. Using averaging algorithm
If the above method is still not enough to eliminate the noise, you can also combine it with this one, where we can tell Arduino to only take the average value of an analog input using the simple arithmetic mean formula which is to just sum the total of your data and then divide it by the number of data you have.

In this code we can specify how much data we want to take from the analog pin via the parameter `int sample`

```c++
int average(int pin, int sample)
{
  float values[sample] = {};
  float total = 0;
  
  for (int i = 0; i < sample; i++)
  {
    values[i] = analogRead(pin);      // take x amount of data from the pin specified
  }
  for (float value : values)
  {
    total += value;                   // sum of all the data we gathered
  }
  return (int)(total / sample);       // divide the sum by the amount of data
}
```

The end code would look something like this:

```c++
void setup() {
  Serial.begin(9600);
}

int average(int pin, int sample)
{
  float values[sample] = {};
  float total = 0;
  
  for (int i = 0; i < sample; i++)
  {
    values[i] = analogRead(pin);
  }
  for (float value : values)
  {
    total += value;
  }
  return (int)(total / sample);
}

void loop() {
  Serial.println(average(A2, 200));     // get the average value of analog pin 2 with 200 samples
}

```
