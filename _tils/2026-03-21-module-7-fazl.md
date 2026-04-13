---
title: "Module 7: Probing Neural Networks"
date: 2026-03-21
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

Probing trains classifiers on neural network internal activations to understand what models learn internally, beyond just looking at outputs. This approach helps debug models that find unexpected shortcuts and reveals whether systems actually understand the concepts they appear to use.

This lecture was taught by [Fazl Barez](https://fbarez.github.io/).

---

## Key Takeaways

1. **Probing trains classifiers on neural network internal activations to understand what models learn beyond their outputs.** This helps catch shortcuts like skin cancer classifiers that learn to detect rulers instead of lesions.
2. **Probing shows what information is accessible in representations, not what the model actually uses for its output.** A concept can be encoded in layer 15 while the model reads from layer 20.
3. **Linear probes make stronger claims than complex ones about how information is stored.** Always start simple and explicitly justify any increase in complexity.
4. **Proper baselines matter more than raw accuracy when evaluating probes.** Majority class, random representations, and selectivity control for different confounders that make results look artificially good.
5. **High probe accuracy can come from the probe doing the computational work rather than reading existing structure.** Complex probes may solve tasks themselves instead of extracting information the model already computed.


---


## Detailed Notes

Models find unexpected shortcuts that pass every test but fail in deployment. These systems learn whatever correlates with the labels rather than the intended task, which seems to be the default behavior of empirical risk minimization. Test accuracy doesn't tell you which features the system is using or whether those features will still be present when the distribution shifts. The evaluation itself is blind to the learned mechanism.

<div class="example-box" markdown="1">
**The Skin Cancer Shortcut** <br>
A dermatology AI system achieved expert-level accuracy at detecting malignant lesions. But instead of learning to recognize cancer, it learned to detect rulers in clinical photos. Clinicians use rulers when photographing suspicious lesions, so the training data had rulers correlated with malignancy. The model passed all tests because the same bias existed in evaluation data, but it would fail completely on photos without rulers.
</div>

One way to look inside models is probing: training classifiers on the internal activations of neural networks to see what information they contain. By examining what's encoded in different layers and components, you might uncover structures and insights that output-only evaluation misses. You can't fix something if you can't see it.


### The Correlation vs Causation Problem

Probing shows what information exists in representations, not what the model uses for its output. This happens because probing trains a separate classifier to extract information from frozen activations, but it doesn't trace whether the original model's computation actually reads from those same activations when producing its final answer. This distinction affects every probing result and is where most interpretations go wrong.
<div class="example-box" markdown="1">
**Example**<br>
A concept can be encoded in layer 15 as a byproduct of earlier computation while the model actually reads from layer 20. The probe would find the information, but it doesn't trace where information flows or whether the model uses it. Just because you found something doesn't mean it was used. Just because you didn't find something doesn't mean it doesn't exist.
</div>

### What Probing Actually Measures

Probing trains a small supervised classifier on frozen model states to predict properties of the input, like part-of-speech tags or sentiment. The encoder never updates - only the probe learns from the frozen activations.

The encoder must stay frozen because if you allowed it to update toward the probe's labels, you'd be measuring whether the probe can represent the concept rather than whether it does. The fine-tuned encoder would reorganize its representation to make the probe's job easy, contaminating what you're trying to measure.

Probing sits between behavioral testing and mechanistic analysis. Behavioral testing gives no internal access but is fast and easy. Mechanistic approaches like circuit analysis reveal exact computations but require enormous effort. Probing asks a more modest question: is the information there? Not whether it's used, just whether it's accessible.

### The Linear Representation Hypothesis

Most probing relies on the linear representation hypothesis: features or concepts correspond to directions in representation space. Moving along the direction for concept C changes C while leaving others relatively unchanged.

<div class="example-box" markdown="1">
**Word Vector Arithmetic** <br>
The classic example: king - man + woman ≈ queen. Gender has a direction in the space, and royalty has a direction. The arithmetic works because these concepts are linearly encoded. You can add and subtract concept directions to get meaningful results.
</div>

This isn't arbitrary. Gradient descent with linear output layers selects for linear structures by construction. For class C to score high on the output, hidden representation H must project strongly onto the corresponding output weights. The constraint propagates backward through all layers, so linear structure in representations isn't surprising.

But the linear representation hypothesis breaks in three important ways:

**Superposition** happens when models represent more concepts than they have dimensions by using nearly orthogonal directions that overlap. Individual features aren't cleanly linearly separable. A linear probe might fail even if the concept is present, depending on which direction you're probing. This isn't a model failure - it's an efficiency feature of how we train systems by compressing large input dimensions into smaller spaces.

**Distributed encoding** spreads properties across multiple components because neural networks are highly distributed. None of these components can individually support linear decoding. If you project out one direction, you might not remove the information because it's spread across different directions.

**Compositionality** means that syntax is a tree, not a point or direction. The structure that probes can learn isn't understood through distance in vector space but through sparse tree distances or other relational measures.

The practical implication: a failed linear probe doesn't mean the absence of what you're looking for. It means that thing might not be linearly encoded, which is different.

### Types of Probing and Their Claims

Different probe types make very different claims about what they find. Mixing these conclusions is a common problem that leads to overinterpretation.

**Linear probing** uses logistic regression on hidden representations. Success means the concept is a direction in the representation space that's directly usable for activation steering, attribution, or editing. This makes the strongest geometric claim.

**Nonlinear probing** fits an MLP on hidden representations to test whether properties are recoverable in any form. This is useful when linear probes fail and you want to establish that information is present in some other form. But if the probe is sufficiently expressive, it may compute the property from simpler features rather than reading structure that's already there. High accuracy means the property is decodable from the internal representation, which is a much weaker claim than saying the property is encoded as a direction.

**Structural probing** learns a metric over representations for relational properties like syntax, where targets have structure rather than being simple classes. This makes claims about metric geometry, not classification boundaries.

**Causal probing** uses activation patching to establish whether the model actually uses information for its output. You run a clean input and cache activations, run a corrupted input with one key change, then substitute the clean activation into the corrupted run. If the output shifts toward the clean prediction, that activation is causally used for the output.

<div class="example-box" markdown="1">
**Linear vs Nonlinear Claims** <br>
Suppose a linear probe fails to detect sentiment in layer 10, scoring 55% accuracy. An MLP probe on the same layer achieves 85% accuracy. The linear probe failure doesn't mean sentiment isn't there - it means sentiment isn't linearly encoded. The MLP success means sentiment is decodable but tells you nothing about the geometric structure. These are completely different claims about what the model learned.
</div>

### Evaluation: Why Accuracy Means Nothing

High test accuracy without proper baselines is meaningless. Those baselines control for different confounders that can make random results look impressive.

**Majority class baseline** controls for dataset imbalance. If 80% of your labels are positive sentiment, guessing positive 80% of the time gives 80% accuracy just from imbalance. If your probe barely exceeds this, you've established that the dataset is imbalanced and the representation doesn't encode anything useful.

**Random representation baseline** uses the same probe and labels on activations from an untrained model. This controls for what's present in the data before training does anything. If a probe on random representations achieves 70%, your trained representation needs to substantially exceed 70% before you can claim training contributed useful signal. This baseline catches lexical shortcuts where properties correlate with word identity or frequency so strongly that probes achieve high accuracy by memorizing input patterns regardless of what the model learned.

**Selectivity** trains the same probe on the same representation with randomly shuffled labels. The accuracy you get is the ceiling on what the probe can achieve through pure memorization. No information in the hidden representation helps because labels are random. Selectivity is linguistically accurate minus control accuracy. High selectivity means representations contribute signal the probe can't manufacture. Low selectivity means most accuracy comes from the probe's own capacity, not the encoding.

<div class="example-box" markdown="1">
**The 80% Sentiment Trap** <br>
Your probe achieves 85% accuracy on sentiment classification. Impressive! But 80% of your dataset is positive sentiment, so majority class baseline is 80%. Random representations achieve 82%. Shuffled labels achieve 78%. Your selectivity is 85% - 78% = 7%. The probe is barely better than memorization, and most of its success comes from dataset imbalance rather than meaningful encoding.
</div>

### What Probes Have Revealed

Probes found that BERT layers organize somewhat like the classical NLP pipeline. Early layers encode morphology and basic syntax, middle layers encode semantic roles and coreference, and late layers encode world knowledge and factual associations. This hierarchy emerged from training to predict masked words with no explicit supervision for grammar or semantics.

Factual knowledge concentrates in later MLP layers, which researchers characterize as key-value memories. Language models can retrieve factual associations without fine-tuning by filling in blanks like "The capital of France is ___". This localization enabled editing approaches that modify specific facts by intervening on particular layers.

Truth appears to have an explicit direction in representation space. Researchers found linear directions where true statements project more strongly than false statements, achieved through unsupervised contrastive search. The probe for detecting truth achieved 90% accuracy, suggesting models' internal geometry distinguishes true from false in ways we can linearly decode even when their outputs don't reflect this.

But remember: these are accessibility findings, not usage claims. The information exists in these locations, but that doesn't prove the model reads from these locations when computing outputs.

### What Probing Cannot Tell You

Probes are passive observers that show what information is accessible without looking at the relation to surroundings. They read from representations without modifying the network, so they say nothing about whether computation producing the output passes through this localization or reads the signal.

Probes don't establish causal relevance. You need more rigorous approaches like activation patching for that. Probes don't establish mechanisms - they tell you what's encoded, not where or how it was computed. Probes don't establish completeness. You may find one encoding, but many others might exist, and the one you found may not be what the model uses.

<div class="example-box" markdown="1">
**The Context Problem** <br>
Researchers found a "truth direction" that worked well on factual statements. But when they tested the same direction on different contexts, it failed to transfer. A direction meaningful in one distributional setting didn't generalize to another. The linear hypothesis is a local approximation, and geometry changes across contexts. The directions you find may not be the directions that matter elsewhere.
</div>

### The Complexity Trap

Complex probes like deep MLPs are dangerous because they may solve tasks themselves rather than read existing structure. If the probe is sufficiently expressive, it's probably doing work the model wasn't doing in its true computation rather than finding encoding that's already there.

High accuracy from a complex probe means the property is computable from the internal representation, not that it's encoded there. L2 regularization doesn't help because it penalizes weight magnitude but doesn't reduce capacity to memorize. Only reducing the number of parameters addresses memorization concerns.

A practical guideline: if linear probes fail but MLP probes succeed, be explicit that your finding is decodability, not direction in representation space. These are very different claims about geometry.
