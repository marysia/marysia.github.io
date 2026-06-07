---
layout: note
title: "Communication to Policymakers and the Public"
course: "AI Evaluation: Capabilities & Safety"
course_id: eval
module: "10"
module_title: "Governance, Policy and Regulation"
lecturer: "Stuart Elliott"
lecturer_affiliation: "OECD"
date: 2026-04-22
---


## Overview

Technical evaluations of AI are only useful in public life if they translate into something non-experts can act on. This lecture frames AI evaluation as a communication problem (how to provide policymakers, educators, and the public with a small number of legible indicators of AI progress) and walks through the OECD's nine-capability scale as one working answer.

This lecture was taught by Stuart Elliott from the OECD, where he leads the AI and the Future of Skills project.

---

## Key Takeaways

1. Mature policy fields rely on a small, stable set of headline indicators (output, unemployment, and inflation in economics, PISA scores and graduation rates in education, life expectancy and death rates in health) that the public broadly understands and that map cleanly to policy goals. AI currently has no equivalent and needs one.
2. Useful public indicators share four properties: they are few in number, easy to summarise in a phrase, tied to recognised policy goals, and underwritten by technical machinery the public does not have to see.
3. Comparing AI to *human capabilities* gives the public a familiar yardstick. The OECD frames AI progress along nine capability dimensions covering both cognitive abilities (language, problem-solving, creativity, social interaction) and physical ones (vision, manipulation, robotic intelligence).
4. Each capability is rated on a five-level scale where level five corresponds to full human equivalence (effectively AGI in that capability), the middle levels represent the current research frontier, and the lowest levels are problems considered solved.
5. The most direct downstream use of the scales is mapping AI capabilities onto the demands of specific occupations (done first by human raters as a ground truth, then extended by AI raters across roughly 800 occupational titles) to support foresight conversations about work and education.

**Concepts**
- **Headline indicator**: A small, public-facing statistic that summarises the state of a domain (e.g. inflation, life expectancy) and supports policy debate.
- **Capability scale**: A graded description of what an AI system can do along a single dimension, anchored to recognisable performance levels.
- **AGI-equivalent level**: The top of each capability scale, defined as performance fully matching the relevant human capability.
- **Foresight exercise**: Reasoning about the skill landscape a decade out so that curricula and policy can be designed for the world students will graduate into, not today's.
- **Expert-judgement evaluation**: Rating AI by surveying domain specialists. High in validity but infeasible at scale.
- **Benchmark-based evaluation**: Rating AI by performance on standardised tests. Well-suited to the frontier but silent on capabilities AI cannot yet exhibit.
- **Literature-synthesis evaluation**: Categorising the published research itself against a scale to locate where work is dense, sparse, or absent.
- **Levels-of-automation framing**: A separate scale (familiar from autonomous driving) describing how humans and an AI system divide responsibility on a given task, distinct from the system's raw capability.

---

## Detailed Notes

### The communication problem

A great deal of AI evaluation work is produced for an audience of other AI researchers. The problem is that the people who most need to act on what evaluations show (legislators, regulators, educators, employers, voters) cannot read those evaluations directly. The current public conversation about AI gravitates toward a familiar set of issues: bias, hallucination, privacy, intellectual property, occasional mentions of catastrophic risk, and growing concern about job losses. Whether that set is the right one is itself a question. A serious evaluation programme has to decide which conversations it wants to support.

Other policy domains offer a useful template. Economics communicates through three headline indicators: output (the size of the economy), unemployment (whether people can work), and inflation (whether prices are rising). Each is summarisable in a single phrase, and each maps to a clear policy direction: more output, less unemployment, less inflation. Education does the same with learning levels (PISA), graduation rates, and years of schooling. Health relies on life expectancy, birth rates, and death rates.

These regimes share a structure. A small number of indicators sit on the public face. Beneath them lies a large body of technical work (definitional disputes, such as how economists have argued about measuring inflation for decades, data collection, periodic methodological revisions) that the public never sees. The indicators are updated on a regular cadence so that direction of travel is visible. They are accessible without being shallow.

AI has none of this yet. Building it requires deciding which capabilities matter, which scales to construct, how to evaluate against them, and how to keep the system current as the underlying technology changes.

### The OECD nine-capability framework

The OECD's AI and the Future of Skills project takes one concrete pass at the problem. It frames AI progress against the full range of *human* capabilities, on the reasoning that human capabilities are something the public already has intuitions about. The framework identifies nine capabilities and treats them as roughly covering what people can do.

The nine are organised loosely into cognitive capabilities typically developed in formal education (language, literacy, numeracy, problem-solving, creativity, metacognition, social interaction) and physical capabilities developed largely outside it (vision and manipulation, and an integrated robotic-intelligence dimension). The claim is not that these nine partition human ability cleanly, but that together they give a recognisable picture of what an AI system would need to do to substitute for, or augment, a human across the tasks humans typically perform.

**Including the sensitive cases on purpose.** Creativity and social interaction are on the list deliberately. Public discussion often asserts that AI simply lacks creativity or genuine social engagement. The scales reject that framing and ask instead at what level a given system operates. A consciousness scale was drafted as well but not released. External reviewers reported that the draft was not yet technically right, and that publishing a public scale of AI consciousness carried real policy risk, most concretely that attributing consciousness could become a rhetorical device deployed to shift accountability away from the organisations building the systems. The draft and the three peer reviews are available in the technical report. The scale itself was withheld.

### Designing a five-level scale

Each capability is rated on a five-level scale, where level one represents solved problems (the kind of thing taught in a first graduate course in the relevant subfield), the middle levels represent the current research frontier, and level five represents full human equivalence on that capability.

The five-level structure was chosen for communication. A reader can say "that's at level three" without understanding the underlying detail, and the ordering immediately conveys that level four entails everything at level three plus more. Anyone who wants the detail can read the level descriptors, which aim to capture qualitative differences: not "more language ability" but specific kinds of nuance that emerge between levels.

<div class="example-box" markdown="1">
**The language scale, as of roughly a year before this lecture, places current AI at level three.** Whether the right placement is now level four is an open question being worked through in the next update. The point of the placement is not precision but to give a non-specialist reader a defensible answer to "where is AI now on language?" and to make movement visible when the next update lands.
</div>

Building the scales was harder than the final product makes it look. AI researchers, asked to lay out a five-level progression, tended to push back: that is not how they naturally describe progress in their fields. What they *do* carry, implicitly, is a three-level distinction (solved problems, current research, things too hard to work on, i.e. the things they would tell a graduate student not to attempt). Starting from that implicit three-level structure and stretching it produced scales the researchers could endorse. The lesson generalises: when you ask domain experts for a public-facing scale, start from the categorical distinctions they already use.

### Evaluation methodology

Three approaches were considered for placing AI on each scale.

**Expert judgement with human tests.** This is the most direct route to a human-comparable rating and was the project's initial plan. It proved infeasible: there are not enough willing experts to cover enough tests to give adequate scale coverage, and the logistical cost is prohibitive on a recurring basis.

**Existing AI benchmarks.** A natural fallback, but benchmarks cluster where AI is competitive. The levels where AI cannot yet perform (precisely the levels that matter most for a public scale) are sparsely covered by benchmarks, because no one builds a benchmark to measure something nothing can do.

**Literature synthesis, partially automated.** The current approach. Research papers are categorised against the scale levels and the density of work at each level is examined. Where work is plentiful and broad, capability is plausibly present. Where work is sparse or narrow, it is not. This treats the field's own research distribution as evidence of where capability actually sits. It is imperfect but tractable, and it updates naturally as new work appears.

### Linking capabilities to occupations

A scale that nobody uses is just a document. The most developed downstream application is occupation mapping. The nine capability scores are translated into capability *demands* for specific jobs.

The translation runs in steps. Project staff first rate a sample of occupations by hand, establishing a ground truth. AI systems are then asked to perform the same ratings, and their output is compared against the human ratings to validate the procedure. The validated AI rater is then extended across the full set of roughly 800 occupational titles. The eventual aim is to add feedback from workers and supervisors in those jobs as a further validation layer.

The result is a table of capability demands by occupational group: how much language, how much problem-solving, how much manipulation, how much social interaction each kind of job actually requires, on the same five-level scale used to describe AI. This makes the capability indicators directly usable in conversations about which jobs AI is likely to transform, how, and on what timescale.

The framing extends to education. Schools today are preparing students who will graduate in five, ten, or fifteen years. The world they enter will not be today's world. If a capability is on the verge of being substitutable by AI, the curricular case for teaching it intensively weakens. If a capability is durably out of reach, or is the kind needed to oversee AI systems, the case strengthens. The scales provide a vocabulary for that foresight conversation that does not require every educator to also be an AI researcher.

### What the scales do not cover

A capability map is one slice of what the public needs. Several things sit outside it deliberately and matter regardless.

*Pervasiveness.* Capability says what a system *can* do. Pervasiveness says how widely deployed it already is. Policy made on the cusp of a technology becoming ubiquitous looks very different from policy written after the fact. A public indicator regime should track adoption as well as capability.

*Risk and safety.* The capability scales were not designed as a risk taxonomy, but they touch risk in two ways. The highest levels of some scales (problem-solving, robotic intelligence) make social and ethical concerns intrinsic to the capability itself, in the sense that a system reaching level five must handle these for the rating to be defensible. And safety problems often arise from *combinations* of capabilities rather than any single one, so a multi-dimensional capability picture is a reasonable input to a separate risk analysis.

*Product-level evaluation.* Indicators of where the frontier sits are distinct from evaluations of specific deployed products. Both matter. A school district adopting an ed-tech platform needs the second, not the first.

*Norms and public participation.* Treating the public only as data sources or as people to be protected misses a third role: shaping the norms, incentives, and boundaries inside which AI is built. Signals of public response (campaigns to pollute training data, refusal to engage, demands for transparency) are themselves information policymakers need.

*Reliability and epistemic humility.* Public communication should preserve the uncertainty that the underlying evaluations carry. A sample of 10 in a study and a sample of 2000 are not the same evidence. Sound-bite reporting flattens the difference. Indicators that travel into press releases need to carry their methodology with them in a form that survives the journey.

### Capability vs. human–machine relationship

A separate kind of scale, familiar from autonomous driving, describes not the system but the *relationship* between the system and the humans working with it: at low levels the human drives and the system assists, at intermediate levels supervision shifts, and at the top the system operates without human involvement. This is not a substitute for a capability scale, but a complement to it. The same underlying capability can be deployed under very different human-oversight arrangements, with very different policy implications. A mature indicator regime probably needs both: one set of scales for what the technology can do, another for how humans and the technology divide the work on a given task.

---

## Conclusion

The headline lesson is that evaluation is not finished when a number is produced. It is finished when the number is in a form that a non-expert can use to make a decision. That is a different craft, with its own constraints: small enough to remember, simple enough to summarise, anchored to something familiar, durable enough to be updated on a cadence, and honest enough to carry its uncertainty into the public conversation.

The OECD's nine-capability, five-level framework is one working attempt. It deliberately compares AI to human ability, refuses to write off contested capabilities like creativity, and links its scales to the domain of work, where policy demand is most concrete. It does not cover pervasiveness, deployed-product quality, public response, or risk in any direct way, and a complete public indicator regime would need to add those.

If you are building evaluations that you hope will influence policy, the practical implication is to design the public face first. Decide what conversations the indicator is meant to support, who needs to use it, what comparison makes it legible, and what cadence keeps it alive. The technical work then has a target, and the indicator has a chance of being more than a document.
