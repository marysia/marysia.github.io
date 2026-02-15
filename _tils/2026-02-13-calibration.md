---
title: "Module 3: Calibration"
date: 2026-02-11
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

This lecture covered why we're interested in calibration, calibration techniques and evaluation metrics for this, multi-class calibration and proper scoring rules. 

This lecture was taught by [Peter Flach](http://people.cs.bris.ac.uk/~flach//).

---

## Detailed Notes 
**Calibration** is about whether the confidence scores your machine learning model outputs actually mean what they claim to mean. Many classifiers are good at *ranking* (putting spam emails at the top of the list) but bad at *calibration* (knowing how confident to be). Just because the model returns values between 0.0-1.0, does not mean that these are real probabilities. 
 
<div class="example-box" markdown="1">
**Spam Example**: <br>
Imagine you have an email spam classifier that says: <br>
	•	Email A: "90% chance this is spam"	 <br>
    •	Email B: "90% chance this is spam"	 <br>
    •	...and so on for 100 emails  <br>
If your classifier is well-calibrated, then when you check those 100 emails that it labeled "90% spam", you should find that about 90 of them actually ARE spam.
</div>

Well-calibrated probabilities are useful for good decision making, meaningful interpretation (only calibrated estimates deserve to be called "probabilities") and meaningful threshold selections rather than arbitrary 0.5 cutoffs. 

The most common problem is *overconfidence*, which is when models are too sure of itself. *Underconfidence* is less common but happens when classifiers are overly cautious and rarely give predictions far from 50%.

<div class="example-box" markdown="1">
**Spam Example**: <br>
If you find that only 60 are actually spam, then your classifier is **overconfident**, it's too sure of itself. 
If you find that 98 are actually spam, then your classifier is **underconfident**, it's not confident enough.
</div>

**Binning**
To check if your model is calibrated, you need to group predictions into **bins** and see if the frequencies match. However, a model rarely gives the *exact* same prediction twice.

<div class="example-box" markdown="1">
**Binning Problem Example**<br>
Imagine you have 1000 spam predictions like 73.2%, 73.7%, 74.1%... all slightly different numbers. To check calibration, you need to group similar predictions together, but how?
- *Too few bins* (e.g., 0-33%, 34-66%, 67-100%): A 35% prediction and 65% prediction end up in the same bin, even though they're very different
- *Too many bins* (e.g., 73-74%, 74-75%, 75-76%): Each bin might only have 1-3 emails, so the percentages become meaningless (one email can swing a bin from 0% to 100%)
- *Just right* (e.g., 70-80% contains 150 emails): Enough examples to trust the statistics, specific enough to be useful
</div>

There's no mathematical formula for the perfect number of bins. It depends on your data size, how predictions are distributed, and your specific needs. This binning challenge affects both how you measure calibration and how you fix it.

**Perfect calibration ≠ useful classifier**
Perfect calibration doesn't guarantee a useful classifier. A model that always predicts 50% spam probability would be perfectly calibrated (if exactly half your emails are spam), but it would be completely useless for decision-making. 


### Evaluating Calibration

How do you actually measure whether your model is well-calibrated? There are two main approaches: visual methods and numerical metrics.

**Reliability Diagrams**
The most intuitive way to evaluate calibration is through **reliability diagrams**. These plot predicted probabilities (x-axis) against actual frequencies (y-axis). If your model is perfectly calibrated, all points should lie on the diagonal line.

<div class="example-box" markdown="1">
**Reading Reliability Diagrams**: <br>
• Point at (0.8, 0.6): When your model predicts 80%, it's actually right only 60% of the time → overconfident <br>
• Point at (0.3, 0.5): When your model predicts 30%, it's actually right 50% of the time → underconfident <br>
• Point at (0.7, 0.7): When your model predicts 70%, it's right 70% of the time → well-calibrated
</div>

**Expected Calibration Error (ECE)** provides a single number that quantifies *how much red area* there is in your reliability diagram - essentially measuring the average distance from the diagonal line. You bin your predictions (e.g., 0-20%, 20-40%, 40-60%, etc.), calculate the average prediction in each bin, calculate the actual frequency in each bin, measure the difference and take a weighted average of thos differences. 

<div class="example-box" markdown="1">
**ECE Example**: <br>
• Bin 1 (0-20%): Average prediction = 15%, Actual frequency = 25% → Difference = 10% <br>
• Bin 2 (20-40%): Average prediction = 30%, Actual frequency = 28% → Difference = 2% <br>
• ECE = weighted average of all bin differences
</div>

Instead of averaging differences across bins, **Maximum Calibration Error (MCE)** just reports the *largest single deviation*. This tells you about your worst-calibrated region.

If you only cared about minimizing ECE, you could build a "classifier" that always predicts the same probability for every email - say 60% spam if 60% of your emails are actually spam. This would get perfect ECE (zero calibration error) because your average prediction would exactly match the overall frequency. But this classifier would never help you distinguish spam from legitimate emails and provides no useful information for filtering your inbox.

### Multi-class calibration

In binary classification, you have one probability to worry about (e.g., 70% spam). In multi-class, you have a *probability vector*, where multiple probabilities that must sum to 1. That means we can't easily put them in bins. So how do we deal with this? 


<div class="example-box" markdown="1">
**Example**: Your classifier predicts [.6, .3, .1] for an email where the numbers represent spam, promotional and personal respectively. Another prediction is [.6, .2, .2] for the same classes. Both have the same "confidence" in spam (60%), but they're making very different statements about the other classes.
</div>

**Confidence Calibration**
The simplest approach is to only consider the *highest predicted probability* and ignores everything else. In the example above, that means that both predictions would be treated identically. This is what many recent papers on neural network calibration use, but the problem is that this completely ignores the distribution over other classes. 

**Class-wise Calibration**
Alternatively, you can treat each class separately using a "one-vs-rest" approach. In the earlier example it would separately check if "spam" probabilities are calibrated (spam vs. not spam), the "promotional" probabilities are calibrated ("promotional vs. not promotional) and the "personal" probabilities are calibrated (personal vs. not personal). The problem is you end up with multiple reliability diagrams that may contradict each other, and it's unclear how to combine them.


**Full Multi-Class Calibration**
Finally, you can look at the *entire probability vector* as a unit. Then you collect all predictions with the same vector (e.g., [0.6, 0.3, 0.1]) and check if the empirical class distribution matches this vector. The problem is that with many classes, you rarely get enough examples with the same exact vector

Proper multi-class calibration is still unsolved, because *binning becomes impossible* and *data sparsity*. With 3 classes, your probability space is 2-dimensional. With 10 classes, it's 9-dimensional. How do you create meaningful bins in high-dimensional space? And how do you get enough examples in each bin? 

Multi-class calibration is fundamentally harder because you're trying to calibrate entire probability distributions, not just single numbers, and we don't have fully satisfactory solutions yet.


### Proper Scoring Rules
Instead of grouping predictions into bins, **proper scoring rules** evaluate calibration one prediction at a time. For each individual prediction, they calculate a penalty score based on what actually happened. Technically, proper scoring rules don't measure *pure calibration*. They measure *calibration loss* plus *refinement loss* (how well your model separates different classes).

<div class="example-box" markdown="1">
**Log Loss Example**: <br>
Your model predicts: 70% chance of spam. <br>
*Reality*: It actually IS spam. Penalty: -log(0.7) = 0.36 <br>
*Reality*: It's NOT spam. Penalty: -log(0.3) = 1.20 (much higher penalty)

The better calibrated your model, the lower your average penalty across all predictions.
</div>

The most common proper scoring rules are *log loss* (what most deep learning models optimize during training, which penalizes wrong predictions exponentially) and *brier score* (which uses squared differences and is more forgiving of confident wrong predictions than log loss).

This works for multi-class problems as it works directly with probability vectors and does not require binning, it scales to any number of classes (without suffering from the curse of dimensionality), can calculate scores for individual predictions and is a well-established theory. 

<div class="example-box" markdown="1">
**Why proper scoring rules ≠ calibration**: <br>
A model that always predicts [0.33, 0.33, 0.33] for three classes might be perfectly calibrated if classes are equally distributed, but it would still get a poor proper scoring rule score because it's not useful for decision-making.
</div>

## Conclusion 
Calibration involves tradeoffs everywhere: which method to use, how to bin your data, whether to use training or validation sets for calibration. There are no simple answers, which is why calibration is "an art as well as a science."

The field is still evolving, with multi-class calibration remaining an open research problem. When evaluating calibration approaches, be critical: why does one method work better than another? What assumptions is it making? 

Most importantly: **always validate on held-out data** and remember that perfect calibration alone doesn't guarantee a useful classifier.

<div class="note" markdown="1">

## Additional Notes

### Calibration Techniques Overview
1. **Platt Scaling** - Fits a sigmoid curve to your classifier's scores
   - Good for: Underconfident classifiers or models that don't output probabilities at all (like SVMs)
   - Bad for: Overconfident classifiers (makes the problem worse)
   - Assumes: Score distributions are normal with equal variance across classes

2. **Isotonic Regression** - Creates a step-wise calibration map using ranked data
   - Good for: Any monotonic miscalibration pattern, doesn't assume specific distributions
   - Bad for: Small datasets (becomes too brittle)
   - Method: Uses ROC curve analysis to find optimal probability bins

3. **Beta Calibration** - Uses beta distributions instead of normal distributions
   - Good for: Both overconfident and underconfident classifiers
   - Advantage: Can recognize when a classifier is already well-calibrated and leave it alone
   - Parameters: Has 3 degrees of freedom vs. 2 for Platt scaling, allowing more flexible corrections

### Measuring Calibration Quality
- **Reliability Diagrams**: Visual plots showing predicted probability (x-axis) vs. actual frequency (y-axis)
- **Expected Calibration Error (ECE)**: Averages the differences between predicted and actual probabilities across bins
- **Binning Trade-off**: Too few bins = oversimplified view, too many bins = not enough data per bin

### Multi-class ECE
- **Confidence ECE**: Only uses the winning class probability (throws away information)
- **Class-wise ECE**: Gives you multiple numbers that are hard to interpret together
- **Full multi-class ECE**:  Still an open research problem due to the "curse of dimensionality" in binning

### Key Calibration Principles
- **Perfect calibration ≠ useful classifier**: A model that always predicts 50% can be perfectly calibrated but useless
- **Ranking vs. calibration**: Many models rank well (spam at top, ham at bottom) but have poor probability estimates
- **Validation data required**: Never calibrate on the same data you trained on - always use a separate validation set

</div>
