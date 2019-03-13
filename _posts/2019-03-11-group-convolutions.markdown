---
layout: post
title: "Group-Convolutions: a new take on convolutional neural networks"
subtitle: "overcoming the data challenge in medical image analysis"
date:   2019-03-11 12:25:56 +0100
author: Marysia Winkels
permalink: /blog/group-convolutions
categories: AI-projects
abstract: "Artificial intelligence has the opportunity to cause a huge disruption in the way healthcare operates, but the challenge is gathering enough data. So what if, instead of focussing all our energy on this data collection process, we can adjust our deep learning algorithms to require less data? That's what we do with 3D G-CNNs!"
---
<hr>
### TL;DR
_Artificial intelligence has the opportunity to cause a huge disruption in the way healthcare operates, but the challenge is gathering enough data. So what if, instead of focussing all our energy on this data collection process, we can adjust our deep learning algorithms to require less data? That's what we do with 3D G-CNNs!_

_Group-Equivariant Convolutional Neural Networks:_
<ul style="line-height: 180%">
  <li>Reduce the data requirements for medical image analysis 10x</li>
  <li>Proved effective for <i>pulmonary nodule detecion</i></li>
  <li>Invariant not only to <i>translation</i>, but also <i>rotation</i> and <i>reflection</i> </li>
  <li>Far better than data augmentation</li>
</ul>

<font size="2">
	<b>Note:</b> This post is intended to be an <i>intuitive</i> introduction to the work previously presented at MIDL 2018, ICML 2018 and an upcoming issue of the Medical Image Analysis journal. A more in-depth, technical explanation will appear later.
</font>
<hr>

Deep learning, and convolutional neural networks in particular, have rapidly become the methodology of choice for all (medical) image related tasks.  However, these techniques typically require a substantial amount of labeled data to learn from, meaning a human radiologist needs to manually record their findings in a way the computer can understand. Furthermore, if we want the algorithms to generalise well over different patient populations, scanner types, reconstruction techniques and so forth, we need even more data! 

This presents us with the challenge of _data efficiency_: the ability of an algorithm to learn complex tasks without requiring large quantities of data. Instead of spending all our time and energy at gathering more data, we try to increase the efficiency of the algorithms to handle the data that we already have.


# The Solution
To explore how we can improve the data efficiency of convolutional neural networks (CNN), we first need to understand why CNNs are such an appropriate choice for image tasks in the first place. The great thing about CNNs are that they are roughly invariant to translation. This means that if a model has learned to recognise a structure, such as a dog, it doesn't matter where in the image the dog appears, it will still recognise it as being a dog. 

<div class="Figure">
	<br>
    <img src="../assets/gconv/ender-translated-1.jpg" alt="Image" width="250"/>
    <img src="../assets/gconv/ender-translated-2.jpg" alt="Image" width="250"/>
	<br> <br>
	<div class="Figure index">Figure 1.</div><div class="Figure description"> Two images of the same dog, at a slightly different location (an example of translation). Still easy for the model to understand. </div>
</div>
 
This is great for images -- after all, it rarely matters where exactly in an image a structure occurs, as long as you can see that it's there. To get technical here for a moment, translations are a type of transformation you can apply to the image, but whether or not you apply it has no influence on the prediction of the model. However, the problem is that there are other types of transformations, such as reflection (mirroring) or rotation that sadly do currently influence the prediction of the model. 

<div class="Figure">
	<br>
    <img src="../assets/gconv/ender-rot-1.jpg" alt="Image" width="125"/>
    <img src="../assets/gconv/ender-rot-2.jpg" alt="Image" width="125"/>
    <img src="../assets/gconv/ender-rot-3.jpg" alt="Image" width="125"/>
    <img src="../assets/gconv/ender-rot-4.jpg" alt="Image" width="125"/>
	<br> <br>
	<div class="Figure index">Figure 2.</div><div class="Figure description"> Images of the same dog, but this time rotated by 90 degrees. Incomprehensible to the model. </div>
</div>

This is a problem, because this means that you can't just present your algorithm with one orientation of an object (such as a dog), and expect it to work with objects that are similar, but rotated or flipped. In practice, however, especially in the medical domain, rotations and reflections of things you want to detect  -- such as pulmonary nodules -- occur both on a small and large scale. In order for the model to be able to recognise that, you have to present the algorithm with all these orientations separately while training, which -- as you can guess -- means you need more training data. 

Our solution to this was to create a new type of convolutional neural network (CNN) called the _group-equivariant convolutional neural network_ (G-CNN), which can handle the rotated and reflected versions of images. And not only regular images, but also 3D volumes -- the type that of images that we have when we have CTs or MRIs.

# Applying to the Real World
Yes, recognizing dogs in pictures is all fun and games, but how well does this work for _real_ problems, like a medical finding? As a case study, we use _pulmonary nodule_ detection. Pulmonary nodules are small lesions in the lung that may be indicative of lung cancer, which is why radiologists will generally try to detect these so they can track the growth over time. However, looking for these nodules can feel like looking for a needle in a haystack - without the advantage that you can just burn down the haystack to find said needle. 

Lung nodules are visible on a chest CT -- a 3D scan of the chest, visualising bones, muscles, fat, organs and blood vessels in grayscale. A typical chest CT is comprised of ~300 images (_slices_), stacked together to form the whole scan. You can imagine that looking through ~300 black and white images to find a small abnormality can be a tedious task, especially considering that nodules can take many shapes and forms. 

That's where AI comes in to help! 

<div class="Figure">
	<br>
    <img src="../assets/gconv/nodule-1.png" alt="Image" width="250"/>
    <img src="../assets/gconv/nodule-2.png" alt="Image" width="250"/>
	<br> <br>
	<div class="Figure index">Figure 3.</div><div class="Figure description"> Example of a nodule detected and analysed on an single CT slice by <a href="http://www.aidence.com/our-solution">Veye Chest</a>, Aidence's pulmonary nodule detection software.  </div>
</div>

Normally, when approaching a data science problem, you utilise all the data you have available. Although we'd of course like to improve the detection of pulmonary nodules specifically, we are also conceptually very interested in how well our new type of network performs when presented with a lot less data to train on.

That's why we trained our model on four different dataset sizes: 30, 300, 3.000 and 30.000 scans to train on respectively. You can imagine that 30 scans, or maybe even 300, is waaay too little to get meaningful results on, but we'll do it anyway, just to see what happens! 


# Did it work?  
The results were astonishing. Of course, we hoped that our intuition was correct and we'd achieve an increase in performance, or a similar performance with a model trained on  less data. What our experiments showed, however, was that the models trained on 10x less data with our new type of convolutional neural network achieved a similar (or better!) performance than a standard CNN trained on 10x as much data. This might not seem like a lot, but imagine being a radiologist, and the difference in having to manually detect, segment or classify (and report!) 100 samples instead of 1.000. This improvement in efficiency corresponds to a major reduction in cost and effort of data collection. This in turn makes creating new models more accessible, and brings pulmonary nodule detection and hopefully other CAD systems closer to reality. 

<div class="Figure">
	<br>
    <img src="../assets/gconv/barplot.png" alt="Image" width="400"/>
	<br> <br>
	<div class="Figure index">Figure 4.</div><div class="Figure description">The G-CNN (<font color="#2EC0D6">blue</font>) models are as good (or better than) the score of the regular CNN (<font color="grey">grey</font>) trained on ten times the amount of data, which means we've achieved a <b>10x</b> increase in data efficiency! </div>
</div>

<br><br>

## References

_This research was performed as part of my thesis for the MSc Artificial Intelligence at the University of Amsterdam. It was supervised by [Taco Cohen](http://www.tacocohen.wordpress.com) (Machine Learning researcher at Qualcomm and  recently named as one of the [35 under 35 by MIT](https://www.innovatorsunder35.com/the-list/taco-cohen/)) &  prof. dr. [Max Welling](https://staff.fnwi.uva.nl/m.welling/) (research chair in Machine Learning at the University of Amsterdam and a VP Technologies at Qualcomm), as they originally laid the foundation of the work on equivariance and group-convolutional neural networks._

_Implementations for 2D and 3D group-convolutions in Tensorflow, PyTorch and Chainer can be easily used from the [GrouPy](https://github.com/tscohen/GrouPy) python package. A Keras implementation for 2D can be found [here](https://github.com/basveeling/keras-gcnn)._

<font size="2">
M. Winkels, T.S. Cohen, <b><i>3D G-CNNs for Pulmonary Nodule Detection</i></b>, International Conference on Medical Imaging with Deep Learning (MIDL), 2018.
<br>
M. Winkels, T.S. Cohen, <b><i>Pulmonary Nodule Detection in CT Scans with Equivariant CNNs</i></b>, Under review at Medical Image Analysis (MIA), 2018.
<br>
M. Winkels, T.S. Cohen, <b><i>3D Group-Equivariant Neural Networks for Octahedral and Square Prism Symmetry Groups</i></b>, FAIM/ICML Workshop on Towards learning with limited labels: Equivariance, Invariance, and Beyond (ICML), 2018.
<br>
T.S. Cohen, M. Welling, <b><i>Group Equivariant Convolutional Networks</i></b>, Proceedings of the International Conference on Machine Learning (ICML), 2016.
</font>
