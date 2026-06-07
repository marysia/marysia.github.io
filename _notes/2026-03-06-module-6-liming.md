---
layout: note
title: "Construct-Oriented Evaluation"
course: "AI Evaluation: Capabilities & Safety"
course_id: eval
module: "06"
module_title: "Construct-Based Evaluation"
lecturer: "Liming Jiang"
lecturer_affiliation: "Microsoft Research Asia"
date: 2026-03-06
---


## Overview

This lecture introduces construct-oriented evaluation as an alternative to traditional task-based AI assessment, borrowing methods from psychology to better evaluate AI models. 

This lecture was taught by [Liming Jiang](https://liming-jiang.com/) from Microsoft Research Asia.

## Key Takeaways & Concepts

1. AI systems have measurable underlying "constructs" (like comprehension, reasoning, language modeling) that explain their behavior across different tasks.
2. Construct-oriented evaluation offers three advantages: predicting performance on new tasks, explaining why models succeed or fail, and ensuring test quality through systematic validation.
3. A three-stage pipeline exists for implementation: identify relevant constructs, develop measurement tools, and validate test quality.

- **Constructs**: Underlying factors that explain behavior patterns
- **Operationalization**: Converting abstract constructs into measurable behaviors  
- **Reliability**: Consistency and stability of test results
- **Validity**: Whether tests measure what they claim to measure
- **Top-down approach**: Identifying constructs through observation and expertise
- **Bottom-up approach**: Extracting constructs from empirical data patterns

---

## Detailed Notes

General-purpose AI system change the way we need to think about evaluation. Instead of building from scratch, we can look at how psychologists measure general-purpose intelligence in humans. The field of psychometrics has spent decades developing theories and methods, with construct-oriented evaluation as its foundation.

*Constructs* are underlyiing factors that explain behavior patterns. Depression, creativity, leadership, emotional intelligence are all constructs. When someone consistently manages emotions well, reads social situations accurately, and helps others through difficult feelings, we infer they have high emotional intelligence. The construct is the best explanation for these observable behaviors.

### Evidence for AI Constructs

Do AI systems actually have constructs? Research shows that three factors (comprehension, language modeling, and reasoning) can explain 82% of the variance in model behavior across 27 different tasks. Regardless of whether these AI constructs mirror human ones or are entirely unique, they exist and they explain behaviors.

This creates the foundation for construct-oriented evaluation in AI. Instead of just measuring task performance, we can measure the underlying factors that drive that performance.

### Advantages

**Predictive Power**
Construct-oriented evaluation enables two types of prediction. At the task level, if you know an AI's reasoning ability level and a new task requires higher reasoning than the AI possesses, you can predict it will likely fail. At the real-world level, constructs predict future outcomes just like they do for humans. Academic performance correlates with reasoning ability, job satisfaction with certain personality traits, and interpersonal relationships with emotional intelligence.

**Explanatory Power**
Task-based evaluation can't explain why models succeed or fail. Two AI systems might score identically on a test but have completely different ability profiles. Construct-oriented evaluation decomposes performance into different dimensions, revealing specific strengths and weaknesses. This is like the difference between knowing someone scored 120 on an IQ test versus knowing they excel at verbal reasoning but struggle with spatial tasks.

**Quality Assurance**
Traditional evaluation can't answer basic questions about test quality: did we measure what we intended to measure? Are the results consistent and stable? Psychometrics provides systematic methods for testing reliability (consistency) and validity (whether you're measuring the right thing). This quality assurance is largely missing from current AI evaluation practices.

### The Three-Stage Implementation Pipeline

**Stage 1: Construct Identification**
Two approaches work together here: top-down and bottem-up. The top-down approach starts with real-world observations and expert knowledge. Hallucination in AI systems was identified this way, as we noticed AI systems often generate incorrect or fabricated information, which affects performance and user experience.

The bottom-up approach extracts patterns from empirical data without clear assumptions about what you'll find. The Big Five personality traits were developed this way, analyzing thousands of personality descriptive terms to find five factors that explain most variance in human behavior. For AI, we can apply similar techniques to the massive amounts of performance data already collected across tasks and benchmarks.

These approaches complement each other. Top-down provides theoretical grounding, while bottom-up ensures you haven't missed important patterns in the data.

**Stage 2: Construct Measurement**
**Operationalization** converts abstract constructs into measurable behaviors. For emotional intelligence, this means distilling messy real-world observations (like noticing someone consistently navigates office drama well, reads social situations accurately, and helps others through difficult feelings) into specific testable aspects: perceiving emotions, understanding emotions, and regulating emotions. This converts spontaneous, random behaviors into selected, representative ones and transforms unsystematic observations into standardized measurement.

Guidelines specify overall test features like proportions of different construct aspects (a math test might be 40% geometry, 30% algebra, 30% data analysis), item formats (multiple choice versus open-ended), and time limits. The goal is ensuring test items capture your target construct while dodging interference from irrelevant factors (like making sure a reasoning test doesn't accidentally become a reading comprehension test because of complex wording).

Advanced measurement models like Item Response Theory and Confirmatory Factor Analysis beat simple scoring (just adding up right answers) by accounting for aspects like some questions are harder than others, some are better at distinguishing high performers from low performers. These models put construct levels and item difficulty on the same scale, so you can compare performance across different tasks and predict how someone will do on new tasks.

**Stage 3: Test Validation**
**Reliability** measures consistency and stability. Different types address different error sources: test-retest reliability catches time-related problems (like an AI reasoning test giving different scores when run in the morning versus evening due to server load), internal consistency catches content problems (like a "math ability" test that accidentally includes reading comprehension questions), and inter-rater reliability catches scoring problems (like two researchers rating AI-generated essays with completely different standards for "creativity").

**Validity** measures whether tests actually measure intended constructs. Structural validity checks if the test's internal structure matches theoretical expectations. For example, an emotional intelligence test should have distinct sections for perceiving, understanding, and regulating emotions, but you might discover that items designed to measure "understanding emotions" are actually measuring "regulating emotions." Criterion-related validity assesses whether test scores predict important external outcomes, like using IQ tests to predict academic performance or checking mental health questionnaires against professional diagnoses. Convergent and discriminant validity ensure tests behave logically - a language proficiency test should correlate highly with writing ability tests but show no correlation with math or spatial navigation tests.

### Conclusion

This field is emerging with many unanswered questions. Even in psychometrics, after centuries of research on human evaluation, questions remain and precision continues improving. The same will be true for AI evaluation.

The practical path forward involves systematically adopting quality assurance practices that are standard in psychometrics but often missing from AI evaluation. This means testing reliability and validity, reporting test quality metrics, and continuously gathering evidence to improve evaluation methods.

The goal isn't perfect evaluation but better evaluation that provides more reliable, valid, and useful information about AI capabilities. Construct-oriented evaluation offers a proven framework for achieving this goal.
