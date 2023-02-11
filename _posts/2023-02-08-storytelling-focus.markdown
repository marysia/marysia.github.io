---
layout: post
title: "Data Storytelling part 3: Focus"
date:   2023-02-08 16:25:56 +0100
author: Marysia Winkels
permalink: /blog/data-storytelling-focus/
categories: AI-projects
abstract: "some abstract"


---

To convey a story with your data, it is up to you to draw the attention of the reader to the right place so they arrive at a similar conclusion. It's all about drawing their <emph>focus</emph> to the right place. 

Some relatively simple tricks can easily help with this: 

* Use *color* to highlight the important part of your visualization
* Add *text* to your design to immediately provide context
* Use the conclusion you want readers to draw as a *title*

Let's walk through an example. The image below visualizes the  the goals scored by each country in the group stage of the 2020 European Championships. 



<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/storytelling/goals1.png" alt="Image" height="400"/>
</div>

All the data is there, but it can be a little unclear at first glance what my intention is for showing you this image. What do I want to talk about here? 


<div class="Figure">
	<br>
	<img src="{{site.baseurl}}/assets/storytelling/goals4.png" alt="Image" height="400"/>
	<br> <br>
</div>

Adding a splash of color immediately draws your attention to the highlighted bar. It's likely that, when I show you this image, I have something to say about the number of goals of the Netherlands in this European Championship. 

Of course, the color chosen matters. Red, for instance, has a negative connotation. Green, on the other hand, is a more gentle, positive color. However, as the other bars are blue, the color green would not stand out as much. Therefore, in this case the color orange was chosen -- it provides a nice contrast with the blue bars, and is the color associated with the Netherlands.


<div class="Figure">
	<br>
	<img src="{{site.baseurl}}/assets/storytelling/goals5.png" alt="Image" height="400"/>
	<br> <br>
</div>

We can even actively draw the attention away from the other bars, by giving them less noticeable colors. This is called shading, and would also work with dark grey vs. light grey. 

But we are not limited to color to draw the focus of the reader. Remember, the reason why we're emphasizing certain parts of our image is because we want the reader to arrive at a similar conclusion by looking at the image alone -- without our verbal story to go along with it. 

<div class="Figure">
	<br>
	<img src="{{site.baseurl}}/assets/storytelling/goals6.png" alt="Image" height="400"/>
	<br> <br>
</div>



So, in order to give our story a little more context, we can add elements to the chart such as a line along with some text. In this case, a line that indicates the maximum number of goals in the championship four years earlier. This emphasizes that quite a few countries scored more goals than the top number of goals in the championships the years before. 

<div class="Figure">
	<br>
	<img src="{{site.baseurl}}/assets/storytelling/goals6.png" alt="Image" height="400"/>
	<br> <br>
</div>

We can even further emphasize this point by adjusting the colors again. 


<div class="Figure">
	<br>
	<img src="{{site.baseurl}}/assets/storytelling/goals7.png" alt="Image" height="400"/>
	<br> <br>
</div>

 By changing the colors and adding some text, we have transformed our first graph -- which was simply a lot of information -- into a visualisation that immediately and easily leds you draw some conclusions, such as: the Netherlands was the top scorer in the group phase and there were more high-scoring teams than  in the previous championship. 

<div class="Figure">
	<br>
		<img src="{{site.baseurl}}/assets/storytelling/goals1.png" alt="Image" height="150"/>
	<img src="{{site.baseurl}}/assets/storytelling/goals7.png" alt="Image" height="150"/>
</div>


There are of course many more elements than just text, lines or color that you can add to a chart. But these are usually enough to get the point across. You also don't want to add to much, because then you might lose the audience. 

# Conclusion in the Title

Another element that really helps to draw attention: the title. Look at the chart below for example. It visualizes the result of a survey of students of class 3A who were asked whether they liked a subject or not. This graph displays the percentage of students who liked the subject. 


Because of the title, which contains the word "popular", you will most likely  be inclined to look at the top few subjects first. 

<div class="Figure">
	<br>
		<img src="{{site.baseurl}}/assets/storytelling/lang1.png" alt="Image" height="300"/>

</div>

However, when we slightly adjust the title to focus on the negative, your attention will most likely be guided towards the bottom few class subjects.

<div class="Figure">
	<br>
		<img src="{{site.baseurl}}/assets/storytelling/lang2.png" alt="Image" height="300"/>
</div>

So the title is another, very effective way of guiding the reader's attention to where you want it to go. And you can of course combine that with previous techniques, such as adding color.

<div class="Figure">
	<br>
		<img src="{{site.baseurl}}/assets/storytelling/lang3.png" alt="Image" height="200"/>
				<img src="{{site.baseurl}}/assets/storytelling/lang4.png" alt="Image" height="200"/>
	<br> <br>

	
</div>

And adding more context to the graph itself. 

<div class="Figure">
	<br>
		<img src="{{site.baseurl}}/assets/storytelling/lang5.png" alt="Image" height="200"/>
				<img src="{{site.baseurl}}/assets/storytelling/lang6.png" alt="Image" height="200"/>
	<br> <br>
	
</div>

Because of some simple design choices, your attention is guided to a completely different part of the data in these two graphs. The one on the left focussing on how languages are popular, while the on the right focusses on how STEM subjects are not very popular.

### Conclusion

This goes to show: in order to use a visualization as a basis for a conversation, or to support your point, you don't need to be a graphic design wizard. Some simple adjustments to your matplotlib plots are good enough to transform an exploratory visualization to an explanatory one. This will help focus the conversation with those who you are showing your visualization to to what *you* want to focus on. 




---- 



<emph>A note on color</emph>
When you pick colors, make sure they are easy to read, do not take all the attention away from the graphic, and match the theme of the graph. 

You can customise the colors to fit the branding or theme, for instance, orange for the Netherlands, but do keep in mind that the default colors in matplotlib and other visualization libraries are usually chosen as they are the best combination of colours for those with colorblindness. 


