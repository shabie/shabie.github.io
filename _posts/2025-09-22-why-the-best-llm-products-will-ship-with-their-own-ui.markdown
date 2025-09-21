---
layout: post
title:  "Why the best LLM products will ship with their own UI"
date:   2025-09-22 01:10:00 +0200
---

We're pretty good at giving feedback to traditional software. When something breaks, we know exactly what to do: file a GitHub issue, ping the team on Slack, leave a detailed bug report explaining exactly what went wrong and how to reproduce it. The whole system works because it assumes we can articulate precisely what "wrong" means.

But what happens when that assumption breaks down?

Think about a frustrating experience with a barista. Maybe they weren't exactly rude, but something felt... off. You didn't file a complaint; you just left a smaller tip or tried a different coffee shop next time.

We don't send GitHub issues to baristas. The feedback is behavioral and loaded with nuance. And here's the thing: that indirect feedback often contains the most valuable information about what needs to improve.

LLMs increasingly handle requests in this murky territory: "What commonalities do you see in these outage reports?" "Summarize the discussion from the quarterly sales review" or "Help me prioritize these competing project requirements." These aren't problems with objectively correct solutions but judgment calls that depend on context and implicit knowledge.

When an LLM handles these requests, how do you know if an unsatisfying answer reflects the system's limitations or the inherent difficulty of the problem? The feedback becomes ambiguous.

Users reveal satisfaction through behavior, even when they can't articulate it:

- Regenerating the same response multiple times in a row
- Rephrasing the question after getting an answer
- Scrolling through a response in 400ms—skim-and-abandon
- Not clicking the links provided in the answer
- Interrupting the streamed model response

Each behavior is an implicit quality signal that never makes it into explicit feedback.

If you only build an API, you lose all these behavioral signals. An API call can't tell you someone abandoned the response halfway through or spent ten minutes rephrasing their question.

A UI captures this behavioral feedback _and_ can even actively guide usage: placeholder text that teaches better prompting, warnings when the model ventures into speculation, progressive disclosure that discourages wall-of-text queries. None of this travels through JSON.

## A Cultural Blind Spot

Many backend engineers instinctively resist building UIs, viewing them as "just presentation layer" that can be handled downstream. This API-first mindset served us well in traditional software, where the core logic lived in services and interfaces were largely interchangeable.

But with LLMs, this separation breaks down. The UI _is_ your data collection layer for the behavioral signals that drive model improvement. Teams that default to "build a great API and let others handle the frontend" may inadvertently cede their most valuable feedback loop to whoever controls the user experience.

## The Implication

I strongly suspect we'll see more vertical integration in the best LLM applications. Not because companies don't want to enable third-party developers, but because throwing away the behavioral feedback loop means throwing away a key source of competitive advantage.

[IDEs](https://devops.com/openai-acquires-windsurf-for-3-billion/) and [browsers](https://www.reddit.com/r/artificial/comments/1lwptpc/comment/n2gdhzb/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button) aren't "wrappers"; they're the two surfaces where **behavior itself is the ground truth**, and that's why the winners are racing to own them. The corollary for specialized LLM applications: the UI layer can't be an afterthought.

After all, if you're building something that lives in the land of "it depends," you need feedback that's just as nuanced as the problems you're trying to solve.