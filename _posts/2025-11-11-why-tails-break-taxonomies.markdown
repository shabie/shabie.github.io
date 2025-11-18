---
layout: post
title:  "How tails break taxonomies"
date:   2025-11-11 00:11:00 +0200
---

There's an iron law of categorization: categories only stabilize where data is dense. Everywhere else, they fracture into opinion. This is why ten thousand IT tickets classify themselves, but five security incidents spark taxonomy wars. If you're designing LLM schemas without accounting for this law, you're building for the wrong end of the distribution.

I've been there. I've categorized both incident postmortems and IT helpdesk tickets, and the contrast is striking. IT tickets - password resets, VPN issues, software access, hardware issues - formed clean clusters immediately. Incident postmortems? Everyone championed a different taxonomy, and none of them stuck. Same task, wildly different outcomes.

Eleanor Rosch explained why. She argued that categories exist to "reduce the infinite differences among stimuli to behaviorally and cognitively usable proportions." This compression renders the unquantifiable quantifiable and lets us count, analyze, and act. But here's the problem: the usefulness of a category scales directly with how many instances you have in that category, and the fewer instances you capture, the more your categories fragment into uselessness.

The problem isn't rarity per se, but **insufficient instances to identify stable patterns**. Complex phenomena, those that resist clean discretization, tend to be rare, which compounds the difficulty. Categorization accentuates similarities within a group while suppressing differences, but with few instances, we can't tell which similarities matter. A multitude of plausible categories make equally good sense, and frequency isn't there to discipline the boundaries.

Let's take an example: an e-commerce site's customer service queries. They're easy to classify; billing issues, returns, delivery delays. Not because the categories are perfectly crisp, but because the events they capture are common. Thousands of queries create dense clusters with clear centers of gravity. Edge cases remain, but they drown in a sea of exemplars.

Now contrast this with Joseph Tainter's analysis in _The Collapse of Complex Societies_. He spends chapters wrestling with how to categorize Rome's fall. Military? Economic? Ecological? The possible explanations amount to many plausible frameworks, but the event itself is singular (n=1 for the Roman Empire) and incompletely documented. Each lens captures a fragment of truth, but without frequency to discipline boundaries, every category competes to be "the" explanation. For genuinely rare and complex phenomena like this, perfect categorization is impossible. One could argue this keeps some disciplines funded; scholars can endlessly reinterpret singular events when frequency won't settle the question.

Yet here's the deeper trap: rarity isn't always intrinsic to what you're categorizing, **it can be a function of your taxonomy's depth**. Take those e-commerce queries and add a fifth level to your decision tree: "billing issues > subscription services > Amazon Prime > after free trial expiration > duplicate charges." Suddenly your categories are nearly as empty as Tainter's. Each added dimension slices your sample size until you're back in contested territory, arguing over whether query #47,281 belongs in bucket "duplicate charges" or "wrongly charged."

This is exactly why Hamel and Shreya, in their LLM evals [course](https://maven.com/parlance-labs/evals), advise focusing on the "dominant issue" in the error trace annotation. They're asking you to avoid the curse of the long tail. **Overspecificity is taxonomy's silent failure mode: when every instance gets its own bucket, you haven't categorized, you've just renamed the data.**

For incident response, this explains the perpetual taxonomy debates. Major incidents live on the tail of the distribution so each incident feels unique, involving novel combinations of factors. Some teams "escape" to tagging instead of mutually-exclusive categories, but this is like swapping SQL for NoSQL: writes become easier, reads become harder. If done without discipline, tagging defers the categorization problem to analysis time, where making sense of 47 overlapping tags is its own nightmare.

The solution isn't perfect categories; it's honest ones. Use coarse-grained buckets where the data is sparse, fine-grained ones where it's dense. For AI applications churning out structured outputs, this means schemas should be designed for the head of the distribution, not the tail. You should stop fixating on edge-case labels that will always be debated.

If you find yourself spending more time arguing about category boundaries than using the categories, you're already too deep in the tail.