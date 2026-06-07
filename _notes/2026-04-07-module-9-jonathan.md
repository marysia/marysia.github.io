---
layout: note
title: "Future of Work & the Economy (II)"
course: "AI Evaluation: Capabilities & Safety"
course_id: eval
module: "09"
module_title: "Real-World Evaluation: Societal Impacts of AI"
lecturer: "Jonathan Prunty"
lecturer_affiliation: "Leverhulme Centre for the Future of Intelligence, University of Cambridge"
date: 2026-04-07
---


## Overview

This lecture examines why AI systems that perform well on benchmarks often fail when deployed in business environments. It covers the disconnect between laboratory testing and real-world performance, introduces capability-oriented evaluation as an alternative approach, and explores future scenarios where machines will judge human versus AI suitability for workplace tasks.

This lecture was taught by [Jonathan Prunty](https://www.lcfi.ac.uk/people/jonathan-prunty) from the University of Cambridge.

---

## Key Takeaways & Concepts

1. Despite massive investment, 80% of businesses have no plans to adopt AI, and 95% of organizations implementing AI get zero return on investment due to the "pilot to production chasm."
2. Current benchmarks fail because they measure performance on simple, isolated tasks rather than the complex cognitive capabilities needed for real work environments.
3. Capability-oriented evaluation measures fundamental cognitive abilities like planning, memory, and social cognition rather than task-specific performance scores.
4. Businesses need evidence-based methods to evaluate which cognitive capabilities their tasks actually require before deploying AI systems.
5. Future workplace dynamics will involve "reverse Turing tests" where machines judge whether humans or AI should perform specific tasks, requiring transparent evaluation methods.

**Evaluation gap**: Disconnect between lab performance and real-world deployment success  
**Capability-oriented evaluation**: Measuring latent cognitive abilities rather than task performance  
**Cognitive dark matter**: Hidden capabilities humans possess that aren't captured in benchmarks  
**Task requirements analysis**: Determining which cognitive capabilities specific jobs actually need  
**Reverse Turing test**: Machines evaluating human versus AI suitability for tasks  
**Pilot to production chasm**: The failure rate between successful AI pilots and scaled deployment

---

## Detailed Notes
Business leaders are making bold claims about AI transformation. Mark Benioff says CEOs will no longer lead all-human workforces because we're entering the era of AI co-workers. Nvidia's CEO suggests AI adoption will be gradual, but when it hits, we might all end up making robot clothing. Anthropic's CEO recently claimed we're 6 to 12 months away from AI doing everything software engineers can do.

The money follows the hype. $240 billion flowed into AI investment in just the first quarter of 2026, creating intense adoption pressure for businesses worried about falling behind competitors.

Surveys of business integration, however, suggestthat while individuals increasingly use AI tools for tasks like email writing and document creation, formal enterprise adoption remains limited. The UK government found that only one in six businesses have integrated AI into their operations, and 80% have no active plans for systematic adoption. A study of 6,000 businesses across the US, UK, Germany, and Australia found that over 80% report no measurable impact on employment or productivity from AI integration efforts. Most striking was MIT's finding that despite $30-40 billion in enterprise AI investment, 95% of organizations are getting zero return on their integration projects.

<div class="example-box" markdown="1">
**The Pilot to Production Chasm** <br>
MIT researchers documented what they called the "pilot to production chasm." While many businesses investigated integrating AI into their workflows and some ran successful pilots, only 5% actually achieved successful implementation at scale. The gap between proof-of-concept and production deployment represents billions in wasted investment and countless failed projects. This doesn't account for individual productivity gains from tools like ChatGPT, but rather the failure to embed AI systems into business processes.
</div>


### Evaluation Gap

The evaluation gap is the disconnect between good lab performance and poor real world adoption exists for several interconnected reasons: 

**AI behaves like a normal technology** rather than the revolutionary force it is sometimes claimed to be. Like electricity or the internet, transformative technologies face structural barriers that slow their spread through organizations. Electric dynamos existed for 40 years before producing measurable productivity gains because businesses needed time to reorganize workflows, train workers, and overcome regulatory hurdles.

**Benchmarks are fundamentally broken** as predictors of real-world performance. Most benchmarks are constructed through web scraping, crowdsourcing, or even AI generation without the statistical controls standard in psychology research. Unlike randomized controlled trials that isolate specific variables, benchmarks lack control groups and often contain multiple confounding factors.

The benchmark lifecycle follows a predictable pattern: creation, contamination, saturation, then abandonment. AI systems essentially get taught to the test, improving benchmark scores without necessarily improving the underlying capabilities those benchmarks were meant to measure.

**Performance scores miss the underlying capabilities** that determine real-world success. Consider a hypothetical "jump bench 500" containing 500 different high jumps. Knowing an athlete passed 75% tells you little about their actual jumping ability. But knowing they consistently pass jumps up to 2 meters and fail above that level gives you a capability estimate that predicts performance on new jumps.

**Different kinds of intelligence** create additional evaluation challenges. Anthropomorphism leads us to assume that identical outputs reflect identical cognitive processes. When AI systems and humans both answer questions correctly, we might assume they're using similar reasoning, but AI systems often use pattern matching or statistical shortcuts rather than genuine understanding.

Anthropocentrism causes the opposite problem. When AI systems fail tests designed for humans, we might underestimate their capabilities. Many psychological tests have multiple demands beyond the target capability. A theory of mind test might require reading comprehension, visual processing, and cultural knowledge in addition to perspective-taking ability.

<div class="example-box" markdown="1">
**The Bar Exam** <br>
ChatGPT passing the bar exam generated significant media attention and assumptions about its legal capabilities. For humans, bar exam performance predicts lawyering ability because humans possess correlated cognitive capabilities - legal knowledge comes bundled with planning, social reasoning, and ethical judgment. An AI system might have extensive legal knowledge without possessing the other cognitive capabilities needed to function effectively as a lawyer, making the bar exam a poor predictor of actual legal practice ability.
</div>


### Capability-Oriented Evaluation

Instead of measuring performance on specific tasks, **capability-oriented evaluation** attempts to measure the underlying cognitive abilities that enable performance across many different tasks.

The approach involves three main phases. First, cognitive capability profiling measures AI systems across fundamental cognitive abilities. Second, task requirements analysis determines which capabilities specific workplace tasks actually require. Third, mapping combines capability profiles with task requirements to generate suitability scores.

**Cognitive capability profiling** starts by defining capabilities of interest and developing clear criteria for measuring them. Recent work identified 18 cognitive capabilities across four broad clusters: memory-based capabilities, executive function capabilities (planning, cognitive flexibility, inhibitory control), spatial and perceptual capabilities, and social and communicative capabilities.

Each capability gets a detailed rubric explaining how to evaluate the level of demand for that capability in any given task. For example, the theory of mind rubric defines level 0 as "the task does not require modeling the mental states of others" and progresses through increasingly complex perspective-taking demands.

Using these rubrics, researchers can annotate existing benchmarks to understand what cognitive demands each item actually places on test-takers. This creates a large battery of items where each is characterized not just by its topic area but by the specific cognitive capabilities it requires.

When AI systems complete this battery, Bayesian measurement models can estimate their capability levels across all measured dimensions. Instead of getting a single performance score, you get a profile showing estimated capability levels for planning, memory, social cognition, and other fundamental abilities.

**Task requirements analysis** involves working with businesses to understand which cognitive capabilities their specific tasks require. This phase uses questionnaires and interviews with domain experts to map workplace activities to cognitive demands.

Participants first rank the importance of different work activities within their domain. Manufacturing workers might rate tool use and quality checking as highly important while rating social tasks as less relevant. Data workers might prioritize analytical and programming tasks over physical manipulation.

After familiarizing participants with the cognitive capabilities framework, they build profiles for specific tasks by allocating 100 "ability points" across five capabilities they consider most important for that task. This creates weighted importance matrices showing how much each capability contributes to success in different workplace activities.

**Mapping capabilities to requirements** combines the capability profiles with task importance weightings to generate suitability scores. A weighted power mean aggregates across different capabilities based on their importance for specific tasks, with adjustable parameters controlling how much strong capabilities can compensate for weak ones.

The output shows not just whether AI systems are suitable for different tasks, but why. Instead of a black-box recommendation, businesses get transparent explanations of which cognitive capabilities drive suitability decisions and where capability gaps exist.

### Reverse Turing Tests

The original Turing test challenged machines to convince humans they were human. A different dynamic is emerging in workplaces where machines increasingly evaluate whether humans or AI should perform specific tasks. While not technically a reverse Turing test, this represents a fundamental shift where machines judge human versus AI suitability rather than trying to imitate humans.

This dynamic already exists in many contexts. Crowdworking platforms like Amazon Mechanical Turk and Prolific use algorithms to decide which workers get which tasks. Ride-sharing and delivery platforms use machine judgment to allocate jobs. Content moderation systems decide whether human reviewers need to evaluate ambiguous cases.

The trend will likely accelerate as AI capabilities improve and businesses face more decisions about human versus AI task allocation. Medical triage systems might route routine cases to AI while sending complex cases to human doctors. Customer service systems might handle straightforward inquiries automatically while escalating emotionally sensitive situations to human agents.

<div class="example-box" markdown="1">
**Dynamic Call Routing** <br>
A telecommunications company might route thousands of daily calls using real-time demand assessment. Routine billing inquiries go to AI agents with unlimited capacity, while emotionally sensitive complaints get routed to human agents with limited availability. The system needs to quickly assess each call's cognitive demands and match them to appropriate capability profiles, making split-second decisions about human versus AI assignment.
</div>

**Potential problems** with machine-mediated task allocation include bias amplification and adversarial dynamics. If machine judges inherit biases from training data, those biases get scaled to affect thousands of allocation decisions rather than individual human judgments. Additionally, AI systems might learn to game evaluation systems by imitating human communication patterns or deliberately underperforming on capability assessments to avoid certain types of tasks.

**The capability-oriented evaluation** approach does offer some advantages for machine-mediated task allocation. Instead of evaluating humans through interviews and CVs while evaluating AI through benchmarks, both humans and AI systems could be assessed using the same cognitive capability framework. This enables valid comparisons and evidence-based allocation decisions with transparent explanations of why specific assignments were made.

The approach also makes gaming more difficult. AI systems might easily imitate human communication patterns to fool surface-level evaluations, but faking comprehensive cognitive capability profiles requires sophisticated manipulation across multiple cognitive dimensions and thousands of test items.

### Limitations and Open Questions

Capability-oriented evaluation faces several challenges that limit its current applicability.

**Human subjectivity** in rating capability importance represents a significant limitation. Domain experts understand their tasks but aren't necessarily experts in cognitive psychology. Their intuitions about which capabilities matter most might not align with objective measures of what actually predicts performance.

**Task complexity** creates additional challenges. Many workplace tasks involve implicit knowledge and tacit skills that experts struggle to articulate. Defining the cognitive demands of complex, multi-step processes requires more sophisticated analysis than current questionnaire methods provide.

**Scalability concerns** limit practical implementation. Having cognitive psychologists interview every business leader isn't feasible for widespread adoption. The approach needs more automated methods for task analysis and capability requirement estimation.