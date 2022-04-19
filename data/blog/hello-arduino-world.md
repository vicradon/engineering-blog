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
