---
layout: post
title: "Data Storytelling part 1: Introduction"
date:   2023-01-23 16:25:56 +0100
author: Marysia Winkels
permalink: /blog/data-storytelling-introduction/
categories: AI-projects
abstract: "<i>\"Beautiful data visualizations and fancy infographics are not for me. As a data scientist, I need nothing more than my trusty matplotlib toolkit!\"</i> ... do you recognise yourself in this statement? Let me convince you why you, too, should care about telling a story with your data."


---



As data scientists, it's crucial to understand the difference between data visualization intended to gain insights and visualizations to communicate those insights to others. *Exploratory* visualization is used to form hypotheses and find hidden gems in the data, while *explanatory* visualization is used to clearly communicate insights to an audience.

It's important to rea that the best visualization for exploration may not be the best for explanation.


So let's start at the basics. Why do we do bother visualizing data to begin with?


Some people are under the impression that charts are simply "pretty pictures", and that all of the important information can be derived through statistical analysis. However, visualizations are hugely beneficial to uncover information from your data that simply staring at the raw numbers won't reveal. 

Take the dataset visualized below for example. All images represent 13 distinct datasets with the exact same summary statistics, while drastically different in appearance when plotted. (courtesy of Same Stats, Different Graphs)


<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/storytelling/datasaurus_dozen.gif" alt="Image" width="350"/>
</div>


But most data scientists know that visualization is an incredibly useful tool to understand your data better.  So.. what does that have to do with data storytelling? 

First of all, there are different _types_ of visualization. We have data visualizations like the one below, which is a simple scatter plot of nine data points made with matplotlib. This was made, just like our dinosaur plot, to get a little more insight in the data at hand. 

<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/storytelling/simple_scatter.png" alt="Image" width="250"/>
</div>


But this is a very different chart from the chart we see in below. Whereas one is a simple plot, most likely intended for data analysis, the other is designed to incite an emotion.  The title, the color and the orientation of the bars, they all contribute to the message -- you don't even have to read the numbers to know what that message is. 

<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/storytelling/iraq_negative.png" alt="Image" width="250"/>
	<br> <br>
	<div class=" Figure index">Figure:</div><div class="Figure description">An infographic to mark the end of the US' military engagement in Iraq in 2011 by <a href="http://www.simonscarr.com/iraqs-bloody-toll">Simon Scarr</a>.</div>
</div>

And just as easily, the message can be flipped. The data visualized is indentical, but by reversing because the way the data was presented was different, it guides you to a wildly different conclusion.
 

<div class="Figure">
	<br>
	    <img src="{{site.baseurl}}/assets/storytelling/iraq_negative.png" alt="Image" width="150"/>
	<img src="{{site.baseurl}}/assets/storytelling/iraq_positive.png" alt="Image" width="150"/>
	<br> <br>
	<div class="Figure index">Figure:</div><div class="Figure description">Original infographic next to an alternative version of Iraq death toll infographic by <a href="https://www.infoworld.com/article/3088166/why-how-to-lie-with-statistics-did-us-a-disservice.html">Andy Cotgreave</a>. </div>
</div>
 

# Why does that matter to me? 
 <img src="{{site.baseurl}}/assets/storytelling/exploratory.png" alt="Image" width="100" align="right" />
 
 The first chart we saw, the simple matplotlib scatter plot, is an example of *exploratory* data visualization. This is what we do if we, as individuals, are looking for insights hidden in our data. We form hypotheses  -- for instance, is the customer satisfaction going up over time? --  and check these with our data through visualization. This is what you do when you're looking for whatever is noteworthy or interesting about your data. The hidden gems, so to say.  
 
 
 The second type of data visualization is *explanatory* data visualization.  This is the visualization you make when you want to communicate the insight you found with someone else, communicate to an audience-- most likely, not a data scientist. 
 
 
 <img src="{{site.baseurl}}/assets/storytelling/insight.png" alt="Image" width="150" align="right" />
 
 It doesn't immediately have to be a fancy infographic, like we saw before. But where exploratory data visualization is experimentation-driven and doesn't require a clear conclusion, explanatory data visualization *does* intentionally guide the reader to a certain conclusion.
 
 <!-- <div class="Figure">
 	<br>
     <img src="{{site.baseurl}}/assets/storytelling/insight.png" alt="Image" width="300"/>
 	<br> <br>
 </div> -->

And it turns out: 
 
>>>  The best <emph>exploratory</emph> visualization is not necessarily the best <emph>explanatory</emph> visualization

When you are exploring your data, you have the luxury of time. You can look at summary statistics, create different visualizations, spend time disecting the information on the graph presented to you, and make connections to insights you gathered before. 

When you are *communicating* your insights, you do not have that luxury of time. If you're presenting to a stakeholder, you cannot take them through your whole process which led you to your insight; it should be as clear as possible from the get-go. 


And more often then not, there is a point you want to make. An action you want them to take. Maybe your analysis revealed that the recent changes to the marketing campaign have made people look at your company more favourably. Therefore, you recommend continuing with this. Or maybe you have discovered these farm chickens grow to be the healthiest on a certain food, therefore you recommend this diet. 

The point is: explanatory visualizations aren't there to give some color to your slides. They are there to support a *point*. Because data is only valuable if given context. 

And while data itself may be objective, every decision you make on how to present it shapes a different story. 

And that's where _data storytelling_ comes in. The art of visualizing your data in such a way that it starts the conversation *you* want to start.


So how can you shape your own data story? Focus on the following aspects: 
* Foundation
* Focus
* Forward

See the next parts of this series for details. 


<!-- # Bonus Chart
<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/storytelling/population_growth1.png" alt="Image" width="300"/>
    <img src="{{site.baseurl}}/assets/storytelling/population_growth2.png" alt="Image" width="300"/>
	<br>
	<br>
	<div class="Figure index"></div><div class="Figure description"> Two line charts visualizing population growth.
Both  charts are based on the same dataset, but a tiny decision like the amount of years you display as reference have a huge impact on the story the chart tells. <br> <a href="https://public.tableau.com/app/profile/jmperafan/viz/WorldPopulationGrowth_2/WorldPopulationGrowth">Source.</a></div>
</div> -->



<!-- # Key Points of a Data Story



<b><emph>- Foundation</emph></b>



<b><emph>- Focus</emph></b>



<b><emph>- Forward</emph></b> -->




