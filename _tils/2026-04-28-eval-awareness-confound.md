---
published: false
layout: til
title: "\"Eval awareness\" is a confound"
date: 2026-04-28
tag: Evals
---

If a model can tell it's being evaluated, your numbers measure behaviour-under-observation, not behaviour. Worth checking whether prompts leak that they're a test — framing, suspiciously round task counts, telltale system prompts all give it away.
