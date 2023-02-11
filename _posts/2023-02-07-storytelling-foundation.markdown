---
layout: post
title: "Data Storytelling part 2: Foundation"
date:   2023-02-07 16:25:56 +0100
author: Marysia Winkels
permalink: /blog/data-storytelling-foundation/
categories: AI-projects
abstract: "The first step towards data storytelling is to choose the right foundation. How do you choose the right chart type, and display your data in the most correct and effective way?"


---

When you want to convey a story with your data, your first choice is how to display the data itself. This means choosing the right chart type, and making the right decisions regarding your chart.

This might seem obvious, but some charts are better to transfer information than others. Let's look the two graphs below. They show exactly the same data: the total number of participants and the number of trainings offered during the year. 

But what graph is better at showing the trend -- how the numbers change during the year? Is it the line plot over time, or the bar chart?  

<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/storytelling/trends.png" alt="Image" width="700"/>
</div>


In this case, thel ine chart would be better if you were interested in the trend. Which doesn't mean a bar chart is a bad choice -- this bar chart gives you a nice opportunity to compare the number of participants to the number of trainings on a monthly basis. But it isn't the right choice when you're interested in *trend*.

Let's look at another example.  Again, both display the exact same data source. But which visualization makes it easier to determine which investment has a greater share? 


<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/storytelling/share.png" alt="Image" width="700"/>
</div>

 In the first graph, the blue and the orange blocks seem roughtly the same size, and the green and red one do too. 

 However, in the second graph, although the bars for international stock and large cap stock are of relatively similar size, as well as the bonds and real estate bars, it's still easier too see -- it helps that the bars are ordered. And if the background of the graph had been a grid, that would have made things even easier. 
 

Certain chart types simply work better for certain types of data, and you do not have to reinvent the wheel yourself. [chartguide.com](chartguide.com) has an amazing poster, as seen below, that gives an overview of what charts work well for what data types. 

<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/storytelling/chart_guide.png" alt="Image" width="700"/>
</div>


But this is only the first step. Once you've figured out the right chart to use, there are still a couple of things you need to take into consideration:  
* Make sure the reader is <emph>familiar</emph> with your type of chart
* Choose the data to display <emph>carefully</emph>

###  Make sure the reader is <emph>familiar</emph> with your type of chart

A common mistake, and one I'm guilty of myself as well. Charts with error labels and confidence intervals are great when it comes to exploratory data analysis --  they help you, as a data scientist or data analyst, understand your data a lot better as they give a general idea of how precise a measurement is. 

But they are not your friends when it comes to explanatory data analysis. They won't help the reader understand the data.

 Take the boxplot below for example. This chart tells us a lot in terms of attack score for each Pokemon. We don't only know the average, we also get a feeling for the consistency within that group. 

<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/storytelling/pokemon.png" alt="Image" width="700"/>
</div>

But there's a lot going on here, and again, while I had the luxury of taking the time to analyse this chart, the person who I'm presenting does not have that privilege. And they need to to know the point I'm trying to make *immediately*. 

Boxplots aren't as common outside of exploratory data anaylysis as one might think. So before you show them the Pokémon boxplot image, you might have to show them this one...
<div class="Figure">
	<br>
    <img src="https://www.simplypsychology.org/boxplot.jpg" alt="Image" width="700"/>
</div>


And explain that inter quantile range is calculated by determining the upper and lower quantile, and the size of the inter quantile range is used to determine the lower whisker and upper whisker by subtracting 1,5x the inter quantile range from Q1 and Q3 respectively.

I doubt that this will make your (presumably non-technical) stakeholder very happy..

So. When you choose your chart, keep in mind how *familiar* the person you are presenting to will be with that type of image.

###   Choose the data to display **carefully**

So. You've chosen the right chart for your visualization. But now on to the next challenge: how to present your data. 

Although you might feel data is objective, the way you choose to present it heavily influences the story it tells. Let's look at the example below: it shows the population growth over time over the last sixty years. 

<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/storytelling/population_growth1.png" alt="Image" width="300"/>
	    <img src="{{site.baseurl}}/assets/storytelling/population_growth2.png" alt="Image" width="300"/>
</div>

When we look at the graph on the left, it shows quite a bit of growth -- it looks a bit worrying. But the one on the right, based on exactly the same data source, looks critical. 

Both line charts come from exactly the same dataset, but a tiny decision like the amount of years you display as reference have a huge impact on the story the chart tells.

But not only the ranges you choose influence the impact. 



<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/storytelling/limited.png" alt="Image" width="200"/>
	    <img src="{{site.baseurl}}/assets/storytelling/original.png" alt="Image" width="200"/>
	    <img src="{{site.baseurl}}/assets/storytelling/percentages.png" alt="Image" width="200"/>
		
</div>

The graphs above are all based on the same data source. In the first graph, we show the difference between sales for store A and store B. It seems like store A is doing a lot better! But notice that the limits on the graph don't start at zero. 

 If we change the limit to start at zero, the results look a lot closer. 

 And if we change the units to percentages, it makes senses to set the range from zero to 100 -- and the results for store A and store B look even more similar. 

 The choices you make in how you display your data, influence the conclusions people will draw from the graph. 


> By not choosing to display data -- even lack of data -- you can severly impact the story.


### Conclusion

An important decision in data storytelling is the chart you choose to visualize your data. Some charts are more suitable than others. But not only that -- it is up to you to make the right decisions, on what data to show and on how to display that information. 

 As data storytellers, unlike fiction storytellers, we have an obligation to stay close to the truth.

