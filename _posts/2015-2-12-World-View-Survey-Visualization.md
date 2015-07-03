---
layout: post
title: World View Survey Visualization
---

####Finding interesting Data
I searched for interesting data in the open database of the World View Survey. They do a worldwide survey with the purpose of studying changing values and their impact on social and political life. The WVS is the largest non-commercial, cross-national, time series investigation of human beliefs and values ever executed, currently including interviews with almost 400,000 respondents. The survey is done almost 100 countries which contain almost 90 percent of the world’s population.

Their survey, which started in 1981 is conducted in rounds, or “waves” as the WVS calls it. Until now they have conducted 6 waves each spanning over 4-5 years. Since the survey questions to a large extent are unaltered between waves it is possible to research how social values have shifted over the last decades. I thought this aspect of time was the most fascinating scale of the WVS data. Through their Online Visualisation Tool it is possible to learn how answers to a certain question have been answered in each wave. I started exploring how different answers to different questions had changed over time. I was mostly interested in South American countries and political values since this is something that interests me and I have some knowledge about. Many nations in the area have a history of military regime and dictatorships across the 1900s. The ensuing democratic systems instituted in these nations since the 1980s have generally been week and struggling but continuous. In this context I found an intricate survey question in the data:


*“I'm going to describe various types of political systems and ask what you think about each as a way of governing this country. For each one, would you say it is a very good, fairly good, fairly bad or very bad way of governing this country?" 

Option 1: Having a strong leader who does not have to bother with parliament and elections”*

This question is interesting to me, because it reflects on both the fact that these nations have a history of violent military regimes but also on frustration towards the current democratic system. I spent last summer in Argentina; a country in deep economical crisis so I thought it might be interesting to see how answers to this question had changed over time. Here is the data from the World View Survey:
![My helpful screenshot]({{ site.url }}/images/WWS1.png)
This data show that the answer Very Good and Fairly Good both increased about 10% since 1994. This I found quite remarkable. And I was interested  in visualising it.

###VisualiZation prototyping
![My helpful screenshot]({{ site.url }}/images/WWS2.png)
This visualisation of the data can be created through WVS own online visualisation tool. It shows the increase in answers “Very Good” and “Fairly Good” but separately. I find that the increase in these answers makes more impact and tell more of a story if visualised together. 

![My helpful screenshot]({{ site.url }}/images/WWS3.png)
I used the WVS online tool to make a pie chart of the answers to the question in each wave and placed them next to each other chronologically. This visualisation method has been called “small multiples”; the same graphical design structure repeated but with one variable changed. My variable is of course time. In comparison with the chart I think this visualisation does more to tell the story since you can see the shift at a glance.

Next I used D3 to improve my “small multiples”-visualisation idea. I will explain this through the model of the Visualisation Pipeline.



###Visualisation Pipeline
####Raw Data
Raw data was collected from the WVS database. The data was quite clean. However I choose to sum answers of “don’t know/no answer” because the difference between them was not of significance to the story I wanted to tell.

####Data Tables
I created a data table for Argentinas answers to the question over the wave 3 to wave 6 (Argentina was not part of the two first waves). Each row in the table represents answer “Very Good”, Fairly Good, Bad, Very Bad and No Answer/Don’t know in that order.

```
wave3 wave4 wave5 wave6
5.9       7.9       5.3       14.5
20.8    28.1    26.8    30.1
29.6    25.6    26.4    22.8                                       
34.8    24.9    20.1    21.4
9.0       13.5    21.4    11.1
```

####Visual Structures
I then mapped each results to a pie chart in D3. But instead of putting each pie chart next to each other to do a small multiples visualisation, I mapped each waves data as a state of and animated pie chart. This was an experiment to investigate if the small multiples visualisation could be improved by animation. I did this in D3.js and created a view.

####Views
I created a view in D3 where the state of the pie chart could be altered by click of radiobutton representing the different waves. On choosing a wave, the pie chart will do an animated transform to that state. So if you go through the waves chronologically you can actually see the increase in positive answer over time. I then played around with a colour scale that made good representation of the answers. I also added a legend. 
![My helpful screenshot]({{ site.url }}/images/WWS4.png)

###Expanding my research questions
I was happy with my visualisation of Argentina's posture on strong leaders over time. However I was interesting to see how this corresponded with other South American nations. So I made equivalent visualisations for all South American nations available from the WVS data base and place them in the same view as the Argentina visualisation. Unfortunately not all the wave 3 through wave 6 was available for all countries, why I had some states completely grey (meaning "no data) I thought it would be cool if the user could play time forward for all nations simultaneously so I added a overall control that rules over all other controls. The user can still interact with individual pie charts. I also added a small box which appears when hovering over each nations pie chart. This works as details-on-demand about the political history of each nation.
![My helpful screenshot]({{ site.url }}/images/WWS5.png)

###Ending thoughts
I was happy to try out D3.js for the first time! I learned a lot! It seems like a very powerful visualisation tool and I look forward to taking on more difficult tasks with its help. During this process, I wanted to make an animated visualisation. Unlike more static visualisation which can easily be sketched out on paper or prototyped in a graphic design software, I had to try to visualise it in my head and I was not really sure how it would turn out until much later when I had coded it. I wonder if there are any good methods for prototyping animation? In any case very interesting process!



*This project was made as part of a course in Information Visualisation at KTH.*