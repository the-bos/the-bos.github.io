---
layout: post
title:  "The 3T Framework for Agent Evals: Text, Tools, and Truth"
date:   2025-06-06 8:00:00 -0600
---

> **TL;DR**: In this post, I discuss how the 3 T's -- Text, Tools, and Truth -- comprise a powerful core for evals.
I distinguish between offline and online evals, and argue for doing both.




## "Agent evals are your most precious IP."

This was claimed on center stage at this year's LangGraph Interrupt conference said this, inducing a loud murmur in the crowd.

After a solid first morning covering agent architectures and conceputalizing the Agent Engineer as an emergent role, the entire afternoon was dedicated to instilling, no, indoctrinating the following idea into everyone's brains:

#### You NEED to do evals, and you NEED to do them well.

I think this is was something all of us kind of knew, to various extents, but didn't really _feel_ in our _bones_ until then.

And I think it's because, frankly, evals seem _hard_, at least to do well.

But... are evals really as critical as everyone seems to be insisting?


## The hard truth: Anyone can build an AI agent.

Indeed, there are a wealth of resources, learning content, and open source tooling to help you stand up your own agent surprisingly fast.

#### But it's a LOT harder to build a good agent.


Well, what is a good agent?

Is it magic?

No. It's hard work.

And I'm here to echo the idea that a lot of hard work is in your evals.

Sure, it's possible to rely on subjective evaluation to ensure your agent is performing well, but this requires either a dedicated team reviewing agent output on a weekly basis, or a gift of really, really strong intuition.


## The good news: YOU can build a good agent!

I'm not here to say evals are easy.

But I do think it's easy to get confused in all of the commotion surrounding them.

However, I'm excited to discuss a formulation that has helped me in my day-to-day, and just might help you as well.

## Case study: The `bugtriager` agent


Imagine you’ve just shipped an AI agent that helps triage bug reports. It lives in Slack, reads messages from engineers and QA testers, and responds with diagnostic suggestions — maybe even proposed fixes or links to logs.

You’ve designed it, architected it, tested it in dev and staging. Maybe it parses stack traces, queries logs, or hits internal APIs. Maybe it’s even starting to sound helpful.

But now your team is asking:

<blockquote style="font-style: normal;">
"How do we know it <em>works</em>?"
</blockquote>

And a more unsettling voice creeps in from your own head:

<blockquote style="font-style: normal;">
  "Wait... how do <em>I</em> know it works??"
</blockquote>


You’ve seen a few promising replies, and a couple weird ones. You’ve read the logs. But you need more than anecdotes. You need structure.

That’s where the 3T Framework comes in:
Text, Tools, and Truth.
A lens to evaluate not just whether your agent said something plausible — but whether it understood the user, used the right tools, and aligned with the truth of the system.

## The 3 T's of Good Evals: Text, Tools, and Truth

...

<!-- Tool call: intentional bug using proprietary class, ensuring the tool call hits the class tool, or logs, or something-->


<!-- Truth as a flexible "unsupervised" tool for online evals, etc.-->

<!-- Truth as a robustness tool to changing tool names, etc.-->


