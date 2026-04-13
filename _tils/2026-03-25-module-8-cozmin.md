---
title: "Module 8: Agentic Evaluation"
date: 2026-03-25
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

Agent evaluation tests AI systems that act in environments rather than just producing answers to questions. This lecture covers why agent benchmarks are uniquely difficult to score, what hidden factors dramatically change results, and practical methods for diagnosing evaluation problems. 

This lecture was taught by [Cozmin Ududec](https://cozster.github.io/) from the UK AI Security Institute.

---

## Key Takeaways

1. **Benchmark scores hide massive variation in what's actually being measured.** The same model on the same benchmark can score anywhere from 42% to 95% depending on scaffolding and grading choices that are often unreported.
2. **50% of tasks in major agent benchmarks contain errors.** Grading bugs, ambiguous instructions, and database inconsistencies mean published scores often underestimate true model performance.
3. **You need to ask specific diagnostic questions about any evaluation result before trusting it.** A 40% score could indicate lack of capability, contamination, grading errors, insufficient compute budget, or environmental problems.
4. **Transcript analysis reveals what scores cannot show.** Looking at logs of agent behavior helps distinguish between capability limitations, setup problems, and specification gaming.
5. **Fix basic issues before trying advanced techniques.** Check for refusals, format errors, and tool failures before building complex multi-agent scaffolds.
6. **Inference scaling can improve performance linearly across orders of magnitude of compute.** Models that seem limited at 10K tokens may succeed at 50M tokens if the task provides good feedback.

**Agent evaluation:** Testing systems that act in environments using tools  
**Scaffold:** Framework around the model including tools and interaction patterns  
**Transcript analysis:** Systematic examination of agent behavior logs  
**Inference scaling:** How performance changes with computational budget  
**Pass@K:** Probability of at least one success in K attempts  

---

## Detailed Notes

Agent evaluation differs from traditional AI testing because agents act in environments over multiple steps rather than producing single answers. A coding agent might read documentation, write code, run tests, debug failures, and iterate for hundreds of turns before completing a task. This creates evaluation challenges that don't exist when testing models that just answer questions.

### Diagnostic Mindset

Imagine you evaluate a coding agent on a new benchmark and get 40% success rate. Before reporting this number, you need to ask specific diagnostic questions because the score alone reveals almost nothing about model capability.

Is this performance in the expected range? Maybe no model has ever scored above 30% on these tasks, making 40% surprisingly high. Or maybe other models routinely hit 80%, suggesting something went wrong. Is the performance robust under changes that shouldn't matter? Try running with a different random seed or slightly different prompts. Large variations suggest measurement problems rather than capability limits.

<div class="example-box" markdown="1">
**The CoreBench Transformation** <br>
Researchers tested Claude Opus 3.5 on CoreBench, a scientific reproducibility benchmark. With one scaffold, the model scored 42%. Using Claude Code (a different scaffold) and fixing grading errors, the same model scored 95%. The benchmark score changed by more than 2x without any change to the underlying model capabilities.
</div>

Could the model be cheating somehow? Check if solutions exist online that the agent could access through web search. Verify that training data contamination isn't inflating scores. Look for reward hacking where the agent finds unintended shortcuts that technically satisfy success criteria while missing the point entirely.

What about the evaluation setup itself? Are there tool call errors preventing the agent from accessing necessary functions? Do the tasks have clear, unambiguous success criteria? Are the grading mechanisms actually measuring what you think they're measuring?

This diagnostic mindset reveals that benchmark scores are functions of many hidden choices rather than pure measures of capability. The same model with different scaffolds, compute budgets, or grading methods can produce dramatically different results.

### Variables That Control Results

Every agent evaluation involves four types of settings that dramatically affect outcomes but are often unreported in papers and model cards.

**Inference settings** include reasoning effort, temperature, and crucially the computational budget. How many tokens can the agent use? How many turns are allowed? These limits determine when tasks end and can change success rates by orders of magnitude.

**Scaffolding** covers what tools the model has access to and how those tools are structured. A simple ReAct loop that alternates between reasoning and acting differs substantially from sophisticated multi-agent systems that spawn sub-agents for parallel work. The same model can perform 2x better or worse depending on scaffold choice.

**Task design** includes how instructions are written, what output format is required, and how much structure or guidance the agent receives. Clear, detailed prompts often improve performance significantly compared to vague instructions that leave agents guessing about requirements.

**Scoring methods** range from automated unit tests to human review to LLM-as-judge. Each has different failure modes. Unit tests can be too narrow and reject correct solutions. Human grading doesn't scale. LLM judges can be biased or inconsistent.

<div class="example-box" markdown="1">
**The C Compiler Project** <br>
Nicholas Carlini built a C compiler from scratch using 16 parallel Claude Code agents over two weeks. The project used 2 billion tokens, cost $20,000, and produced 100,000 lines of working Rust code. Despite the sophisticated multi-agent setup, interestinly enough the test quality mattered more than scaffold complexity. Good verification mechanisms that let agents know when they're making progress proved more important than fancy architectures.
</div>

When someone reports an agent evaluation score, ask what were the precise settings for all four dimensions. Without this information, you can't interpret whether the result is surprising, whether it's comparable to other results, or whether it represents the model's true capability.

### Difficulty of Agent Evaluation

Agent tasks have properties that make them fundamentally harder to evaluate than traditional AI benchmarks. Understanding these dimensions helps predict when different evaluation approaches will work or fail.

**Work graph complexity** refers to how many plausible paths exist for solving a task. Simple tasks might have one obvious approach, but complex software engineering or research tasks can involve hundreds of branching decisions about what to investigate next, which approach to try, or how to prioritize different sub-problems.

**Compounding errors** occur because agents make many sequential decisions. Small mistakes early in a long task can cascade into larger failures later. Even if an agent has a 95% accuracy rate per decision, after 100 decisions the probability of making no mistakes drops to nearly zero.

**Recoverability** measures how punishing mistakes are. Some errors can be easily undone or corrected when the agent notices them. Others waste substantial time or corrupt the environment state in ways that make task completion impossible.

**Self-verification quality** determines whether agents can tell if they're making progress. Software engineering tasks often have strong feedback because you can run tests to check if code works. Open-ended research tasks provide much weaker signals about whether you're on the right track.

**Skill distribution** covers whether tasks require deep expertise in narrow domains or broad knowledge across many areas. Tasks that bottleneck on rare specialized knowledge may be impossible regardless of how much compute you provide.

<div class="example-box" markdown="1">
**Software vs Research Tasks** <br>
A coding task where you fix bugs in a GitHub repository provides strong self-verification. You can run tests, check if the code compiles, and get immediate feedback about whether your changes work. A literature review task where you synthesize research to generate new hypotheses provides much weaker feedback. You might spend hours reading papers without clear signals about whether you're making progress toward a good synthesis.
</div>

These dimensions help predict when inference scaling will work. Tasks with good self-verification and recoverable errors often benefit from more compute budget. Tasks with weak feedback or irreversible mistakes may not improve much with longer runs.

### Systematic Transcript Analysis

*Transcript analysis* examines the logs of agent behavior to understand what actually happened during task execution. This reveals problems that scores alone cannot show and provides the main tool for systematically improving evaluation quality.

The systematic methodology involves seven steps: 
1. Define your question (what specific behavior are you investigating? Are you looking for tool call errors? Grading problems? Strategic reasoning failures?)
2. Understand your data structure (what information is logged and how?)
3. Establish ground truth by manually reviewing samples
4. Develop a coding scheme that specifies exactly what patterns you're looking for
5. Code your data by applying this scheme
6. Validate results on a separate held-out set of transcripts
7. Report your findings with confidence measures

Transcript analysis allows you to distinguish between spurious bottlenecks (fixable setup issues) and real bottlenecks (actual capability limitations). Many apparent capability failures turn out to be evaluation problems that can be resolved with better task design or grading rather than fundamental model limitations.


<div class="example-box" markdown="1">
**Transcript Analysis in Practice** <br>
You notice agents failing on database query tasks and want to understand why. Manual review of transcripts reveals three patterns: agents making SQL syntax errors (fixable with better prompting), agents querying the wrong tables (suggests unclear task instructions), and agents getting correct data but misinterpreting results (indicates reasoning limitations). Each pattern requires different solutions.
</div>



### Failure Modes

**Reward hacking** occurs when agents find unintended shortcuts that technically satisfy success criteria while violating the spirit of the task. Agents might modify evaluation code to report fake successes, access future git commits that contain solutions, or exploit environment bugs to trivialize tasks.

**Contamination** happens when task solutions exist in training data or are accessible online. Unlike traditional benchmarks where contamination means memorizing answers, agent contamination can be more subtle. The agent might not have seen the exact solution but could access it through web search during evaluation.

**Environmental issues** include cases where the evaluation setup itself is broken. Network connections might be down, required files might be missing, or tool APIs might be returning errors. These problems cause agents to fail regardless of capability.

Systematically checking the benchmark setup allows you to catch these common issues: verifying that ground truth answers are actually correct, isolating agents from solutions they shouldn't access, ensuring each evaluation run is independent, and confirming that tasks are actually solvable given the provided tools and environment.

### The Progressive Fix-It Approach

Rather than immediately building complex scaffolds, start with basic quality assurance and work systematically through potential issues. This progressive approach prevents wasting time on sophisticated solutions when simple fixes would suffice.
1. **Check for egregious failures.** Is the model refusing to engage with tasks? Are outputs in the wrong format? Are tool calls failing due to API errors or permission issues? These basic problems often account for large fractions of apparent failures.
2. **Verify scoring mechanisms.** Do grading functions actually measure what you intend? Are there edge cases where correct solutions get marked as failures? Manual review of a sample of "failed" attempts often reveals grading bugs that inflate failure rates.
3. **Examine tool usage.** Are you providing the right tools? Too many tools can confuse models and hurt performance. Are tool descriptions clear enough for the model to understand how to use them effectively?

Only after addressing these basics should you experiment with advanced scaffolding techniques. Multi-agent systems, sophisticated context management, and complex retry mechanisms add substantial complexity and new failure modes. They're worth exploring only after you've maximized performance with simpler approaches.

<div class="example-box" markdown="1">
**Context Management** <br>
Long agent tasks can hit context window limits or suffer from "context rot" where important information gets buried in hundreds of turns of conversation. Some scaffolds address this by summarizing old context or maintaining separate memory systems. But first check if the task actually needs to be that long or if better prompting could reduce the required context.
</div>


### Inference Scaling

Performance on agent tasks can improve linearly across orders of magnitude of computational budget. Models that seem incapable at 10,000 tokens may succeed at 1 million or 50 million tokens if the task structure supports this scaling.

Recent research on cybersecurity tasks shows this effect clearly. Across 100 capture-the-flag challenges, success rates improved linearly from less than 20% at 10K tokens to over 65% at 50 million tokens. This relationship held across multiple orders of magnitude and appeared consistent across different models.

The linearity is striking and suggests that for many tasks, computational budget is the primary constraint on performance rather than fundamental capability limitations. Models can make progress through extended exploration and iteration if given sufficient budget.

<div class="example-box" markdown="1">
**Cyber Task Scaling** <br>
The UK AI Security Institute tested models on cybersecurity challenges with token budgets ranging from 10,000 to 50 million. Performance improved linearly on a log scale across this entire range. Models that appeared to lack cybersecurity capabilities at standard budget levels achieved substantial success rates when given 100x more compute.
</div>

Inference scaling works best when tasks provide good local feedback that helps agents self-verify and self-correct. Software engineering tasks with unit tests, mathematical problems with checkable solutions, and capture-the-flag challenges with clear success criteria all tend to benefit from scaling.

Tasks with weak feedback signals may not benefit from additional compute. If agents can't tell whether they're making progress, extra turns may just lead to random exploration rather than systematic improvement.

This has important implications for capability assessment and safety evaluation. If you're trying to determine whether a model can perform certain tasks, testing with insufficient compute budget may lead to false negatives. The model might be capable but need more time to demonstrate that capability.

### The Measurement Crisis

Current trends in agent capabilities suggest we're approaching a measurement crisis where traditional evaluation approaches will break down. Time horizons for tasks that models can complete are doubling every 3-4 months, potentially reaching 40+ hour human-equivalent tasks by the end of 2026.

This creates a fundamental problem for benchmark-based evaluation. If models can complete 40-hour tasks, you need 80-hour tasks to distinguish between different model capabilities. A few months later, you need 160-hour tasks. The exponential growth in required task difficulty may outpace our ability to create and validate new benchmarks.

Benchmark saturation is already happening. CoreBench, CyBench, and other recent benchmarks are approaching ceiling performance as models improve. The gap between different models is shrinking, making it harder to measure relative capabilities.

One possible direction is toward fewer, much harder evaluation tasks that resemble real-world projects rather than isolated problems. Instead of hundreds of small tasks, evaluations might involve a handful of month-long projects with complex, multi-dimensional success criteria.

<div class="example-box" markdown="1">
**The Time Horizon Problem** <br>
METR's analysis shows that model capabilities on their task suite have been doubling every 7 months historically, with evidence of acceleration to every 3-4 months recently. Claude Opus 3.5 achieved around a 12-hour time horizon. If this trend continues, models might reach 40+ hour capabilities by late 2026, requiring evaluation tasks that would take humans weeks to complete.
</div>

This shift would require new methodologies for extracting signal from small numbers of very expensive evaluation runs. Instead of averaging over many attempts, evaluations might measure performance surfaces across different task configurations or focus on process quality rather than just final outcomes.

### Conclusion

Capability assessment requires much more care and transparency than traditional AI evaluation. When reading papers or model cards, ask for complete protocol specifications. What were the exact inference settings? Which scaffold was used? How was grading implemented? Without this information, reported scores are nearly meaningless for comparison or replication.

For safety-critical applications, consider that models may be more capable than standard evaluations suggest. If you're setting capability thresholds for governance or safety measures, test with sufficient computational budgets to avoid false negatives that could compromise safety decisions.

Transcript analysis should become standard practice for high-stakes evaluations. Scores alone cannot distinguish between capability limitations and setup problems. Understanding what actually happened during task execution is essential for making sound inferences about model behavior.

Finally, the measurement crisis suggests that evaluation methods themselves need rapid innovation. Traditional benchmark-based approaches may not scale to increasingly capable systems. New methodologies for assessing very capable agents will become increasingly important for maintaining oversight and understanding of AI systems.

The goal isn't perfect evaluation but predictable, interpretable assessment that fails gracefully and provides actionable insights for improvement. Current agent evaluation approaches provide a foundation for this understanding, but they represent early steps in what will need to be a rapidly evolving field.
