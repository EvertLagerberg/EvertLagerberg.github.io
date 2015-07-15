---
layout: post
title: Arduino to Unity Project
imgfolder: /images/Arduino_to_Unity
images:
  - name: 1.png
    thumb: t1.png
    text: Wiring Diagram to turn LED on and off with button
  - name: 2.png
    thumb: t2.png
    text: The thumbstick consist of two potentiometers one for horizontal movement (X-axis) and one for Vertical(Y-axis). You can also press down on the thumbstick to press a button. 
  - name: 3.png
    thumb: t3.png
    text: I soldered the thumbstick to the breakout board and then soldered on a 5pin header to be able to connect cables to the break out board.
  - name: 4.png
    thumb: t4.png
    text: The same link also provided a example sketch to print out the values of the horisontal and vertical potentiometers. 
---
#Alternative input modalities for interaction in Unity

#Part 1: Project Specification

###Background
Unity is a powerful open source game engine, which comes with many helpful features for creating interaction. For example, to simulate that the player is controlling a character in a first person perspective, we can just drop a camera object into our Unity scene. This gives us the functionality to move the placement of the camera around in the scene with the arrow buttons of the keyboard, and rotate the camera around  the Y and X axis’s with the mouse. But what if we wanted to design how the physical user interacts with the game world. What if we wanted to use other physical sensors or other input modalities to interact with the game world?

Ardouino is a open-source platform which consists of a circuit board with a programmable microprocessors and various sockets for input and output. The inputs can be used to connect sensors to the arduino and the output to connect actuators. Through the Arduino IDE we can program the microprocessor to tell it how to react on sensor input. Arduino allows us to quickly build prototypes for interactive electronic objects.

###Problem
Investigate how the Arduino platform can be connected Unity to send input data from the sensors connected to the Arduino. Evaluate the usability(accuracy, satisfaction, stability) of a hardware prototype input system based on a Arduino-to-Unity-connection.

###Project plan:
1. Acquire the necessary hardware components
2. Solder and connect cables to set up hardware prototype
3. Investigate how to have the Arduino IDE send input data to Unity, then implement.
4. Investigate how to connect input data to controls in unity. Then implement.
5. Investigate how to control objects, cameras in a Unity scene.

###Evaluation:
The project will focus on creating a proof of concept prototype of a hardware input system to control objects and camera in Unity. The evaluation of this system will primarily be a technical documentation of how well the connection from Arduino to Unity works. If the prototype works well enough I can do user tests with 1-2 user and interview them about usability.

###Limits:
I will start with trying to implement the control of the camera in unity with a thumb stick connected to the arduino (Like an analog stick of a modern game console). The thumb stick can be moved in Y and X axis so this input can be used the camera in for directions. If I have time I will implement a second thumb stick to control rotation of the camera around the Y and  X axises. If I have more time I would like to add other sensors and use them as controls, for example a pressure sensor to act as a button.

#Part 2: Testing,testing..
To start off my project I wanted to do a quick and dirty session with the goal to set up a connection between the Arduino IDE and Unity. For this session I had the following equipment:

Hardware:
1 An Arduino Uno microprocessor
2. A Breadboard and Cables
3. a 3-Pin Button
4. a LED-lamp

Software:
1. Arduino IDE
2. Unity 4

###Arduino to Serial:
I started by setting up a simple Arduino sketch for using the 3-pin button as a sensor to turn the LED-lamp OFF/ON. I used an example sketch available in the Arduino IDE called Button. There is a tutorial for it [here](http://www.arduino.cc/en/Tutorial/Button).

For the wiring I simply used the schematics provided from the retailer where I bought the button
{% include gal.html image="1.png" %}


I edited the arduino sketch from the tutorial as to also printed the "1" if the button was pressed and "0" if it was open.

<iframe src="https://player.vimeo.com/video/129361406" width="500" height="281" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

###Part 3:Components and soldering

I ordered some new components for my project:

- 2 x [thumbstick joysticks](https://www.sparkfun.com/products/9032)
- 2 x [break out board(for the joysticks)](https://www.sparkfun.com/products/9110)
- 1 x [connecting headers](https://www.sparkfun.com/products/116)

{% include gal.html image="2.png" %}

{% include gal.html image="3.png" %}

Then I connected the thumbstick to the arduino as described [here](http://42bots.com/tutorials/arduino-joystick-module-example/)

{% include gal.html image="4.png" %}
 
The sketch worked fine. I noticed that both potentiometers had a range of 1-1020. For example, pushing the thumbstick all the way forward showed 1020 on the X-axis and 510 on the Y-axis. I wanted to define 8 directions on the thumbstick input so I wrote a sketch that printed a number depending on within which range it was.

{% include gal.html image="4.png" %}

The result:
<iframe src="https://player.vimeo.com/video/129476287" width="500" height="888" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>




{% include galheader.html %}
