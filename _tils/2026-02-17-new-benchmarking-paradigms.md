---
title: "Module 4: New Benchmarking Paradigms"
date: 2026-02-17
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

This lecture covered how benchmarks and leaderboards work in AI research. The focus was on understanding why competitive ranking on standardized tests has become central to how AI systems get developed and improved.

This lecture was taught by [Lorenzo Pacchiardi](https://www.lorenzopacchiardi.me/).

---

## Key Takeaways & Concepts
AI research embraces an "anything goes" philosophy where you can try any architecture, training method, or data preprocessing approach, but this freedom makes systems hard to compare.

Benchmarks provide the necessary constraint. You can explore freely during development, but eventually you have to submit to competitive empirical testing on standardized tasks.  It's not a perfect system, as test set reuse makes absolute performance claims unreliable, and no benchmark can capture all aspects of intelligence. But the ranking comparisons remain robust enough to guide research priorities and identify promising directions.

1. Benchmarks create progress through the "leaderboard effect", as competitive ranking motivates researchers to keep improving their methods.
2. Rankings between models stay reliable even when absolute performance scores become misleading due to test set reuse.
3. Benchmark saturation is inevitable, forcing the field to create harder evaluation tasks.
4. The shift from narrow AI to general AI has created a "plurality of benchmarks" problem where no single test can capture all capabilities.


---

## Detailed Notes

### What Makes a Benchmark Work

A benchmark is three components working together: the dataset (collection of inputs and expected outputs), the evaluation metric (how you score model performance), and the evaluation protocol (the specific conditions under which you test).

The evaluation protocol matters more than people realize. You can take the same dataset and metric but get completely different results by changing how you preprocess the data or, in the case of language models, how you structure your prompts.

<div class="example-box" markdown="1">
**LLM Prompting Protocol Variations** <br>
Take the GSM8K math benchmark. You could evaluate the same LLM using different prompting approaches: zero-shot ("Solve this problem"), few-shot (showing 3 example solutions first), or chain-of-thought ("Let's think step by step"). The same model on the same dataset with the same accuracy metric can get dramatically different scores just based on how you structure the prompt. These protocol differences can completely change which models appear to perform best.
</div>

This is why reproducibility in AI evaluation requires documenting not just what dataset and metric you used, but exactly how you applied them. Small protocol changes can make the difference between a model looking good or bad.

### The Leaderboard Effect

Benchmarks measure how well models perform, but they also have a secondary effect: they influence what researchers work on. When researchers can easily compare their methods against others on a standardized test, it creates what's called the "leaderboard effect."

Having a standardized benchmark gives researchers clear feedback on whether their ideas actually improve performance. When something works well and climbs the leaderboard, the broader research community also pays attention and tries to build on those successful approaches. This guides research efforts toward approaches that perform better on that specific benchmark, and performance keeps climbing until the benchmark gets saturated. 

<div class="example-box" markdown="1">
**The 2012 ImageNet Competition** <br>
The ImageNet competition ran annually with steady but slow improvements in image classification accuracy. Then in 2012, AlexNet (a convolutional neural network) achieved a huge jump in performance that surprised the computer vision community. This single result shifted the entire field toward deep learning approaches. Within a few years, neural networks dominated not just ImageNet but most computer vision tasks.
</div>

This cycle explains why AI progress often looks like sudden breakthroughs followed by rapid incremental improvements. The breakthroughs create new leaderboard leaders, then the community optimizes those approaches until the next breakthrough.

Capabilities that don't have widely-used benchmarks are less likely to be optimized for, since researchers can't easily tell whether their changes help or hurt performance in these areas.

### Why Rankings Matter More Than Scores

The absolute performance numbers on benchmarks are often misleading, but the rankings between models stay reliable.

This happens because of test set reuse. Individual researchers might try to avoid overfitting to test data, but the research community as a whole ends up doing "repeated adaptive testing." Every time someone publishes a new method that beats the leaderboard, they've essentially optimized on the test set indirectly. This inflates all the absolute scores, but it affects all models roughly equally, so the relative ordering between them remains valid.


<div class="example-box" markdown="1">
**ImageNet vs ImageNet-V2 Study** <br>
Researchers created ImageNet-V2, a new dataset with the same task (image classification) but completely different images from ImageNet. When they tested various models, the absolute accuracy scores were different - most models performed worse on the new data. But the ranking of which models were best stayed almost exactly the same. The model that was #1 on ImageNet was still #1 on ImageNet-V2.
</div>

This means when you see claims like "Model X achieved 93.7% on benchmark Y," the specific number isn't very informative about real-world performance. What matters is how that 93.7% compares to other models on the same benchmark.

This means that if you want to compare which approach works better between two different methods, benchmark rankings work well. If you want to predict how a specific model will actually perform in the real world, absolute benchmark scores are less reliable.

This means that if you want to know which model or approach performs better overall, benchmark rankings work well. You can reliably say "Model A outperforms Model B on math tasks" or "fine-tuning method X works better than method Y on the same base model." But if you want to predict how a specific model will actually perform in the real world, absolute benchmark scores are less reliable.


### From Simple Tasks to General AI

Traditional machine learning had a clean setup. Train your model on a dataset, test it on a held-out portion of the same dataset. As everyone used the same training data, benchmarks really were comparing algorithmic approaches and architectural choices.

Modern AI is messier. Companies like OpenAI, Anthropic, and Google each train their models using different proprietary datasets, architectures, and training methods that they don't share publicly. When these models get compared on benchmarks, you're not just comparing one variable. Instead, you're comparing the combined effects of completely different data, model designs, and training procedures that may be largely unknown to researchers.

<div class="example-box" markdown="1">
**Traditional**: Train a computer vision model on 50,000 CIFAR-10 images, test on 10,000 held-out CIFAR-10 images. Everyone uses the same training set, so you're comparing architectures and training methods.<br>
**Modern**: Train GPT-4 on unknown internet data, test on MMLU (college-level questions), GSM8K (math problems), ARC-AGI (visual reasoning), and dozens of other benchmarks. Compare against Claude trained on different unknown data. You're comparing everything: data quality, training methods, architectures, and post-training procedures.
</div>

This also makes contamination a bigger issue. With traditional ML, contamination meant accidentally including test examples in your training set. With internet-trained models, contamination can be much subtler - maybe your training data included websites that discussed the benchmark questions, or similar problems in a different format.

Additionally, general-purpose AI systems can potentially do thousands of different tasks, but you can only test them on a small subset. This means benchmark scores become less representative of overall capability than they were for narrow AI systems designed for specific tasks.


### The Saturation Problem

Every successful benchmark eventually gets "solved." When most competitive models achieve near-perfect performance, the benchmark loses its ability to distinguish between approaches. This forces the field to create harder tests.

<div class="example-box" markdown="1">
**The GLUE to SuperGLUE Evolution** <br>
GLUE (General Language Understanding Evaluation) was created to test language models on reading comprehension and reasoning tasks. Within a few years, models were achieving near-human performance. So researchers created SuperGLUE with harder questions. Now SuperGLUE is also approaching saturation, leading to even more difficult benchmarks like GPQA (graduate-level science questions) and ARC-AGI (abstract reasoning).
</div>

This creates an arms race between AI capabilities and evaluation difficulty. Each new generation of models forces benchmark creators to design more challenging tests. The field has moved from elementary school reading tests to graduate-level physics problems that PhD experts struggle with, all within the span of a few years.

This saturation problem actually shows that benchmarks are working as intended. If models keep getting better at the tasks we care about, we need harder tasks to keep measuring progress.

Some recent benchmarks try to stay ahead of this curve by using expert-created questions that require deep domain knowledge. GPQA uses PhD-level science questions that non-experts can't answer even with internet access. But if previous patterns hold, even these difficult benchmarks will likely get saturated as AI systems continue to improve.


<div class="note" markdown="1">

## Additional Notes

### Historical Benchmark Examples
- **Computer Vision**: MNIST (handwritten digits), CIFAR-10/100 (small colored images), ImageNet (14M images, 20K categories), COCO (object detection), Cityscapes (autonomous driving)
- **NLP**: GLUE/SuperGLUE (language understanding), SQuAD (question answering), WMT (machine translation competitions)
- **Specialized Domains**: Atari games (reinforcement learning), Critical Assessment of Protein Structure Prediction (AlphaFold), weather forecasting datasets
- **Modern LLM**: MMLU (multiple choice across 57 domains), GSM8K (grade school math), ARC-AGI (visual reasoning), GPQA (graduate-level science)

### Types of Learning Paradigms
- **Supervised learning**: Input-output pairs with ground truth labels
- **Reinforcement learning**: Reward functions instead of fixed correct answers  
- **Online learning**: Continuous model updates as new data arrives
- **Few-shot prompting**: Testing models with just a few examples in the prompt

### Common Benchmark Problems
- **Data contamination**: Test data accidentally included in training sets
- **Label errors**: Incorrect ground truth answers (GSM8K had many mislabeled problems)
- **Distribution shift**: Training and test data from different sources or time periods
- **Gaming**: Optimizing specifically for benchmark performance rather than real capability
- **Evaluation protocol variations**: Small changes in testing conditions affecting results

</div>
