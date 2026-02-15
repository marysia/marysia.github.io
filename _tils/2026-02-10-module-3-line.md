---
title: "Module 3: Statistical Foundation of AI Evaluation"
date: 2026-02-10
series: "International Programme on AI Evaluation: Capabilities and Safety"
---


## Overview
This lecture establishes the statistical foundations necessary for AI evaluation. Statistics in AI evaluation are frequently misused or misinterpreted, and impressive-looking numbers that seem authoritative may be meaningless or misleading without understanding their underlying assumptions and limitations.

This lecture was taught by [Line Clemmensen](https://www.imm.dtu.dk/~lkhc/).
--- 

## Key Concepts & Takeaways

**Takeaways** 
1. AI evaluation often fails because we mistake performance on (clean) benchmarks for real-world capability
2. Every evaluation metric is just a number from a specific dataset with specific assumptions. We need to understand what those numbers *actually* mean (such as their uncertainty, their limitations, their underlying assumptions).
3. Match your satistical methods to your deployment goals. 

**Core Concepts Covered**
- **Validation vs test sets**: Separate data for model selection vs final assessment
- **Bias-variance tradeoff**: Why simple models sometimes beat complex ones  
- **Cross-validation**: Using data splits to estimate performance without separate validation
- **Nested cross-validation**: Two-loop approach separating model selection from assessment
- **Independence assumption**: Why correlated data breaks standard evaluation methods
- **Data leakage**: How preprocessing can contaminate your evaluation results
- **Bootstrapping**: Creating confidence intervals by resampling your data
- **Error analysis**: Systematically examining failures to understand model limitations
- **Representative sampling**: Whether your data reflects your target population
- **Miniature vs coverage sampling**: Optimizing average performance vs ensuring fairness


---

# Detailed Notes

## Examples of Evaluation Failure

**MNIST: clean data vs. real world**
The MNIST dataset is a collection of 70,000 handwritten digits, originally from the US postal service data. It has consistent grayscale images, balanced classes and a large sample size. That makes it perfect for benchmarking machine learning model, right? But what happens when your digit recognition system encounters an envelope decorated with flowers, or one with coffee stains, or handwriting that doesn't fit the standardized format? The clean, controlled nature of MNIST, which is its greatest strength for model comparison, suddenly becomes its greatest limitation for understanding real-world performance.

We need controlled conditions to compare systems fairly, but those same controlled conditions may tell us little about how systems will behave when deployed. Real-world AI systems typically face fewer labeled examples, more sources of variation, missing or incorrect labels, and complexity that no benchmark can fully capture.

**Google Flu Trends: correlation vs. causation**
In 2009, Google claimed it could forecast flu outbreaks by analyzing search queries for flu symptoms. The system even had a public dashboard. Yet by 2015, Google quietly shut it down. 

What went wrong? It turns out the system confused correlation with causation. When people search for "flu symptoms," it doesn't necessarily mean they *have* the flu, it might just mean they're *worried* about the flu. Just like during COVID-19, when  when everyone was searching for symptoms but relatively few people actually had flu. 

Evaluation that looks statistically rigorous can still fail if it's based on the wrong assumptions about what the data represents. The correlation between search queries and flu cases held during the training period but broke down when deployed, leaving users with a system that predicted "flu awareness" rather than actual flu outbreaks.

## Core Evaluation Concepts

**Validation set & Test set** 
While developing your model, you need to make choices: the best data to use, the best model type to use, the best hyperparameters to use. You typically make these choices by training a few different variants of the model, and testing this against an unseen set of data. 

However, the more often you iterate in your choices and optimize for performance on this unseen set of data, the more bias you introduce and less likely the model is to generalize well. When you try ten different models and pick the best performer, you've essentially overfit to that dataset. Using those same numbers to tell stakeholders how well the system will work gives an overly optimistic picture. 

This is why you need to keep your test data completely separate from the model development process. Only then can you get unbiased estimates of real-world performance. This is why we typically have both a **validation set** and a **test set**. The validation set is used for model selection; the test set is used for model assessment, characterizing how well your final model will perform for end users.


**Bias-Variance trade-off**
Bias measures how far off your model's average prediction is from the true value. Variance measures how much your model's predictions change when you train on different datasets. The bias-variance tradeoff matters because you can't minimize both bias and variance simultaneously.

Consider polynomial fitting examples. Imagine you're trying to fit a curve to data that actually comes from a third-degree polynomial, but you don't know that.

If you fit a simple linear model, you get consistent results across different datasets, but they're consistently wrong. This is high bias, low variance. The model is too simple to capture the underlying pattern. If you fit a very complex ninth-degree polynomial, you can perfectly fit your training data, but the model varies wildly depending on which specific data points you happened to observe. This is low bias, high variance. The model is so flexible it fits noise rather than signal.

The sweet spot lies between these extremes. A third-degree polynomial (matching the true underlying function) provides the right balance, generalizing well to new data. But with limited data, the simpler model might actually perform better on new observations, even though it's "wrong" about the underlying function.

This explains why evaluation results depend so heavily on how much data you have. More data allows more complex models to succeed, but with limited data, simpler models often generalize better despite being less accurate on training data.

**Cross-validation**
 Instead of setting aside a separate validation set, cross-validation splits your data into chunks, trains on most of them, tests on the remainder, and repeats this process. This lets you use more data for training while still getting performance estimates for model selection and hyperparameter tuning.

But cross-validation assumes your data points are independent and identically distributed, which frequently breaks down in practice. When you have multiple observations from the same individual, data collected over time, or samples from different regions, you need specialized approaches. Leave entire individuals or countries out of training if that's how you plan to deploy. And crucially, all preprocessing like normalization must happen within each fold using only training data, otherwise you leak information and get overly optimistic results.

**Nested Cross-Validation**
When you need both model selection and model assessment, regular cross-validation isn't enough because it conflates the two. Nested cross-validation solves this by using two loops: an inner loop for model selection (choosing hyperparameters) and an outer loop for assessment (estimating final performance).

To do this, you take your data and split it into outer folds. For each outer fold, use the remaining data for inner cross-validation to select your best model. Then test that selected model on the held-out outer fold. This gives you unbiased performance estimates because the outer test data was never used for model selection.

After nested CV, you typically retrain your best model on all available data before deployment, since you want to use as much data as possible for your final production model.


**Data Leakage**
Data leakage happens when information from your test set accidentally influences your training process, making your evaluation overly optimistic. The most common culprit is preprocessing. If you normalize your entire dataset before splitting into train/test, you've used information from the test set (its mean and variance) to transform your training data.

The solution is simple: do all preprocessing within each cross-validation fold using only training data, then apply those same transformations to test data. The "vault test" provides a check: you should be able to take a single new observation and process it using only information computed from training data. If you can't, you've probably introduced leakage.


**The Independence Assumption**
The independence assumption becomes particularly problematic with time series data, where the order of observations matters fundamentally. You can't randomly shuffle time series data and expect meaningful results, as you need to respect temporal structure by only using past data to predict future outcomes.

This extends beyond obvious time series to any situation where observations are naturally grouped or correlated. Medical data often involves multiple measurements per patient. Agricultural data might involve multiple fields per farm. Social media data involves multiple posts per user. In all these cases, standard cross-validation can give misleadingly optimistic results because it doesn't properly simulate the challenge of generalizing to new groups.

The solution is to think carefully about your deployment scenario. If you want to generalize to new patients, leave entire patients out. If you want to expand to new geographic regions, leave entire regions out. The key is ensuring your evaluation matches your intended use case.

**Bootstrapping**
Even with proper cross-validation, you still get just a single performance estimate. One number that summarizes how well your model works. But what does that number actually mean? How confident should you be in it? What's the range of performance you might see in practice?

Bootstrapping provides a general answer to these questions. Instead of having one dataset and one performance metric, you create many slightly different datasets by sampling with replacement from your original data. You then train models on each of these bootstrap samples and examine the distribution of performance metrics.

This approach, developed by Bradley Efron in 1979, gives you uncertainty estimates without making strong distributional assumptions. Instead of reporting "accuracy = 85%," you can report "accuracy = 85% ± 3%" with a confidence interval that quantifies your uncertainty. This transforms evaluation from providing false precision to acknowledging the inherent uncertainty in performance estimates.

**Error Analysis**
Beyond getting overall performance numbers, you need to understand where and why your model fails. Error analysis involves systematically examining your model's mistakes to identify patterns and limitations. Again, don't do this on the test set! 

Start by comparing your model's performance to human-level or expert performance to understand whether you have a bias problem (model isn't learning the task well enough) or a variance problem (model isn't generalizing to new data). Then manually examine a sample of misclassified examples - maybe 100 cases if you have a large dataset. Look for common patterns: are there specific types of inputs that consistently cause failures? Are there systematic biases in how the model makes mistakes?

This analysis helps you understand your model's limitations and can guide improvements, whether that's collecting more data for underrepresented cases, adjusting your model architecture, or simply documenting known failure modes for users.


## Representative Data

The bootstrap approach highlights a deeper question: what population does your data actually represent? This connects statistical evaluation to broader questions of fairness and bias in AI systems. Line reviewed various definitions of "representative sample" from the statistics literature, finding that even experts disagree on what this means.

Some definitions focus on *miniature representation*, meaning your sample should be a scaled-down version of the population, maintaining the same proportions across all relevant characteristics. Others emphasize *coverage*, i.e. ensuring you have examples from all important subgroups, even if not in proportion to their population frequency.

The choice between these approaches depends on your goals. If you're optimizing for average performance across your user base, miniature sampling makes sense as it will minimize your overall error rate. But if you're building a system where individual fairness matters, coverage sampling might be better, ensuring robust performance across all subgroups even if it increases average error.

## Choice of Metric

Line illustrated this tradeoff through a challenging comparison: evaluating AI systems for cancer diagnosis versus predicting hospital no-shows. The statistical methods are similar, but the implications are vastly different.

For cancer diagnosis, individual accuracy is paramount. A false negative could cost someone their life. You might prioritize coverage sampling to ensure the system works well across all demographic groups, even if this slightly reduces average accuracy. Group fairness becomes critical as you can't have a system that works well for the majority but fails minorities.

For hospital no-shows, you're optimizing resource allocation at a population level. Here, average accuracy might matter more than individual fairness. A few incorrect predictions won't have life-threatening consequences—they just affect scheduling efficiency. Miniature sampling that reflects your actual patient population might be more appropriate.

This comparison reveals that there's no universal "best" approach to evaluation. The statistical methods must align with the deployment context, the stakes involved, and the values you're trying to optimize.

## Conclusion
Statistical evaluation isn't about applying cookbook formulas, it's about building intuition for what your numbers actually mean. Every evaluation metric comes with assumptions about data independence, representativity, and the relationship between your sample and the broader population you care about.

The key principles that emerged are:

**Understand your assumptions**: Every statistical method makes assumptions about your data. When those assumptions break down, so do your conclusions.

**Quantify uncertainty**: Point estimates without confidence intervals are almost meaningless. Always ask: how confident should I be in this number?

**Match methods to goals**: The "best" evaluation approach depends on how you plan to deploy your system and what you're trying to optimize.

**Separate selection from assessment**: Don't use the same data to both choose your model and characterize its performance.

**Think about generalization**: Your evaluation should simulate the conditions your system will face in deployment, not just perform well on clean benchmark data.

The Google Flu Trends example serves as a reminder that even statistically sophisticated evaluation can fail if it's built on wrong assumptions about what the data represents. The goal isn't just to get good numbers on benchmarks or test sets, it is to build systems that work reliably in the messy, complex world where they'll actually be deployed.

As AI systems become more powerful and more widely deployed, this statistical foundation becomes increasingly critical. The difference between rigorous and sloppy evaluation isn't just academic, it's the difference between systems that work as expected and systems that fail catastrophically when they encounter the real world.

---

<div class="note" markdown="1">

## Additional Notes

### Cross-Validation Variants
- **K-fold**: Standard approach with k chunks, but produces biased estimates due to overlapping training sets
- **Leave-one-out**: Special case that can severely overfit when combined with normalization
- **Group-based**: Essential when observations aren't independent (individuals, time series, geographic clusters)
- **Time series**: Requires respecting temporal order—can't use future to predict past
- **Nested**: Separates model selection (inner loop) from assessment (outer loop)

### Data Representativity Concepts
- **Assertive claim**: Simply stating data is representative without justification (avoid this)
- **Miniature**: Scaled-down version maintaining population proportions
- **Coverage**: Ensuring diversity across important dimensions
- **Absence of selective forces**: Controlling for systematic collection biases

### Measuring Representativity
- **Reflection**: Distributional similarity (Kolmogorov-Smirnov, Wasserstein distance)
- **Coverage**: Diversity measures (combinatorial entropy, geometric volume)
- **Representative**: How well centroids represent groups

</div>
