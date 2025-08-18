---
layout: post
title:  "Agents are search over action space"
date:   2025-08-19 00:01:00 +0200
---

It's no secret that today's LLM-based agents are unreliable. This makes them a gamble for most critical tasks, so where can they be safely applied? The answer lies in finding asymmetry: we should use them in domains where the downside of a mistake is low, but the upside of success is huge; a strategy Nassim Taleb calls antifragile.

But if the consequences of an action are mostly upside, wouldn't simple, rule-based logic be a better tool to trigger those? Why bring in an LLM if things can't go too wrong?

Because, very often, the enormous upside isn't tied to the power of any single action. Instead, it comes from identifying the one or two actions that matter from a sea of irrelevant ones. The real cost isn't in pulling the wrong lever, but in the expensive, time-consuming search for the right one.

This model checks out in the domain of incident response. Say an incident can be caused by any change introduced in the last three hours. If we assume they're all equally easy to roll back, the bottleneck is clearly diagnosis, not action. The real value comes from an LLM agent that can analyze disparate signals to generate a shortlist of candidates that _often_ contains the offending change. This doesn't override human expertise; if an engineer has a better idea, they can ignore the suggestion. But in the absence of a clear lead, the agent provides a high-value head start, turning a wide-open search into a focused inquiry.

Similarly, consider sales lead generation. A salesperson might have a CRM with thousands of potential customers. The cost of sending a personalized email to a lead who isn't interested is negligible. However, the upside of connecting with the right lead at the right time is a huge deal that could make their entire year. The challenge is finding that one lead. An LLM agent can scan all the leads, analyze recent news, social media activity, and other signals to generate a daily shortlist of the 10 most promising contacts. The agent doesn't need to be perfect; it just needs to surface a handful of high-potential needles from a giant haystack, dramatically increasing the leverage of the human salesperson.

This also clarifies where agents _shouldn't_ be used. If you have very few options and each comes with a significant, irreversible cost, an agent is the wrong tool for the job. In those cases, the guardrails afforded by a pre-defined automation workflow are not a limitation; they are a necessary safeguard.

So, the recipe for success is clear: antifragile agents are a search over the action space that creates value when the action space is vast, the cost of exploration is low, and the primary challenge is discovery, not execution.