---
layout: post
title: Arduino to Unity Project Continued
imgfolder: /images/Arduino_to_Unity
images:
  - name: 12.png
    thumb: t12.png
    text: For some reason the horizontal scale grows to the left
  - name: 13.png
    thumb: t13.png
    text: From the inside. The arduino connected to the computer via a serial-to-usb cable running out of the box. The analog sticks are connected to the ports of the arduino. I also had to use a breadboard since I now wanted to connect both analog sticks to the 5V and Ground ports of the arduino.
  - name: 14.png
    thumb: t14.png
    text: From outside. This allowed me to hold the box with one hand on each side and resting each thumb och each thumbstick
---

Continuation of my project with Arduino and Unity, adding a second analog stick and connecting it to a Unity scene
<div id="toc"></div>

#Part 6: From zones to direction vectors

In my previous implementation of the Arduino directional input to Unity I divided the scales of the Y and X-axis ranges and gave each zone a number 0-8. This way I only needed to send that number over Serial to Unity. But that also meant that I only could input 8 directions(+ 0 for standing still). After doing some state of the art research I decided that this was not enough. I wanted to be able to input any direction (360 degrees).

To do this we can picture the the analog stick in a coordinate system where the rested position of the thumbstick is in the origo(x1, y1). If I then move the thumb stick in any direction that is going to change the potentiometers value so that I get an x2, y2 position.

v = x2-x1, y2-y1 then then gives me a vector which goes from origo to the new position of the controller. And if I normalize that vector I get a directional vector which is vector of length one that describes a direction. I could then use Unitys transform.Translate()-function to move an object in that direction in real time, following the user input of X and Y values over the serial port.


###Setting up a coordinate system.
First I had to reverse the range of the X-axis from the thumb sticks horizontal potentiometer. For some reason it was made so that the vertical scale was growing upwards but the horizontal scale was growing to the left. To get this to work as a conventional coordinate-system I needed to reverse it so that the X-axis was growing to the right.

{% include gal.html image="12.png" %}

Next I calculated the delta X and delta Y. Now the axis range from -510  to  510 with 0 in the middle.

###Sending data over serial
Previously I only needed to continuously send number over the serial port to Unity. I could do that with Serial.write() in the Arduino IDE and read it with ReadByte() in Unity. So I was sending one byte with each package. However, 1 byte can only hold a value 0-255. My values where between -510 and 510 and both for the X and Y axis which meant it would no longer fit inside a byte. From reading on line it seems possible to send multiple bytes with the Serial.write()-function, however I am not sure how I would receive that without knowing the exact number of bytes sent. I could also try to split the data up into several packages but that would require some algorithm to check when all the parts of a package has been received. Instead I used the Serial.print()-function which prints "Prints data to the serial port as human-readable ASCII text." as it says in the Arduino documentation. I sent the X and the Y value separated by a "," and then I could use ReadLine() in Unity and then split and parse to get the values. This certainly does not seem like the most elegant method of sending numerical data(converting int to string, converting back to int) but I am yet to find a better way nor anyone else who have done it. Truth is that information on Arduino to Unity Serial communication is quite limited online.

I wrote a C#-script in Unity to read the serial data, split and parse it to X and Y valuebles. Then I created a Vector3(X,0, Y) and normalized it to get a directional vector to describe the direction that I wanted to move the character (in this case the Unity FPScamera). This scrips seemed to work fine. However I had some problems with the serial communication. It seemed that there was a sync problem between the arduino sending code and the unity receiving. My theory is that ReadLine() takes longer than ReadByte() which is why the serial connection are not as direct (write -> read). This causes a async problem. I had to play around with delay(ms) on the Arduino side and ReadTimeout() on the Unity side to make them sync a little bit better. However, if the delays where too high that would gravely affect frame rate in my Unity scene. I finally found out that 4ms delay in arduino and 5ms ReadTimeout() in Unity seemed to work OK. I also had problems with Unity sometimes not parsing correctly and I have a lot of different exceptions when running. I need to optimize the code. But this is the version I have at this point:

<iframe src="https://player.vimeo.com/video/129597164" width="500" height="281" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

It works OK. Framerate is about 60 FPS and acceptable. It is now possible to move in any direction instead of just 8.

#Part 7: Something to hold on to

As I was testing my analog stick, since it was not attached to anything, I needed to hold the breakout board with one hand and steer the analog stick with the other hand. The next step in my project was to add a second analog stick at which point it would get impossible to hold everything still. I needed to build some kind of stand or board to fasten my analog sticks.

The aesthetics and ergonomic of how the input sensors of a interactive controller are placed is of course very important for the user experience. If I choose to continue this project I would want to design a more embodied interaction. At this stage of the project though, I really just needed something that would allow me to place each of my thumbs on the left and right thumbsticks.

Since my arduino wires are quite short my best option was to somehow build in the arduino together with the thumbstick sensors and then have the much longer serial to usb-cable that connects a arduino to a computer, run out of the controller. I found a carton box of the right size. I placed the arduino inside of the box and cut holes in the roof of the box for cables so that I could have the breakout board and the thumbstick outside and on top of the box. I used blu-tack to secure everything. Not a very permanent solution but it worked for now.

{% include gal.html image="13.png" %}

{% include gal.html image="14.png" %}

#Part 8: Adding a second analog stick

Once I knew how to get input from one controller, send it to Unity and use it in a script there, it was very easy to replicate. I just connected the second stick to the arduino just like I connected the first one. The sent data from both sticks over serial. In Unity I only had to make it so that position data from the right stick was not used to do tranform.Translate() in unity but instead [transform.Rotate()](http://docs.unity3d.com/ScriptReference/Transform.Rotate.html)

For moving the FPSCamera-object right or left, I sent a vector of (0, Righstick X-position, 0) to  rotate around the Y-axis. To get this right, I also had to state to the function to rotate in relation to the world(the scene) y-axis and not the object itself.

Then I repeated this function call but for rotation around the X-axis in order to move the FPSCamera-object up/down.

This was the result (sorry for bad filming, It starts to get really hard to film and control the two thumbsticks at the same time!)

<iframe src="https://player.vimeo.com/video/129807848" width="500" height="281" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

#Part 9: Adding to the scene + jumping

###Adding to the scene
I had now added two analog thumbsticks as input and could could use them to control a FPS Camera in Unity. However the very simple Unity scene I had created where making it difficult to se how well this worked. To be able to improve and adjust the details of the interaction I needed a better scene. I went to the [Unity Asset Store](https://www.assetstore.unity3d.com/)

This store is great, you can buy assets and directly from the store import them into you Unity project. Since this was a school project, I chose a scene which someone made available on the [store](https://www.assetstore.unity3d.com/en/#!/content/6459) for free.

As described in earlier post, adding a script to an object is as easy as dragging it on to it. So after opening the scene in Unity, all I had to do was drag my C# script on to the first person camera included in the scene and then I could start using it with my controller just as in my previous much simpler scene. Great!

However, this scene had terrain. The ground was not at the same level everywhere but the where hills and valleys. As long as I was moving to lower levels everything was fine. But when I tried to go up a hill, to a higher levelled terrain my character would fall through the terrain and of the whole plane to an endless fall. As I have mentioned before, I am a novice in Unity. I started investigating if I had to add some kind of physics component to my FPS Camera object. There is something called "Rigid Body" and something else called "Collider". I was a little bit confused by this because whatever I added, I still fell right through. I tested to control the FPS Camera with the mouse and keyboard again and I did not fall through terrain, so it had to have something to do with my script.

I compared the movement of the FPS Camera Object using the mouse & keyboard vs my controller to see how they differed and then understood something that might be very obvious to someone with more experience with Unity: The FPS Camera asset object that comes with Unity really consist of two sub objects called Graphics and Main Camera. When moving the mouse left to right that rotates the whole FPS Camera object. However when moving the mouse forward/backwards only the Main Camera sub-object is rotated. So to replicate that action with my controller I needed to access the camera sub-object. You can do this by the function GameObject.Find() and then use "/" to move down in the hierarchy of you game object. So in my case it was :
cam = GameObject.Find ("First Person Controller/Main Camera");

This worked!

###Jumping
However, as I was investigating physics and terrain in Unity I found this [checklist](http://answers.unity3d.com/questions/147687/checklist-object-or-character-is-falling-through-t.html) for people with problem with falling with terrain:

It says that it is bad practice to use transform.Translate() in a script to move a character and it is better to use [CharacterController.SimpleMove](http://docs.unity3d.com/ScriptReference/CharacterController.SimpleMove.html). It turns there are built-in functions to do what I was thinking up how to do in Unity(of course there are!) I ended up using [CharactedController.Move](http://charactedcontroller.move/) instead and I could pretty much replicate the example script given on the Unity documentation [page](http://docs.unity3d.com/ScriptReference/CharacterController.Move.html).

This example script also came with a jump button implementation. The CharacterController-class of Unity there is boolean [.isGrounded](http://docs.unity3d.com/ScriptReference/CharacterController-isGrounded.html) to check if the character is on the ground or not. We check if the character is on the ground(you can't jump if you are not on the ground in this implementation) then we check if the button of the right analog stick is pressed. I think I mentioned this earlier, but the thumbsticks input sensors more than having the two analog potentiometers also have a digital button which switches between 0 and 1 when you press down on the analog stick. So if the character is on the ground and the button is pressed, we use the [CharactedController.Move](http://charactedcontroller.move/)  to move the character along the y-axis resulting in a "jump". Then we move the character down again over time, which represents the end of the jump when gravity is returning the character to the ground.

#Part 10: Adjusting controls

To get a smoother and better control I did two adjustments


###Movement/Rotation Speed
I removed the normalization of the direction vectors used for movement/rotation. That way, the vector is bigger/small depending on how far you push the thumbstick in some direction. This means that if you just push the thumbstick a little bit you will for example rotate very slowly but if you push it harder the rotation will be faster. This enables the player to do both high-precision and fast turning/moving input.

###Deadzone
One thing I have not described is the deadzone. On a analog controller that can be moved along the X and Y axises, the analog stick should in its initial state (not pushed) be at origo. In my case each thumbstick is in the origo of a coordinate system which goes from -510 to 510 on both axises. So in the initial state, the value of the thumbstick potentiometer should be 0,0. But since its analog and has it is not exactly at 0,0. It might be -20 or +30 off. This position can be detected and understod by my script as a direction vector and will cause unwanted movement/rotation. To avoid this, I created a deadzone, meaning that if a value from the potentiometer is in the range of: -35  < val < 35 it is interpreted to 0.

Today I read a very interesting article/blog about how to program deadzones. It is written about a professional game develop and seems legit. Very interesting, you can read it [here](http://www.gamasutra.com/blogs/JoshSutphin/20130416/190541/Doing_Thumbstick_Dead_Zones_Right.php
)

I learned from this article that a better deadzone is implemented by using the magnitude of the vector insted of just the vector x and y values. I changed my script to a solution based on this implementation and the result was somewhat smoother.

#Project presentation

<iframe src="https://player.vimeo.com/video/130634142" width="500" height="281" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="https://vimeo.com/130634142">DIY Arduino Dual Analog Stick Controller</a> from <a href="https://vimeo.com/milionstylez">Evert Lagerberg</a> on <a href="https://vimeo.com">Vimeo</a>.</p>


[Video demo](https://vimeo.com/130634142)

[Github](https://github.com/EvertLagerberg/Dual-Analog-Stick-Controller-for-Unity)

[Wiring Diagram](https://github.com/EvertLagerberg/Dual-Analog-Stick-Controller-for-Unity/blob/master/WiringDiagram.png)


My goal with this project was to investigate how analog input sensor data could be transferred from Arduino to Unity over a serial port. I wanted to see if this could be done satisfactorily enough to control an in-game character's movement. I found that it was quite hard to find 1) online tutorials for what I wanted to do and 2) similar projects. The most notably project I have found is a Library/Plugin to Unity called [UNIDUINO](https://www.assetstore.unity3d.com/en/#!/content/6804)which can be bought in the unity store for 30 USD. As I understand, the UNIDUINO library has an Arduino class which enables controlling the Arduino directly from within a Unity C# -script. I choose not to use UNIDUINO since I wanted to make something open source and free. There also seemed to be a lot of “automagical” functions might not allow a complete understanding of workflow under the hood. Instead I have gathered various resources that have helped me solve parts of the project.

For finding and assembling hardware components I used this [tutorial](https://www.sparkfun.com/tutorials/272).
For Wiring Diagram and a basic Arduino IDE sketch for the thumbsticks I followed this [tutorial](http://42bots.com/tutorials/arduino-joystick-module-example/).
For basic understanding of how to send data from Arduino to Unity over serial I followed this [tutorial](https://www.youtube.com/watch?v=of_oLAvWfSI).
For writing scripts in Unity and how to move/rotate objects and cameras I have used the [Unity Documentation](http://docs.unity3d.com/Manual/index.html).

More than that, I have read dozens of forum threads to understand parts of this project. 
After doing a lot of research for this project I have found that there is definitely a lack of documentation of the whole process of using Arduino input sensors to interact with Unity.

###Project Process:
The process has gone quite smoothly, and I have been able to solve the problems that have come up relatively quickly. I am familiar with the Arduino platform but I am a novice at best in Unity. During this project I have understood that Unity is a very powerful and excellent tool to create interaction and games. This have been both a positive and a negative experience. Positive when I find built-in functionality that makes it easy to do what I want. Negative when my lack of knowledge of the quite complex Unity interface confuse me. Sometimes I have been wondering if I am doing something the “right way” or if my lack of knowledge of the Unity workflow makes me reinvent the wheel a few times over. I have learned a lot about Unity during this project and I am looking forward to using it again in future projects. 

Other than problems with drawing wires and programming the scripts, it has also been very interesting to think about the dual analog stick controller convention of video games. Early in the process I decided to play a first person perspective game with a PlayStation 3 controller to get a feel for the dual analog controller. I found that interaction of moving a imagined player character with  the left stick and controlling the movement of the head of the imagined player character with the right stick is quite complex. Having played this kind of games, with this interaction convention for many years, I still find it quite hard to describe how the two interactions inputs are used as one. There are also several conventions to this interaction that the player is fully to partly unaware of. For example different speeds of vertical and horizontal rotation of the player characters head, or the implementation of a deadzone. It is very interesting to think more about how tangible interaction can be very complex, but quite simple for a user familiarised with the specific input convention.

###Outcome
Please follow this link to see a video demonstration to see the final version of my project. 

I have succeeded in using sensor input data from an Arduino to build a custom controller script in Unity. In my implementation you move the character in any direction by pushing the left thumbstick. How far from it’s initial position you push it determine the speed of movement of the character, enabling you to both very small and precise movements and quicker movements. Pushing the right thumbstick will rotate the camera of the character in that direction, again rotating fast or slow depending of how far you push the joystick. By pressing the right thumbstick button down you can make the character jump and then fall back towards the ground.

All this interaction is created from six variables sent over serial port from Arduino to Unity: 
1. Left thumbstick x-position
2. Left thumbstick y-position
3. Right thumbstick x-position
4. Right thumbstick y-position
5. Left button state(not used)
6. Right button state

1-4 are three digit numbers in the range of -510 < val < 510 which means that they will not fit in a byte. The Arduino IDE allows sending of data over serial port as bytes, array of bytes or strings(then converted to bytes). In my implementation I send lines of strings that are then in Unity split and parsed to integers. However, reading a string from the serial port takes time and since the the Arduino is sending data continuously over the serial port this can cause asynchrony problems. To make this work smoothly I added some delay(4 ms) at the end of each loop in the Arduino script and a maximum time(5 ms) for Unity to read in a line of strings. This delay causes a drop in frame rate in Unity. I still managed to keep it above 60FPS in a rudimentary scene and above 30FPS in a fully texturized scene with terrain, running on a Macbook air. 

The transfer of data in my implementation is quite stable which makes the interaction feel smooth and instant. The implementation of speed of movement/rotation depending on how far the thumbtack is pushed, makes it possible to performs accurate movement of the camera. 

###Evaluation
Technical evaluation: The prototype controller can be used in a fully textured Unity scene with terrain and run with over 30 fps on a Macbook Air. There is a delay but the a 30 fps fram rate on a relatively low power computer shows that it should not affect the user experience. There is almost no delay between user input and corresponding action in Unity. The movement is smooth without shake and jerks. The movement/rotation speed depending on how far the thumbtack is pushed makes it possible to perform movements with high accuracy.

User test: I did a very limited user study with one user. This user had extended experiences playing first person perspective - video games with both WASD + mouse -input and dual analog stick gamepads. I used the talk aloud methodology where the participant are asked to voice their thoughts while performing specified tasks. I did not tell the user beforehand how the game was controlled. The only instructions given to the user was how to hold the controller correctly so that the thumbs rested on the thumbsticks. The user immediately understood that the controls where according to a dual analog stick movement convention and started moving around the environment. The users first voiced impression was that the movement speed of left thumbstick was to high and that a quick push would move the character further than wanted/expected. However, in about 30 seconds more of testing the user understood that movement speed depended on how far the analog stick was pushed. After this feature was explained the user was more comfortable. Within minutes the users had adapted to user the controller without problems. The user also gave feedback that he felt that he should be able to move the character even when being in the middle of a jump. He stated that even if you realistically could not move around and strafe in the middle of a jump in real life, his impression was that you can do that in many similar games. During no part of the test did the user voice frustration over technical or interaction problems. The impression is that the user found the experience satisfactory.


###Discussion
I have made a proof of concept implementation sending sensor input data from Arduino to Unity and using it there to control a character. There are several ways to improve or further develop this project.

As mentioned in this report, the transferring of data was done by sending lines of strings over a serial port from Arduino to Unity. It is possible, that the transfer of data can be optimized to increase frame rate in Unity. I chose to not focus too much on such optimization since I wanted to spend more time improving the user interaction. 

The First Person Camera- asset that comes with Unity is a quite elaborate object with many built-in functions which I have only touched upon. Rather I drag my script on top of the existing object. It would be interesting to see how the result of my project could be better integrated in the First Person Camera –object’s functionality.

Unity is compatible with many USB-gamepads such as for example a PlayStation or Xbox Controller. There are no obvious benefits from building your own dual analog controller as shown by the result of this project. Hence, this project should rather be understood as a proof of concept that any sensor input data to an Arduino could be used to interact with Unity. There exists a wealth of such Arduino compatible sensors on the market today and electronic web stores sell them bundled with tutorials, often for a very small price. Possible input sensors are for example accserolometers/gyros, biometrics sensors, flex/force sensors, temperature, weather and light/sound sensors. The Arduino platform allows users to build DIY-user input interactions systems to interact with Unity. In continuation this allows Unity to be a platform not just to create digital experiences but embodied digital experiences, moving outside of the screen. My project shows that this is possible. Dual analog stick input is probably more complex data than input from most of the type of sensors mentioned above. My hope is that I myself and others can use the platform I have built for Arduino to Unity communication to create more interesting and innovating game interaction without much investment. My project is open source and available publicly on github, featuring a readme with instructions, Arduino and Unity code and a wiring diagram.



{% include galheader.html %}
{% include toc.html %}