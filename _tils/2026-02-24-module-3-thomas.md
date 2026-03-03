---
title: "Module 3: Uncertainty Quantification"
date: 2026-02-24
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

Machine learning models typically output single predictions, but knowing when to trust those predictions requires understanding and measuring uncertainty. 

1. Uncertainty quantification serves three purposes: selective classification (refusing to answer when uncertain), system integration (passing probability distributions to downstream components), and system improvement (diagnosing where models struggle).
2. Uncertainty comes from multiple sources (irreducible noise, lack of training data, being outside the training distribution) that require different measurement approaches and suggest different improvement strategies.
3. Prediction intervals for regression provide guaranteed coverage by outputting ranges instead of point estimates, with conformalized quantile regression combining practical varying-width intervals with theoretical guarantees.
4. Ensemble methods measure epistemic uncertainty through model disagreement but fail on outliers, where all models confidently agree despite being far from training data.
5. Outlier detection addresses this failure mode, with foundation models (trained on diverse data) helping deep learning systems detect novelty by recognizing unusual combinations of familiar components.

This lecture was taught by Thomas Dietterich.

---

## Detailed Notes

### The Goal of Uncertainty Quantification

Uncertainty quantification serves three distinct purposes, each addressing different system needs: selective classification, system integration and system improvement. 

**Selective Classification**
The first goal is *defensive*. If a model isn't confident about an answer, it should refuse to answer rather than guess. The payoff is higher trust when the model does commit to a prediction. It's essentially about the model knowing when to say "I don't know". 

<div class="example-box" markdown="1">
**The Onewheel Problem** <br>
Self-driving cars need to identify pedestrians, cyclists, and other road users. But the onewheel (a skateboard-like device with one large central wheel) didn't exist when ImageNet was created in 2009. When a vision system encounters one, it should recognize this uncertainty and behave conservatively, slowing down and giving extra space, because people on onewheels move differently than pedestrians or cyclists.
</div>

<div class="example-box" markdown="1">
**Foggy Driving Conditions** <br>
Less visual information means higher uncertainty, which should trigger more conservative behavior like slowing down or integrating information across more video frames.
</div>

**System Integration**
The second goal is *collaborative*. When your ML model feeds into a larger system, forcing it to make hard binary decisions throws away valuable information. Outputting probability distributions lets downstream components make better decisions.

**System Improvement**
The third goal is *introspective*. High uncertainty tells you where your model struggles, which guides improvement efforts. Uncertainty patterns reveal whether you should collect more training data, reduce sensor noise, improve label quality, or add new features. Each source of uncertainty suggests different improvement strategies.


<div class="example-box" markdown="1">
**Missing Features in Iris Classification** <br>
The iris flower dataset has four features: sepal length, sepal width, petal length, and petal width. With all four features, you can separate iris setosa from iris versicolor cleanly. But if you only have sepal measurements, the classes overlap completely. The model will show high uncertainty, revealing that you need additional features to solve the problem.
</div>


### Forms of Uncertainty

Understanding where uncertainty comes from helps you know what actions might reduce it. Uncertainty splits into two main types.

**Aleatoric uncertainty** comes from irreducible randomness, such as measurement noise in sensors, inherent randomness in the process, or noise in labels. No matter how much data you collect, this noise persists.

**Epistemic uncertainty** comes from lack of knowledge. You could reduce it by collecting more training data, adding new features, using better sensors, or improving label quality.

This distinction can be hard in practice, as what counts as which type can depend on what actions you consider available. If you can only collect more data with your existing setup, then missing features create aleatoric uncertainty, as - from your perspective - it's irreducible. If you can add features, that same problem becomes epistemic.

Most sources of uncertainty show up in the aleatoric parameters when you fit a model. Missing an important feature makes your model perform poorly, but from the model's perspective it just looks like noisy data. The model's noise estimates (like σ² in regression or softmax probabilities in classification) will be high, even though the real problem is epistemic.

**Measuring Epistemic Uncertainty Through Ensembles**
One practical way to measure epistemic uncertainty is training multiple models and checking if they disagree. If you train an ensemble using different random seeds, bootstrap replicates, or dropout, and all the models agree despite noisy data, the problem is fundamentally aleatoric. But if the models disagree with each other, that's epistemic uncertainty.

<div class="example-box" markdown="1">
**Dropout for Uncertainty Estimation** <br>
During a forward pass through a neural network, randomly zero out 10% of the weights. Do this 100 times with different random masks. If all 100 passes give similar predictions, epistemic uncertainty is low. If they give wildly different predictions, epistemic uncertainty is high.
</div>

This approach has a critical failure mode: *outliers*. Far from the training data, neural networks often predict constant values, and ensemble members all agree with each other. It looks like low epistemic uncertainty when actually you're in uncharted territory and should be maximally uncertain.

We also have a third type of uncertainty that standard epistemic uncertainty misses: how far is your query from the training data? Even with infinite training data and no noise, uncertainty should increase when you ask questions outside the training distribution. This is **local epistemic uncertainty**. It's still about lack of knowledge, but it's specifically about whether you have training data near this particular query.

**So, what to do?** 
Each uncertainty type suggests different improvement strategies. High aleatoric uncertainty means you might need better sensors or cleaner labels. High epistemic uncertainty means you need more training data. High local epistemic uncertainty means you need anomaly detection to catch queries outside the training distribution.

Low measured epistemic uncertainty doesn't mean you can't improve your system. It just means more data with the same setup won't help. You might still need better features, less noise, or better labels.

For deployment, combine multiple uncertainty signals: aleatoric uncertainty from model outputs, epistemic uncertainty from ensemble disagreement, and local epistemic uncertainty from anomaly detection. Each catches different failure modes.


### Prediction Intervals for Regression

Instead of outputting a single prediction like "the answer is 6," you want your model to say "the answer is probably between 5 and 7." The width of that interval tells you how confident the model is. Narrow intervals mean high confidence, wide intervals mean uncertainty.

The goal is making these intervals honest (the true answer actually falls inside most of the time), informative (as narrow as possible while still being honest), and conditional (wider when the model should be uncertain, narrower when it should be confident).

**Linear Regression**
Linear regression is the only case where you can write down the exact formula and see what's happening. When you fit a line to data points, the prediction interval gets wider in three situations: the data itself is noisy, you don't have much training data, and you're making predictions far from where you have training data. These three sources multiply together rather than add.

<div class="example-box" markdown="1">
**Iris Flower Classification** <br>
With both sepal and petal measurements, you can separate iris species cleanly with narrow prediction intervals. When you lose petal length as a feature, everything overlaps and your intervals get much wider. The model's perspective: it just looks like noisier data.
</div>

**Bayesian Approaches**
The Bayesian approach treats uncertainty as "averaging over all possible models." Instead of fitting one line, imagine you have a probability distribution over all possible lines that could fit your data. Your prediction interval comes from asking what range of answers all these plausible lines give you.

This is mostly theoretical - it's computationally expensive for real problems. But it helps you understand what ensembles are approximating and motivates other methods.

**Gaussian Processes**
Gaussian Processes don't assume your relationship is linear but still give you exact mathematical answers. The model is more uncertain between training points and less uncertain near them. At the edges of your data, uncertainty explodes.

These are used for Bayesian optimization, like generating test scenarios for self-driving cars in regions where the car's behavior is most uncertain. They don't scale to huge datasets, but they're powerful for small-to-medium problems where you need principled uncertainty estimates.

**Quantile Regression**
Quantile regression solves a real problem: what if noise isn't constant everywhere? Instead of predicting the average, you predict the 10th percentile and 90th percentile directly. The gap between them is your interval.

<div class="example-box" markdown="1">
**House Price Prediction** <br>
In cheap neighborhoods, prices cluster tightly (narrow interval). In expensive neighborhoods, prices vary wildly (wide interval). Quantile regression captures this naturally by fitting separate models for different percentiles.
</div>

The problem: no guarantees. You fit two separate models and hope they behave sensibly. They might even cross each other (90th percentile below 10th percentile), which is nonsense.

**Conformal Prediction**
Conformal prediction adds guarantees to any model. Train your model (any model - neural net, random forest, whatever). On a separate calibration set, measure how wrong you were. Sort those errors. Make your interval wide enough to cover 90% of those errors if you want 90% coverage.

This works with any model, gives finite sample guarantees (not asymptotic), makes no assumptions about your data distribution, and is simple to implement. The catch: it gives you the same width interval everywhere, even when your model is sometimes confident and sometimes uncertain.

**Conformalized Quantile Regression**
This combines quantile regression (which lets interval width vary) with conformal prediction (which adds guarantees). Fit your two quantile lines (10th and 90th percentile). On a calibration set, measure how much you still got wrong. Add that safety margin to both lines. Now you have conditional intervals with guarantees.

This is the recommended practical approach - it captures varying uncertainty across the input space while providing theoretical guarantees about coverage.

All these methods all assume your test data looks like your training data. They don't handle distribution shift (the world changed) or outliers (you're asking about something totally new, like the onewheel). That's why you still need anomaly detection on top of prediction intervals.


### Prediction Intervals for Classification

For classification, you can output a set of possible classes with a guarantee that the true class is in that set with probability 1-α. If you have well-calibrated probabilities, sort classes by their predicted probabilities and keep adding until you hit your target coverage. There's also a conformal prediction approach that doesn't require calibration.

But prediction sets aren't particularly useful in practice. The main exception is differential diagnosis in medicine, where you want a shortlist of possible diseases to investigate further based on initial symptoms. For most applications, just use the calibrated probability of the most likely class. If your model says "class 7 with 95% probability" and that probability incorporates both epistemic and aleatoric uncertainty, that's a good enough confidence indicator. You don't need the full prediction set machinery.

### Local Epistemic Uncertainty via Outlier Detection

Ensemble methods for measuring epistemic uncertainty have a critical failure mode: outliers. Far from training data, neural networks often predict constant values, so all ensemble members agree with each other. It looks like low epistemic uncertainty when actually you're in uncharted territory and should be maximally uncertain.

A self-driving car may have never seen a onewheel, but the ensemble might confidently agree it's a pedestrian or cyclist because all the models are extrapolating in the same wrong way.

**Tabular models**
With handcrafted features, distance metrics work well. Normalize your features to zero mean and unit variance, maybe apply PCA, then measure distance to the nearest training point. If the query is far from all training data, flag it as an outlier.

Simple nearest neighbor distance works surprisingly well. The problem is you have to keep your training data around and answer nearest neighbor queries at runtime. When training on billions of examples, this isn't practical. More efficient alternatives like isolation forests (tree-based methods where you only store trees, not full training data) help, but the fundamental approach is sound.

**Deep Learning models**
Deep learning is "lazy" in the way that it only learns features it needs for the training task. Those learned features might be great on the training distribution but completely miss novelty. The learned features weren't designed to detect this kind of novelty, so distance metrics in the learned space fail.

The solution is to train on as much variation as possible, even if you don't think it's directly relevant. Maybe you've never seen a onewheel, but you've trained on skateboards and large wheels from motorcycles or lawnmowers. When you see a onewheel, those different feature detectors activate together in an unusual combination, giving you an anomaly signal.

<div class="example-box" markdown="1">
**CIFAR-100 Variation**<br>
In an experiment with CIFAR-10, they used 10 classes as "known," 10 as "novel" to test on, and 80 as auxiliary foundation model data:
- Training only on 10 known classes: 76% AUC for detecting novel classes
- Training on known + 80 auxiliary classes: 88-89% AUC
That's a huge improvement from just exposing the model to more variation, even though the auxiliary classes weren't the novel test classes.
</div>

For deep learning, you can't just use distance in the learned space because the space wasn't designed to detect novelty. The foundation model approach (train on everything available) is the best current strategy. It doesn't guarantee success, but it's much better than training only on your target classes.


### Applications

Uncertainty quantification has several applications (active learning for selecting training examples, selective prediction for deciding when to abstain), but one of the most interesting applications is to reduce hallucinations in large language models.

One approach focusses on removing incorrect claims from LLM responses, as the issue is that LLMs with high confidence. From a long, detailed response, we want to be able to remove individual incorrect laims, while keeping the correct ones. 

The approach applies conformal prediction to individual claims. Generate a response, extract individual claims, score each claim for validity, use conformal prediction to decide which to keep, then recombine the remaining claims.

<div class="example-box" markdown="1">
**Abraham Lincoln Example** <br>
LLM output: "Abraham Lincoln, born in Idaho, was the 16th president of the United States. He is best known for leading the country through the Civil War."

Extract claims and score by frequency across 5 generations:
- "Born in Idaho": 0.2 (hallucination, rarely generated)
- "Best known for Civil War": 0.4
- "16th president": 0.8 (consistently generated)

Conformal prediction sets a threshold to guarantee 90% of remaining claims are correct. Low threshold keeps everything including the hallucination. Medium threshold removes just the Idaho claim. High threshold removes Idaho and Civil War claims.
</div>

Testing this approach on Wikipedia biographies, Google queries, and math problems showed mixed results. For fact-based questions, reaching 90% correctness required deleting almost 70% of claims. For natural questions and math, about 25% deletion was needed.

Current methods have major gaps. They only capture aleatoric uncertainty (variation across multiple generations) because training data isn't accessible for most LLMs. You can't measure epistemic uncertainty without knowing what the model was trained on. The methods are also expensive, requiring multiple LLM calls for each query.

The scoring function (frequency across generations) is simple but leaves room for improvement. Recent work on combining multiple scoring methods shows promise. This is an active research area where better methods are needed, especially ones that work without access to training data.
