---
layout: post
title: 3D Modeling project part 1 Blender
---

My teacher had told me that he and his team were interested in 3D models of objects and building on the KTH Campus. Since my project would be kind of small and simple I chose to model a trashbin :). This is the photo that i took! It was very snowy, so picture could definitely have been better! Especially if I wanted to make textures tiles from the photo. However, later I came to the conclusion that materials might actually look better for the kind of very smooth painted surface of the trashbin.

![My helpful screenshot]({{ site.url }}/images/trash.png)

Now I was ready to start modelling!
First i loaded in a front photo of the trash bin in Blender as background, moved it slightly to the side and then started to try to make the starting cube object of blender look like the trashbin form.
![My helpful screenshot]({{ site.url }}/images/trashbinmodel1.png)

Since the trash bin i very symmetrical. I had great help from the  Loop Cut tool in blender. The Cut Tool basically creates new subareas (edges and faces) on the object. But what is great about it, is that it makes these sub dividings symmetrically all around the object. So for example, when i sliced  up a hole for the opening on the trash bin, the same opening was there to but cut opened on the opposite side.
![My helpful screenshot]({{ site.url }}/images/trashbinmodel2.png)

I cut open four holes in the top of the object for the holes and dragged the side of the opening slightly inwards. I also rounded the roof of the trash bin.
![My helpful screenshot]({{ site.url }}/images/trashbinmodel3.png)

Then i worked with the two golden parts of the form. There are two parts of the trash bin where the shape is shaped in three lines of golden bands. I had to extend and intended the surface and the round all parts. 
![My helpful screenshot]({{ site.url }}/images/trashbinmodel4.png)

I also added a rectangle on each side of the trash bin. I don't know that function it has, but it is there on the real trash bin so I wanted to add it.
![My helpful screenshot]({{ site.url }}/images/trashbinmodel5.png)

Now I had my model ready and done!
![My helpful screenshot]({{ site.url }}/images/trashbinmodel6.png)

![My helpful screenshot]({{ site.url }}/images/trashbinmodel7.png)

Next up I wanted to add the green and gold textures of the trash bin. I found it quite hard to work with textures for this material since it does not really have much of a texture. It is smooth painted metal. This was easier to make with materials and lightning. I experimented with different materials and adding lightning and thought the result was quite good.

My modelling of the trash bin was finished!
![My helpful screenshot]({{ site.url }}/images/trashbinmodel8.png)

Finally I wanted to try to input my model into Unity. I have never used Unity before and it seems like quite a complex program. Fortunately, first time I used it, it loaded up a default game world and it was easy for med to import my model as .fbx directly into it. Looks good! However I had some problems loading in the materials I created in blender. (the materials shown here are similare materials that I added in Unity). Will have to investigate how to export from blender to unity in .fbx with the materials included.
![My helpful screenshot]({{ site.url }}/images/trashbinunity1.png)

###Problems with materials
After initially just dragging in my model into Unity and saw that it more or less worked I had to work on some details. First, I had problems with the the materials(the blue/green and gold metallic colours) I had created in Blender. After a lot of googling I came to the conclusion that this probably not worked for one of two reasons or both:

1. I used the Cycles Engine in Blender to construct my materials. I do not know if this works outside of Blender

2. My materials used combinations of Glossy shaders. As I understand it, shaders works differently in distinct environments why you can not just translate it from Blender to Unity.

I considered using Crazybumps to create textures of the materials I had created in Blender and loading them in that way to Unity but this would include a lot of extra work. In the end I chose an easy way out. I loaded in screenshots of the rendered trashbin into Unity and then took samples of the colors to create new materials in Unity. I then worked with Unitys internal shaders to try to create the same metallic surface. The result was OK, however next time I will definitely try to plan my work flow differently!
![My helpful screenshot]({{ site.url }}/images/trashbinmodel9.png)

###Creating a scene
Investigating my materials problems, I learned a lot about the Unity architecture. So I created a new scene. Added a floor with a pavement texture, some lightning, my trash bin model and a First-Person-Controller. From there I could press play and walk around and look at my model. Nice!

###Editing model
However I noticed another problem! I could trough to the other side of the trash bin! But circling around to the other side of the model, it was solid. I had not created an interior of the model! I went back to blender, created some new faces and extruded to created and interior, a bottom and a roof of the trash bin. But I still had some problems! After a lot of googling I understood that faces actually has a back and a front side. And only the front side is covered by the material, from the back of the face you can see through it. So I had to display normals in Blender so I could see which faces where turned and then flip normals to turn them around. The picture below show the difference between the model before and after I made this changes, in the scene of Unity.
![My helpful screenshot]({{ site.url }}/images/trashbinunity2.png)

In the end this problems I had with loading in my model to Unity really forced me to read up on how the Unity architecture works and I now feel that I have a basic understanding of how to move between between Blender and Unity. On my next project, because of this, my workflow will probably be quite more straightforward. However I am definitely interested in exploring both Blender and Unity more!

Here is a link to walk around my my scene and look at the trashbin:
[http://www.csc.kth.se/~evertla/Intro/project/KTH_trashbin.html](http://www.csc.kth.se/~evertla/Intro/project/KTH_trashbin.html)

