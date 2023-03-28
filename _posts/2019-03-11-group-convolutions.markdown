---
layout: post
title: "Increasing data-efficiency in medical image analysis"
date:   2019-03-11 12:25:56 +0100
author: Marysia Winkels
permalink: /blog/data-efficiency/
categories: AI-projects
abstract: "Artificial intelligence has the opportunity to cause a huge disruption in the way healthcare operates, but gathering enough data for training remains a challenge. So what if, rather than focusing all our energy on this data collection process, we can adjust our deep learning algorithms to require less data instead?"
---
### Summary
Artificial intelligence has the opportunity to cause a huge disruption in the way healthcare operates, but gathering enough data for training remains a challenge. So what if, instead of focusing all our energy on this data collection process, we can adjust our deep learning algorithms to require less data?

To tackle this, we introduce _Group Equivariant Convolutional Networks_ -- networks with a convolutional layer that generalises the weight sharing property to other types of transformations. 

_Group-Equivariant Convolutional Neural Networks:_
<ul style="line-height: 180%">
  <li>Reduce the <i>data requirements</i> for medical image analysis by <emph>10x</emph></li>
  <li>Invariant not only to <i>translation</i>, but also <i>rotation</i> and <i>reflection</i> </li>
  <li>Proved effective for <i>pulmonary nodule detecion</i></li>
  <li>Far better than data augmentation</li>
</ul>

<hr>
<br>
Deep learning, and convolutional neural networks in particular, have rapidly become the methodology of choice for all (medical) image related tasks. However, these techniques typically require a substantial amount of labeled data to learn from, meaning a human radiologist needs to manually record their findings in a way the computer can understand.
<img src="{{site.baseurl}}/assets/gconv/radiologist.png" alt="Image" width="250" align="right" />
 Furthermore, if we want the algorithms to generalise well over different patient populations, scanner types, reconstruction techniques and so forth, we need **even more data!**

This presents us with the challenge of <emph>data efficiency</emph>: the ability of an algorithm to learn complex tasks without requiring large quantities of data. Instead of spending all our time and energy at gathering more data, we try to increase the efficiency of the algorithms to handle the data that we already have.


# The Intuition
To explore how we can improve the data efficiency of convolutional neural networks (CNN), we first need to understand why CNNs are such an appropriate choice for image tasks in the first place. The great thing about CNNs are that they are roughly invariant to <emph>translation</emph>. This means that if a model has learned to recognise a structure, such as a dog, it doesn't matter where in the image the dog appears, it will still recognise it as being a dog. 

<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/gconv/ender-translated-1.jpg" alt="Image" width="250"/>
    <img src="{{site.baseurl}}/assets/gconv/ender-translated-2.jpg" alt="Image" width="250"/>
	<br> <br>
	<div class="Figure index">Figure 1.</div><div class="Figure description"> Two images of the same dog, at a slightly different location (an example of translation). Still easy for the model to understand. </div>
</div>
 
This is great for images -- after all, it rarely matters where exactly in an image a structure occurs, as long as you can see that it's there. To get technical here for a moment, translations are a type of transformation you can apply to the image, but whether or not you apply it has no influence on the prediction of the model. However, the problem is that there are other types of transformations, such as <emph>rotation</emph> and <emph>reflection</emph> (mirroring) that sadly do currently influence the prediction of the model. 

<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/gconv/ender-rot-1.jpg" alt="Image" width="125"/>
    <img src="{{site.baseurl}}/assets/gconv/ender-rot-2.jpg" alt="Image" width="125"/>
    <img src="{{site.baseurl}}/assets/gconv/ender-rot-3.jpg" alt="Image" width="125"/>
    <img src="{{site.baseurl}}/assets/gconv/ender-rot-4.jpg" alt="Image" width="125"/>
	<br> <br>
	<div class="Figure index">Figure 2.</div><div class="Figure description"> Images of the same dog, but this time rotated by 90 degrees. Incomprehensible to the model. </div>
</div>

This is a problem, because this means that you can't just present your algorithm with one orientation of an object (such as a dog), and expect it to work with a similar object, but rotated or flipped. In practice however, especially in the medical domain, the things you want to recognise and detect can occur in every orientation, both on a small and large scale. A good example of this are pulmonary nodules, but this also goes for other types of lesions. In order for the model to be able to recognise that, you have to present the algorithm with all these orientations separately while training, which -- you guessed it -- means you need more training data. 

Our solution to this was to create a new type of convolutional neural network (CNN) called the <emph>group-equivariant convolutional neural network</emph> (G-CNN), which can handle the rotated and reflected versions of images. And not only regular images, but also 3D volumes -- the type that of images that we have when we have CTs or MRIs.

# Applying to Real World Problems

But enough about dogs. What about _real_ problems, like medical findings? As a case study, we use <emph>pulmonary nodule detection</emph>, which incidentally is the problem we're trying to solve at here over at Aidence.

 Pulmonary nodules are small lesions in the lung that may be indicative of lung cancer, the leading cause of cancer-related deaths worldwide. Radiologists will generally try to detect nodules in an early stage, so they can track the growth over time. However, looking for these nodules can feel like looking for a needle in a haystack - without the advantage that you can just burn down the haystack to find said needle. 

Pulmonary nodules (also often called <emph>lung nodules</emph>) are visible on a chest CT -- a 3D scan of the chest, visualising bones, muscles, fat, organs and blood vessels in grayscale. A typical chest CT is comprised of ~300 images (called _slices_), stacked together to form the whole scan. You can imagine that looking through ~300 black and white images to find a small abnormality can be a tedious task, especially considering that nodules can take many shapes and forms. 

That's where <emph>AI</emph> comes in to help! 
<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/gconv/nodule-1.png" alt="Image" width="250"/>
    <img src="{{site.baseurl}}/assets/gconv/nodule-2.png" alt="Image" width="250"/>
	<br> <br>
	<div class="Figure index">Figure 3.</div><div class="Figure description"> Example of a nodule detected and analysed on a single CT slice by <a href="http://www.aidence.com/our-solution">Veye Chest</a>, Aidence's pulmonary nodule detection software.  </div>
</div>

Normally, when approaching a data science problem, you simply utilise all the data you have available. Luckily, through the efforts of the <emph>NLST</emph> and <emph>LIDC/IDRI</emph>, we happen to have a lot of data available for this task in particular. However, although we'd like to improve our detection results, we are also conceptually very interested in how well our new type of network performs when presented with a lot less data to train on.

That's why we trained our model on four different dataset sizes: <emph>30</emph>, <emph>300</emph>, <emph>3.000</emph> and <emph>30.000</emph> scans to train on respectively. You can imagine that 30 scans, or maybe even 300, is waaay too little to get meaningful results on, but we'll do it anyway, just to see what happens! 


# Did it work?  
**The results were astonishing**. Of course, we hoped that our intuition was correct and we'd achieve an increase in performance, or a similar performance with a model trained on  less data. What our experiments showed, however, was that the models trained on <emph> 10x less data</emph>  with our new type of convolutional neural network achieved a similar - or better! -  performance than a standard CNN trained on 10x as much data. This might not seem like a lot, but imagine being a radiologist, and the difference in having to manually detect, segment or classify (and report!) 100 samples instead of 1.000. This improvement in efficiency corresponds to a major reduction in cost and effort of data collection. This in turn makes creating new models more accessible, and brings pulmonary nodule detection and hopefully other <emph>Computer-Aided Diagnosis</emph> (CAD) systems closer to reality. 

<div class="Figure">
	<br>
    <img src="{{site.baseurl}}/assets/gconv/barplot.png" alt="Image" width="400"/>
	<br> <br>
	<div class="Figure index">Figure 4.</div><div class="Figure description">The G-CNN (<font color="#2EC0D6">blue</font>) models are as good (or better than) the score of the regular CNN (<font color="grey">grey</font>) trained on ten times the amount of data, which means we've achieved a <b>10x</b> increase in data efficiency! </div>
</div>

<br><br>

## References

_This research was performed as part of my thesis for the MSc Artificial Intelligence at the University of Amsterdam. It was supervised by [Taco Cohen](http://www.tacocohen.wordpress.com) (Machine Learning researcher at Qualcomm) &  prof. dr. [Max Welling](https://staff.fnwi.uva.nl/m.welling/) (research chair in Machine Learning at the University of Amsterdam and a VP Technologies at Qualcomm), as they originally laid the foundation of the work on equivariance and group-convolutional neural networks._

_Implementations for 2D and 3D group-convolutions in Tensorflow, PyTorch and Chainer can be easily used from the [GrouPy](https://github.com/tscohen/GrouPy) python package. A Keras implementation for 2D can be found [here](https://github.com/basveeling/keras-gcnn)._

<font size="2">
M. Winkels, T.S. Cohen, <b><i>3D G-CNNs for Pulmonary Nodule Detection</i></b>, International Conference on Medical Imaging with Deep Learning (MIDL), 2018. [<a href="https://arxiv.org/abs/1804.04656">arxiv</a>] [<a href="https://arxiv.org/pdf/1804.04656.pdf">pdf</a>]
<br>
M. Winkels, T.S. Cohen, <b><i>Pulmonary Nodule Detection in CT Scans with Equivariant CNNs</i></b>, Under review at Medical Image Analysis (MIA), 2018. [<a href="{{site.baseurl}}/assets/MIA.pdf">pdf</a>]
<br>
M. Winkels, T.S. Cohen, <b><i>3D Group-Equivariant Neural Networks for Octahedral and Square Prism Symmetry Groups</i></b>, FAIM/ICML Workshop on Towards learning with limited labels: Equivariance, Invariance, and Beyond (ICML), 2018. [<a href="{{site.baseurl}}/assets/ICML.pdf">pdf</a>]
<br>
T.S. Cohen, M. Welling, <b><i>Group Equivariant Convolutional Networks</i></b>, Proceedings of the International Conference on Machine Learning (ICML), 2016. [<a href="https://arxiv.org/abs/1602.07576">arxiv</a>] [<a href="https://arxiv.org/pdf/1602.07576.pdf">pdf</a>]
</font>
