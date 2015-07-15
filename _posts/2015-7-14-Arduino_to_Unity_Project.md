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
  - name: 5.png
    thumb: t5.png
    text: Sketch of 8-directional joystick.
  - name: 6.png
    thumb: t6.png
    text: Sketch of 8-directional joysticks directional vectors.
  - name: 7.png
    thumb: t7.png
    text: D-Pad
  - name: 8.png
    thumb: t8.png
    text: Joysticks/Arcade sticks
  - name: 9.png
    thumb: t9.png
    text: Dual analog sticks
  - name: 11.png
    thumb: t11.png
    text: WASD vs Dual analog Comparison
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
1. An Arduino Uno microprocessor
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

#Part 3:Components and soldering

I ordered some new components for my project:

- 2 x [thumbstick joysticks](https://www.sparkfun.com/products/9032)
- 2 x [break out board(for the joysticks)](https://www.sparkfun.com/products/9110)
- 1 x [connecting headers](https://www.sparkfun.com/products/116)

{% include gal.html image="2.png" %}

{% include gal.html image="3.png" %}

Then I connected the thumbstick to the arduino as described [here](http://42bots.com/tutorials/arduino-joystick-module-example/)

{% include gal.html image="4.png" %}
 
The sketch worked fine. I noticed that both potentiometers had a range of 1-1020. For example, pushing the thumbstick all the way forward showed 1020 on the X-axis and 510 on the Y-axis. I wanted to define 8 directions on the thumbstick input so I wrote a sketch that printed a number depending on within which range it was.

{% include gal.html image="5.png" %}

The result:

<iframe src="https://player.vimeo.com/video/129476287" width="500" height="888" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

#Part 4: Thumbsticks 8 directions input to Unity
I rewrote the Unity C#-script I wrote when I tested with just one button, to act differently on 8 different states(+ 1 state for doing nothing) instead of just on/off that I had for the button. Each number indicates which direction vector is to be used in Unitys tranform.Translate()-function. When we move, we want to move along the x-axis and z-axis but be constant on the y-axis. So the vectors are as follows:

{% include gal.html image="6.png" %}

I then added the C#-script to the first person camera

<iframe src="https://player.vimeo.com/video/129476213" width="500" height="888" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

#Part 5: State of the art controller conventions
As of part 4 I have created a 8-directional joystick input from an Arduino to Unity. I have created a script to read the Arduino input over serial in Unity and added that script to control the movement of the FPS Camera.

The next natural step of my project according to the limit-section of my project specification would be to add a second joystick to control camera rotation of the FPS camera in Unity. However, playing around with my 8-directional joystick I started thinking about game controller conventions. How many directions should I be able to move in? Is 8 directions sufficient? This blog is a brief study on game controller conventions.

###D-Pad
The d-pad(short for directional pad) was an early game controller convention, used on home gaming consoles such as the NES, SNES and Playstation among others. The d-pad have four digital buttons which each gives input of one direction (UP, RIGHT, DOWN, LEFT). However in some implementations of the d-pad, the user can get diagonal directions by pressing two buttons. For example pressing both UP and RIGHT would result in a direction input of a diagonal direction between up and and right. Today these are mostly used in 2D-plattform games. The user input in most games a quite straight forward. The user presses the button in the direction he/she wants to move the character and the character moves in that direction.
{% include gal.html image="7.png" %}

###Joysticks/Arcade sticks
Joysticks or arcade sticks are a directional controller type that consists of a single stick which gives a digital input depending on its position. Joysticks are available in 2-way, 4-way and 8-way set ups, describing the amount of directions possible to move. Today these are mostly used for fighting games where you want to execute exact button combinations do perform in-game combo attacks.
{% include gal.html image="8.png" %}

###Dual analog sticks
Modern home gaming consoles often uses the convention of two dual sticks. The sticks placed on a controller held with both hand and each thumb is resting on one thumbstick. The convention is that the left thumbstick is controlling the character movement and the right thumbstick is controlling the camera rotation. For a first person perspective game this means that the user walks by pressing the left thumbstick in some direction and control the direction the character is turned towards with the right thumbstick. The user input of analog sticks enables user input in any direction (360 degrees) for each stick, meaning that the user can move/rotate in any possible direction. 

This user input requires some training to get accustomed to. Novice user sometimes think it difficult to control these two different movement systems at the same time (plus a dozen other digital buttons usually on these modern controllers). 
{% include gal.html image="9.png" %}

###Keyboard(WASD) + Mouse
On PC the controller convention is to control the movement of the player character with the keyboard and rotation of the camera with the mouse. For a first person game this mean the user walks with the help of four digitial buttons on the keyboard (W, A, S, D) and controll the direction the caracter is turning to with the mouse. This is interesting, because the keyboard + mouse controller conventions only have 4 directional input instead of 360 degrees as the dual analog stick. However often the same game are available over a range of platforms where both these conventions are present. I ended thinking about this a lot, until I decided to do a small comparison.
{% include gal.html image="10.png" %}

###WASD vs Dual analog Comparison
I tested controlling a first person perspective game first with a WASD + mouse and then with a dual analog stick controller. The "game" I tested with WASD was the Unity FPSCamera. With WASD you can only mouse in four directions. The convention is that you move with the same speed in all directions. Moving sideways by pressing A or S and holding the mouse relatively still makes you move sideways while not turned that way. In real life this movement would be called side-stepping but in games it is called "Strafing". This movement is unnatural since in real life a human can not side-step without decreasing movement speed. In game however, strafing is a common movement convention. However, the the mouse is used to rotate the camera. Moving the mouse forward/backwards results in a motion that in the real world would be moving the head up and down. Moving the mouse left or right represents a motion that by real world terms could be described as turning the whole body in that direction. This means that pressing the digital button W on the keyboard will move the camera forward, however, by turning the mouse to the left/right, changes the rotation of the camera and therefore the direction that is forward. In this way, the WASD + mouse conventions can still move in any possible direction (360 degrees)

The game I tested with a dual analog stick was Bioshock Infinite and I used a Playstation 3 controller.
In comparison to the WASD for movement control, the left thumbstick enables the user to move in any direction(360 degrees). I am not sure if the analog controller input is divided into zones or if it is really completely "any direction", but the user experience is that you can move in any direction. In other words, the control is sensible enough that I can move in a direction of 35 degrees or 45 degrees. However, playing the game I noticed that I am do not give very exact inputs down to a certain direction degree. Rather I tend to move forward and backwards and strafe left and right, occasionally moving diagonally, oftenthe 4 direction convention of the WASD. To do exact movement, instead I use the right stick to turn the direction of "forward", just as I would to with the mouse.  However, the right stick movement is not as quick and exact as using a mouse, so my experience was that I am compensating the slowliness of the right stick, by also using the left stick(more than I would WASD) to adjust movement direction. The combination of the the dual sticks then is a quite complex user input, both controlling the camera movement and the right stick also controlling camera rotation. It is not entirely obvios how to understand the coagency of both thumbs in this set up. I find it very complex and intersting in a way that I have not thought of when previously playing video games.

The camera rotation of the right stick can be made faster/slower depending on how far the analog stick is pushed in a certain direction. If I push the right thumbstick a litte to the right the camera will slowly rotate that way, however If I push it further the the camera rotation will be faster. The user experience is that the change of speed is not in steps but continues. I also noted that the maximum rotation speed UP/DOWN was set to much slower than rotation LEFT/RIGHT in the particular game I tested with(Bioshock Infinite).

{% include gal.html image="11.png" %}

###Conclusion 
I will use the dual analog stick convention  and try to use the interaction and user expereience from the control of Bioshock infinite to shape my user input.


{% include galheader.html %}
