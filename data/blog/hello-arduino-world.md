---
title: Hello Arduino World
date: '2022-04-19'
tags: ['Arduino', 'Microcontrollers', 'C++', 'beginners']
draft: false
summary: Arduino is a Microcontroller board that can be used to build robots and smart devices. This blog post goes over the hello-world of Arduino, a blinking LED.
images: []
layout: PostSimple
canonicalUrl:
authors: ['default']
---

## Introduction

We are surrounded by smart devices that help us navigate the world. Some devices tell the current temperature while others open doors when it detects a human. These devices use sensors to operate and embedded code to operate. One of such suite of devices is Arduino. Arduino is a microcontroller board that can be used to build smart devices. You will learn how to build a blinking LED with arduino in this tutorial.

## Prequisites

1. A tablet or laptop
2. A TinkerCAD account. Create one [here](https://www.tinkercad.com/join)

TinkerCAD is a virtual environment where you can create arduino circuits and 3D designs. It's ideal if you want to quickly prototype a circuit.

## Setting up a Blinking LED Project on TinkerCAD

1. Navigate to the circuits menu on your TinkerCAD dashboard
   ![TinkerCAD Dashboard](https://i.imgur.com/CqJV9BU.png)

2. Click on the create circuit button. This should navigate you to the environment where you can create circuits.

![Environment for creating circuits](https://i.imgur.com/lQYDk8i.png)

3. Drag and drop a breadboard, an LED, a resistor, and an Arduino UNO board from the components pane on the left into the work area

![Drag and drop](https://i.imgur.com/jBPXe9m.png)

4. Connect the resistor vertically from the positive terminal on the breadboard to a point where it stops and also connect the LED to be colinear with the resistor. Note that the positives have to connect with each other and the positive leg of the LED is the longer or bent leg.

![Resistor connectivity](https://i.imgur.com/a7uCLG9.png)

5. Connect the positive lane of the breadboard to terminal thirteen (13) on the Arduino and the shorted leg of the LED to ground of the ardduino (GND).

![Connecting the breadboard to the Arduino](https://i.imgur.com/qb3tP0w.png)

6. Now, click on the Code button from the nav bar and change the Edit Mode to text only. You will be writing C++ code in this section. Don't worry if you haven't written C++ before, everything will be explained.

![Code button](https://i.imgur.com/qSK4Qed.png)

## Basics of Arduino Programs

- Why is code necessary
- Most basic code structure

Code is what makes an Arduino setup smart. Arduino programs are written in C++ and must be compiled and uploaded to the board for it to execute.

### Program Structure in Arduino

A basic ARduino program consists of a setup function and a loop. Things like input and output settings are done in the setup function while things like when a LED should light up are done in the loop. Below is an Arduino program with both the setup and loop functions.

```cpp
void setup(){
   // set up inputs and outputs
}

void loop(){
   // determine when to send power to components
}
```

## Logic Flow in a Blinking LED circuit

Arduino has an input-output (I/O) system that depends on pins. Pins are the portals to which current flows in or out. Before such current can flow, the pin needs to be explicitly set for the operation in the setup function. There are three sets of pins:

1. Digital pins
2. Analog pins
3. Power and Ground pins

![Digital Pins](https://user-images.githubusercontent.com/40396070/164608999-cb3fbae6-51f1-4b2a-960a-80c645f0b480.png)

You will be using the digital pins to send power from the board to your components. Find below a basic program that sets pin 13 as an **output** pin.

```cpp
int activePin = 13;

void setup()
{
  pinMode(activePin, OUTPUT);
}
```

The snippet above will cause current to flow from pin 13 to the connected circuit. The connection is made to the positive line of the breadboard.

![Positive line of circuit](https://user-images.githubusercontent.com/40396070/164609081-0c6d019e-fee7-44fd-8de3-1d79f66acad7.png)

The circuit is completed by connecting it to a ground pin. Now that you know how current leaves the Arduino, you need to set how the LED is lighted. You'll achieve lighting by setting a digitalWrite on an output pin. In this case, you'll call digitalWrite on pin 13. Find the implementation in the snippet below.

```cpp
int activePin = 13;

void setup()
{
  pinMode(activePin, OUTPUT);
}

void loop()
{
  digitalWrite(activePin, HIGH);
}
```

The second parameter passed to the digitalWrite function sets it up for high voltage flow. This means five (5) volts will flow from pin 13 to the connected circuit. If you check your circuit, you should see a lighted LED. You may also notice a lighted LED on the Arduino board. This LED shares the same connection with pin 13 so will light up too.

### Making a blinking LED

The most basic Arduino program cannot be complete without some dynamism, else, it's just a dumb circuit. The idea of the loop is that some fixed amount of current flows through the circuit per unit time in the loop. So you will achieve blinking if you delay the flow by a second, stop the flow, then delay for another second before resuming the flow.

```cpp
void loop()
{
  digitalWrite(activePin, HIGH);
  delay(1000);
  digitalWrite(activePin, LOW);
  delay(1000)
}
```

In the end, you'll have a two second duration with a lighted LED for one of those seconds. Note that time in Arduino programs is represented in miliseconds and that 1 second = 1000 miliseconds. You should always use variables to make your programs easier to modify. You can achieve by setting the delay time to a variable called delayTime.

```cpp

int delayTime = 1000;

void loop()
{
  digitalWrite(activePin, HIGH);
  delay(delayTime);
  digitalWrite(activePin, LOW);
  delay(delayTime)
}
```

## Full Code and Circuit Diagram

### Full Code

Find below the full code that you can copy and paste directly into your Arduino editor.

```cpp
int activePin = 13;
int delayDuration = 1000;

void setup()
{
  pinMode(activePin, OUTPUT);
}

void loop()
{
  digitalWrite(activePin, HIGH);
  delay(delayDuration);
  digitalWrite(activePin, LOW);
  delay(delayDuration);
}
```

### Circuit Diagram

Find below the schematic diagram for the circuit.

![Circuit Diagram](https://user-images.githubusercontent.com/40396070/164607685-163be6b0-86af-4fd3-b0c6-5a9e5f3a9996.png)

## Conclusion

Microcontrollers bring some intelligence to circuits. Being able to automatically control how and when current flows in a circuit could make the difference between a flashlight and Mars rover (haha).

In this tutorial, you learned:

1. How to set up a circuit on Tinkercad with a Breadboard, LED, resistor and Arduino UNO
2. How programs work in Arduino
3. How to make an LED blink in an Arduino circuit

You could go further to tinker with different example circuits provided by Tinkercad. Also check out the other tutorials on my blog. Thanks for reading. Adios ✌🏾🧡.
