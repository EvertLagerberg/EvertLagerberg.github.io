---
layout: post
title: Ambient Visualization of Energy(d3.js)
imgfolder: /images
images:
  - name: energyviz1.png
    thumb: energyviz1.png
    text: data files
  - name: energyviz2.png
    thumb: energyviz2.png
    text: Low fi prototype. From the Boston Flickr project I kept the circular shape and the use of colors and color intensity. The inner ring represents cold water the middle ring hot water and the outer ring electricity. The intensity of the color shows high and low periods of consumption. The color codes where later changed.
  - name: energyviz3.png
    thumb: energyviz3.png
    text: It feels natural that cold water should be represented with blue and hot water with red.
  - name: energyviz4.png
    thumb: energyviz4.png
    text: Electricity cold be yellow or ice blue but yellow looked ugly and blue was already taken. I chose purple.
  - name: energyviz5.png
    thumb: energyviz5.png
    text: I liked their metaphor for a dashboard for a driving car given by the client
  - name: energyviz6.png
    thumb: energyviz6.png
    text: Blue represents cold water. Red represents hot water. Purple represents Electricity.  
  - name: energyviz7.png
    thumb: energyviz7.png
    text: Historical view.    
---
This was a project for a course at KTH, Stockholm in information visualization. I used [d3.js](http://d3js.org) to build this project. The code is available in this [repo](https://github.com/EvertLagerberg/Ambient-Visualization-of-Energy). Here is live [demo](http://evertlagerberg.github.io/Ambient-Visualization-of-Energy/) of my project. 

The following instructions was giving for the project:


<div style = 'font-family: "Courier New", Courier, monospace'>
<p>Instructions</p>

<p>Fortum has hired you to visualize energy consumption in the home. They have specified the visualization to be:</p>

1. Ambient
2. Peripheral
3. Calm
4. Beautiful
5. Minimalist
6. Excellent Overview
7. Artistic
8. A really cool dashboard for "understanding" home life (like the dashboard for driving the car)
9. The best visualization of home energy rhythms in the world

<p>Also, they want the visualization to run on a browser. Furthermore, they have asked you to make the visualization look like Fernanda Viegas and Martin Wattenberg's One Year of Boston Flickr image. A circle representing time with rich and deep layers of color. The visualization will go on a tablet embedded in the wall of the new apartments of the Royal Seaport in Stockholm. Home dwellers should feel proud of their "work of art" on the wall. They should show it to their guests when they come home. They should be able to see patterns of activity and energy usage in the home.</p>

<p>To accomplish this task, they have given you some cryptic data, claiming it is the recording of electricity, hot and cold water use in two real homes.</p>

<p>https://www.dropbox.com/sh/c0ehfu4bnfln7x4/AACI2c834-D8kuSCHpsvoQA-a?dl=0</p>

<p>You may have to figure out what the data is and how it is represented.</p>

<p>You remember that in your visualization course you talked about casual visualizations by some authors, their names may have been Zach Pousman, John Stasko, and Michael Mateas. Perhaps some of the material there may inspire you. You think you may have to read a bit more about ambient visualizations and calm technology.</p>

<p>Your task is to satisfy the requirements of your client by hosting a webpage with the clock. It should have a subtle link to a 300-word explanation of your data mappings, visual structures, and other choices.</p>

<p>Are you up to the challenge? Do you have any questions?</p>
</div>


###Interpretation of data
The first off this assignment was making sense of a cryptic and incomplete data set. My interpretation was as follows:


{% include gal.html image="energyviz1.png" %}

From going through the files I concluded that:

- 30-1001 = Home 1, 30-1002 = Home 2
- El/KV/VV = A running measurement tool that at each hour marks the consumed energy(electricity, cold water and hot water) and then resets to zero.
- El-dygn = Sum of electricity consumed during 24 hours.
- For each date in data we have two levels of detail(per day and per hour). We don’t always have both of them, sometimes we have neither.

###Visual Mappings
The structure and characteristics (and lack of) data does not allow us to use exactly the visual mappings of the Boston Flickr project. However we can interpret the adjectives they have given us and create something that at first glance looks similar to that project but really is quite different. Sometimes in interaction design, you have to be creative in how to persuade clients :)
{% include gal.html image="energyviz2.png" %}
*Low-fi-prototype: From the Boston Flickr project I kept the circular shape and the use of colors and color intensity. The inner ring represents cold water, the middle ring hot water and the outer ring electricity. The intensity of the color shows high and low periods of consumption. Important!: The color codes where later changed!*

{% include gal.html image="energyviz3.png" %}


{% include gal.html image="energyviz4.png" %}


###Adding live data
{% include gal.html image="energyviz5.png" %}


I am aware of the problems of [rebound effects](http://enviroinfo.eu/sites/default/files/pdfs/vol7574/0013.pdf) in persuasive ICT which aims to lower consumption. As argued by [Owen (2011)](https://www.youtube.com/watch?v=2S1mPOWRsSc) the only effective method of reducing the energy consumption of any system is by the users continuous intent to use less energy. So I wanted to create a visualisation that would inspire the user to always try to lower their energy consumption in comparison with the relevant season of last year.

###High-fi-prototype:
{% include gal.html image="energyviz6.png" %} 


Imagine that we are in looking at the visualisation on Sunday 7 march 2015. The visualisation shows a typical Sunday in March from last year. What this really means is that each hour has been calculated as an average of that hour for every Sunday in march 2014.

On the right side, we see the streaming live data of energy consumption in the home today. (I made a animation that fast forwards so that one hour is about 1 second but the idea is that the visualisation is updated each hour) Live data has been simulated to produce a randomised by believable output on the same color scale as the 2014 data. The user can compare her consumption today with relevant data from last year – in real time! 

By choosing "History" in the menu, she can also access a full visualisation of last year, per month, week or day to analyse her patterns and be more aware of her ecological footprint.

{% include gal.html image="energyviz7.png" %}

{% include galheader.html %}
 
