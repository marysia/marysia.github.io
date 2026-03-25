---
title: "Module 4: Evaluating Multi-Agent AI Systems"
date: 2026-02-27
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

When AI systems interact with each other and with humans, evaluation becomes much more complex than testing individual models. Multi-agent evaluation requires measuring how well systems generalize to unfamiliar partners and situations, not just familiar training scenarios.

This lecture was taught by Joel Leibo from Google DeepMind.

---

## Key Takeaways

1. **Always measure generalization**, also when evaluating multi-agent systems.
2. **Focus on mixed-motivation scenarios**. Mixed-motivation scenarios (where agents have partially conflicting goals) are more interesting than pure cooperation or pure competition, as they reveal emergent social behaviors.
3. **Environment structure matters** Large worlds (where agents must explore unknown situations) create different strategic dynamics than small worlds (where all possibilities are known upfront).
4. **Separate social and physical generalization** Social environment generalization (interacting with unfamiliar agents) should be tested separately from physical environment generalization (new tasks or settings).
5. **Language models create new evaluation challenges** When agents can recognize classic problems ("Hey, this is Prisoner's Dilemma"), traditional evaluation approaches break down. New methodologies are needed to test genuine strategic behavior rather than learned responses to textbook scenarios.


## Core Concepts

- **Small worlds**: All possibilities known upfront, like traditional game theory
- **Large worlds**: Unknown unknowns requiring exploration
- **Nash equilibrium**: Strategy where no player wants to change unilaterally  
- **Rock-paper-scissors dynamics**: Cyclical strategies where A beats B beats C beats A
- **Tragedy of commons**: Individual rational behavior creating collective bad outcomes
- **Equilibrium selection**: Choosing between multiple stable solutions
- **Population-based evaluation**: Testing algorithms that produce groups of agents
- **Background populations**: Pre-trained, frozen agent groups for standardized testing
- **Melting Pot**: Evaluation framework focusing on social environment generalization
- **Concordia**: Language model framework simulating collaborative storytelling

---

## Detailed Notes

### Generalization in Multi-Agent Evaluation

Multi-agent systems create unique evaluation challenges that don't exist for single agents. When agents interact, their behavior depends not just on the task but on who they're interacting with and how those partners behave. This creates a moving target for evaluation.

This means generalization becomes much more complex. A single-agent system needs to generalize to new inputs or tasks. A multi-agent system needs to generalize to new inputs or tasks, *as well as* to new partners, new group dynamics, and new social situations. 

Measuring generalization is surprisingly difficult to implement for multi-agent systems. Early work often evaluated agents only against the partners they trained with, which is like training and testing on the same data in supervised learning. The solution requires separating training partners from test partners. But unlike static test data, test partners are dynamic and strategic, creating additional complexity.

### Game Theory & Machine Learning

Multi-agent evaluation sits at the intersection of two different ways of thinking about intelligence and behavior.

**Game Theory Perspective**
Game theorists think in terms of strategies, equilibria, and rationality. They ask questions like "What should a rational agent do in this situation?" and "Where will the system settle if everyone acts optimally?" This provides powerful insights into strategic dynamics but assumes agents can compute optimal strategies.

**Machine Learning Perspective**  
Machine learning focuses on learning from data, generalization, and performance on unseen examples. It asks "How well does this algorithm perform on new data?" and "What can we infer from training behavior?" This provides practical learning algorithms but doesn't capture strategic interactions.

Multi-agent evaluation needs both perspectives. Game theory helps understand what kinds of strategic dynamics might emerge. Machine learning provides the tools to measure generalization and avoid overfitting to specific scenarios.

### Small Worlds vs Large Worlds

This distinction, originally from decision theory, helps clarify different types of multi-agent scenarios and their evaluation challenges.

**Small Worlds**
In small worlds, all possible states and actions are known upfront. Classic game theory operates in small worlds. The Prisoner's Dilemma is a small world as there are exactly two actions (cooperate or defect) and four possible outcomes. You can analyze the entire game mathematically.

**Large Worlds**
In large worlds, agents face unknown unknowns. They must explore to discover what's possible. Most real-world scenarios are large worlds. Even something as structured as StarCraft is a large world because the space of possible strategies is too large to enumerate.

Large worlds create different strategic dynamics. Agents can't compute optimal strategies because they don't know all the possibilities. They must balance exploration (trying new things) with exploitation (using what they've learned). This creates opportunities for emergent behavior that doesn't exist in small worlds.

**Evaluation Implications**
Small world evaluation can rely on theoretical analysis and exhaustive testing. Large world evaluation requires empirical methods and statistical testing. Most interesting multi-agent systems operate in large worlds, so evaluation methods must handle this complexity.

### Common Harvest Environment
The classic Prisoner's Dilemma illustrates how individual rationality can lead to collectively bad outcomes. But seeing this principle in action in a large world environment reveals additional complexities. Multi-agent evaluation can't just test abstract strategic choices. The physical environment shapes which strategies are possible and which ones emerge from learning. Evaluation frameworks need to test across different environment structures to understand the full range of possible behaviors.

<div class="example-box" markdown="1">
**Commons Harvest Environment** <br>
Agents move around a world collecting apples. Apples grow back slowly if you leave some in each patch, but disappear forever if you harvest everything. The sustainable strategy is to harvest partially and wait for regrowth. The greedy strategy is to harvest everything immediately.

When agents train with reinforcement learning, they learn the greedy strategy. They rush to harvest everything because they're worried other agents will get there first. The result is resource depletion and lower rewards for everyone. The agents never explicitly choose to be greedy, they just learn to move quickly towards resources.

The structure of the environment matters. When you add walls with chokepoints, some agents learn to control territory by blocking narrow passages. This allows them to become become sustainable because they know no one else can access their resources. The underlying resource dynamics are identical, but the spatial structure enables different strategic solutions.
</div>


### Rock-Paper-Scissors Dynamics

Many multi-agent scenarios have cyclical strategy relationships where strategy A beats strategy B, strategy B beats strategy C, and strategy C beats strategy A. 

This creates a major challenge for evaluation. There's no single "best" strategy as performance depends on what opponents are doing. A strategy that dominates one opponent might lose badly to another. The solution is maintaining diversity in evaluation opponents. Instead of testing against a single opponent type, test against a mixture that includes all major strategy types. This prevents exploitation of specific weaknesses while revealing genuine strategic capabilities.


<div class="example-box" markdown="1">
**Star Craft** <br>
In the real-time strategy game StarCraft, different unit compositions and build orders have rock-paper-scissors relationships. Fast, aggressive strategies beat slow, economic strategies. Economic strategies beat balanced military strategies. Balanced military strategies beat fast, aggressive strategies.

When AI systems learn to play StarCraft, they can get stuck cycling through these strategies. If the current best strategy is fast aggression, everyone learns to counter it with economic builds. Then everyone learns to counter economic builds with balanced military. Then everyone learns to counter balanced military with fast aggression. The cycle continues indefinitely.
</div>

### The Melting Pot Framework

Melting Pot is an evaluation framework that applies machine learning's generalization principles to multi-agent systems. It decomposes the generalization challenge into two parts: 
**Physical Environment**: Can your agents handle new tasks, new maps or otherwise new physical settings?
**Social Environment**: Can your agents work with unfamiliar partners, who have different strategies, goals or behaviors?

These two types of generalization can be tested separately. You can test social generalization by keeping the same task but changing the partners. You can test physical generalization by keeping the same partners but changing the task.

Melting Pot evaluates algorithms that produce populations of agents, not individual agents. This reflects the reality that most multi-agent systems involve multiple instances of the same algorithm interacting with each other and with other systems. The evaluation process trains a population using the candidate algorithm, then tests that population in various scenarios with different background populations. The background populations are pre-trained and frozen to ensure consistent evaluation across different submissions.

**Testing Modes**
*Resident mode* tests how well a population performs when most agents in the environment come from that same population. This tests internal coordination and group dynamics. *Visitor mode* tests how well individual agents from the population perform when immigrating into an environment dominated by agents from the background population. This tests adaptation to existing social norms and practices.

Important is that the evaluation designers are *separate from* the solution designers. The evaluation scenarios and background populations are created by different people than those developing the algorithms being tested.

### Concordia

The shift from reinforcement learning agents to language model agents opened up new possibilities for studying social phenomena, but also created new evaluation challenges. Earlier multi-agent work was limited to computer game environments where agents could only take simple actions like move, turn, or shoot. Language model agents can communicate in natural language, reason about social situations, and draw on knowledge about human culture and norms.

This enables studying much richer social phenomena like moral reasoning, cultural differences, and complex negotiations. But it also makes evaluation more difficult because the agents might recognize the scenarios they're being tested on.

The Concordia framework simulates collaborative storytelling protocols like tabletop role-playing games. Multiple language model agents play characters in a simulated world, while another language model serves as the game master, describing the environment and consequences of actions.

This creates an open-ended simulation where agents can take any action they can describe in language. The game master responds by describing what happens, creating a dynamic story that emerges from the agents' choices.
