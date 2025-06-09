---
layout: post
title:  "The 3T Framework for Agent Evals: Text, Tools, and Truth"
date:   2025-06-06 8:00:00 -0600
---

> **TL;DR**: In this post, I discuss how the 3 T's -- Text, Tools, and Truth -- comprise a powerful core for evals.
I distinguish between offline and online evals, make a case for why you should do do both, and demonstrate how the 3Ts have your back here.




## 🎤 "Agent evals are your most precious IP."

This line was dropped on center stage at this year's [LangChain Interrupt conference](https://interrupt.langchain.com), igniting a loud murmur in the crowd.

After a solid first morning covering agent architectures and conceptualizing the Agent Engineer as an emergent role, the entire afternoon was dedicated to instilling, no, indoctrinating the following idea into everyone's brains:

#### You NEED to do evals, and you NEED to do them well.

This was something we all kind of knew—but didn’t really _feel in our bones_ until that moment.”

And I think it's because, frankly, evals seem _hard_, at least to do well.

But... are evals really as critical as everyone seems to be insisting?


## The hard truth: Anyone can build an AI agent.

Indeed, there are a wealth of resources, learning content, and open source tooling to help you stand up your own agent surprisingly fast.

#### But it's a _lot_ harder to build a **good agent**.


Well, what is a good agent?

Is it magic?

No. It's hard work.

And I'm here to echo the idea that a lot of hard work is in your evals.

Sure, it's possible to rely on subjective evaluation to ensure your agent is performing well, but this requires either a dedicated team reviewing agent output on a weekly basis, or a gift of really, really strong intuition.


## 🌅 The good news: YOU can build a good agent!

I'm not here to say evals are easy.

But I do think it's easy to get confused in all of the commotion surrounding them.

However, I'm excited to discuss a formulation that has helped me in my day-to-day, and just might help you as well.

## 🕵️ Case study: The `bugtriager` agent


Imagine you’ve just shipped an AI agent that helps triage bug reports.
It lives in Slack, reads messages from engineers and QA testers, and responds with diagnostic suggestions — maybe even proposed fixes or links to logs.

You’ve designed it, architected it, tested it in dev and staging.

Maybe it parses stack traces, queries logs, or hits internal APIs. Maybe it’s even starting to sound helpful.

But now your team is asking:

<blockquote style="font-style: normal;">
"How do we know it <em>works</em>?"
</blockquote>

And a more unsettling voice creeps in from your own head:

<blockquote style="font-style: normal;">
  "Wait... how do <em>I</em> know it works??"
</blockquote>


You’ve seen a few promising replies, and a couple weird ones.
You’ve read the logs.
But you need more than anecdotes.

You need a structured view.

That’s where the 3T Framework comes in:
Text, Tools, and Truth.
A lens to evaluate not just whether your agent said something plausible — but whether it understood the user, used the right tools, and aligned with the truth of the system.

## 3️⃣ The 3 T's of Good Evals: Text, Tools, and Truth

### 📄 T1: Text

The big deal about AI agents is that they are a natural language interface between the user and the code.

The user asks it to do a thing, and it does that thing.

Sometimes that thing is a simple text answer, without even talking about tools, actions, or anything really fancy.

In our `bugtriager` example, let's say you have the following conversation with the agent:


```
User: "What sort of things can you help me with?"

AI Agent: I can help triage bug reports, whether they live in Slack, JIRA, you name it. I can look at logs, source code, and other sources of proprietary information to help you resolve bugs fast.
```

Great response!
Users will love this.

But, later that day, you push a change, and ask it the same question to double check everything is still good:

```
User: "What sort of things can you help me with?"

AI Agent: I'm an AI assistant here to help you with any questions or tasks you have! How may I assist today?
```

Wait a minute.

It didn't even _mention_ bugs, or triaging, or really anything that should distinguish it as a bugfixer, not a general LLM.


You decide to add an example in your evals that makes sure the agent responds to this particular query with the right sort of **keywords**.

In a nutshell, this might look like:

```
import numpy as np
from bugtraiger import agent, normalize_text

query = "What sort of things can you help me with?"
expected_text = ["triage", "bug reports", "Slack", "JIRA", "logs", "source code"]

agent_response = agent.run(prompt=query)

hits = [normalize(text) in normalize(agent_response) for text in expected_text)

coverage = np.mean(hits)
```

The key thing to note here is how cheap basic **text inclusion** is. 
No need for LLMs, complex processing, or anything beyond a simple list comprehension.

It's not perfect (the coverage for this example will drop from 100% to 83% if `logs` aren't mentioned, specifically), but it gives you a quick and easy guardrail against regressions.

And you can tune or weight things as needed to capture what is truly important, and what is a good to have.


#### BLUE and ROGUE

Note that there are quite a few existing text similarity metrics out there that you can plug and play into your text evaluators.

Often, they are tailored towards specific NLP tasks you might want your agent to handle.

For example, you can use BLEU for translation scoring, and ROGUE to assess summarization quality.


#### Semantic similarity

Exact text matching has the following drawbacks, among others:

- Not robust to typos
- Doesn't capture semantic variation
- Can't easily handle complex phrasing 

Thankfully, we have an amazing tool at our disposal: text embeddings!


Indeed, all you need is an **embedding model** (off-the-shelf models that you can commit to your repo directly have gotten, like, really good, but fine-tuning can always get you even more lift), and some extra validator logic, and you now have **semantic similarity** checks between the agent response and expected text outputs.
Let's go!



### 🛠️ T2: Tools

Tools are the what help **distinguish your AI agent** from a typical chatbot.

If the LLM is your agent's brain, tools are its _brawn_.

It goes without saying that you'll want to be very intentional about which tools your agent calls in various query scenarios.

Going back to our `bugtriager`:

You have your main app running at `main_app`, saving logs to a logfile on the fly.

Your agent has a tool to inspect these logs at runtime.

**At least, in theory.**

You intentionally send a malformed request to your `main_app` service, triggering a 422 error.

You go to test that your agent can figure all this out:

```
User: why am I getting a 422?

Assistant: Let me check...

Assistant: I'm sorry, but I'm not sure why you are getting a 422. I can't seem to access any traces from `main_app` at the moment.  Maybe you can share some logs we can check?
```

What the heck?

YOU should be checking the logs, `bugtriager`!

If I wanted to copy and paste logs, I'd just use ChatGPT or something.

```
User: don't you have a check_logs tool?

Assistant: You're right, I do. Let me try that...

AI Agent: Good news, I was able to find your 422 in the logs. It looks like you sent `fake-uuid` as the user ID which triggered a ValidationError...
```

Okay, so the agent has the tool, and can use the tool, but didn't seem like it was appropriate from my query.

Hmm...

You go to inspect the `check_logs` tool to see if anything looks strange.

```
@tool
def check_logs():
    """Test description - TODO: actually fill this out"""
	
    ...
```

Oh... there's the smoking gun!

Also, are we not even sending in the user query?
That seems like it would be useful context for the agent when searching through what could be very many logs, potentially with more errors than are relevant to what the user's asking about.

You make the following changes:

```
@tool
def check_logs(user_query: str):
    """
	Check the `main_app` logs for errors, warnings, or other critical information 
        given a user query.
	Useful if the user is hitting a particular HTTP error code.
    """
	
    ...
```

Cool!

Let's try this again in the UI...    


```
User: why am I getting a 422?

AI Agent: Let me check the logs for a 422 HTTP error...

AI Agent: Calling check_logs() tool...

AI Agent: Good news, I was able to find your 422 in the logs. It looks like you sent `fake-uuid` as the user ID which triggered a ValidationError...
```

Voila!

Who knew docstrings would be the bottleneck to good agent experience?


This is a simple example, but what about more complex scenarios where there are multiple factors, and hence multiple tool calls at play?


For example, maybe we want to introduce a `generate_fix` tool that outputs a code suggestion to fix a bug the user is seeing.

Hell, maybe you want another tool to file a GitHub issue, or even summon a separate AI agent to actually fix the bug and put out a PR.

This can be abstracted as a **tool call sequence**, something like:

```
[check_logs, generate_fix, create_github_issue, summon_bugfixer]
```

The key to note here is that we can get quite elaborate with the behavior you might want from your agent when it comes to tool calls.

And that's totally fine!
We just have to make sure this is captured in our evals.

Adding support for separate **tool call modes** will give you the flexibility you need to define diverse examples that provide a strong signal to agent performance.

Three "tool call modes" I've found personally useful:


1. **Exact match**: The agent must call every tool call you provide, in the exact sequence it was provided, and nothing more.
2. **Subsequence match**: The agent must call every tool you provide, in the sequence provided, but potentially with other tool calls sprinkled in.
3. **Subset match**: The agent must call at least one tool you provided, or a general subset of tools, regardless of sequence

But the cool thing here is that could define many other modes that are useful for your specific context.

Just make sure it's easy to surface the observed tool calls from production logic to your eval pipeline.

It's worth noting that captured actions aren't confined to tool calls, necessarily.
For example, maybe you want to ensure a sub-agent is correctly called and executed, or maybe you want to ensure an interrupt is raised before a particular tool is called.
These are great extensions of the spirit of the Second T (and maybe I'll think of a better T-starting category name for them, someday)!


**“But,"** you ask, **"just checking that tools were called isn’t enough. It _can't_ be enough. How do we know the answer the agent gave is actually true?”**


### 🎯 T3: Truth

While text and tool call evaluators are powerful, they also have blind spots — especially when correctness hinges on nuanced reasoning.


Let's say a QA tester reports a flaky test that sometimes fails with a `TimeoutError: Element not found` after 10 seconds.

They send you the stack trace, which points to a button that should appear after a modal loads.

You decide to ask the agent why the test is flaky, attaching the stack trace for reference.

The agent must:

- analyze the trace
- interpret the test logic
- identify the relevant source code
- recognize that the model is tied to an async API call
- realize the DOM may not be updated in time
- conclude: `add an explicit wait for the model to load, or mock the API call`


Indeed, the agent ultimately comes to this conclusion and reports back with the answer you wanted.

Awesome!



You realize this is a great example for evals — about as close to a holistic reasoning eval as it gets.

But... how should you specify it?

#### Would text inclusion work?

You _could_ check that it mentions `timeout`s or `flak(y|iness)`, but this won't guarantee that its solution will actually fix the test.


#### Would tool call checking work?

You _could_ ensure it calls a `source_code` tool, or something, but that isn't sufficient to verify the agent's **correctness**.

Or what I'm calling **truth**.


#### 🦾 Enter: LLM-as-a-Judge™

You decide to invoke a second LLM to evaluate the first agent’s reasoning and conclusions based on the original user query.

This works surprisingly well: you don’t need hand-crafted ground truth labels.

All you need is:

- the user question
- the agent's response
- an evaluation prompt

With the right template and scoring rubric, you've turned a tricky reasoning task into a scalable eval!

The cool thing is, you can apply this to all of your examples. The inputs couldn't be simpler: just your user queries and agent responses.

And you might find that your truth / correctness score tends to fall in between the text and tool scores.
(At least I have.)

I think this happens because text evaluators are very precise and easy to get working, whereas tool calls, while powerful, can be more difficult to excel at across the board.

Truth gives you a juicy middle that might be the most honest indicator of response quality: what's working well, and where your biggest areas of improvement are.



#### 💰 Bonus: Online evals!

Let's fast-forward a bit and say your agent is deployed in prod and serving real customer traffic!
Woohoo!

Your team is natuarally very curious how the agent is doing in the wild. 

It turns out that one of the biggest strengths of the truth evaluator is that it can run immediately after a live user query / agent response event (as a background event hidden to the actual conversation).

You just send this event, along with your same evaluation prompt, to your judging LLM, and that's it!
You suddenly have more eval examples and correctness labels, at the cost of an LLM call and a bit more work specifying your agent's topology.

Truth-based evals like this can run continuously, sampling real queries and auditing responses. No offline simulation needed—just real behavior, judged with real criteria.


<!-- Tool call: intentional bug using proprietary class, ensuring the tool call hits the class tool, or logs, or something-->


<!-- Truth as a flexible "unsupervised" tool for online evals, etc.-->

<!-- Truth as a robustness tool to changing tool names, etc.-->


<!-- A big reason truth works online is that it the LLM bottleneck doesn't matter: it's a post-hoc background call after the agent response has already been surfaced. No ETL spikes.-->


#### 📝 A note on truth

I want to address the elephant in the room here:

- I'm proposing you use one LLM, falliable as it may be, to output whether an answer from _another_ LLM is correct or not,
- without any labels, or human review, or intrepretability beyond our prompt and the LLM's street cred,
- and I _dare_ to call this _truth_?


This is a fair point, but more of a critique of LLM-as-a-Judge rather than the end goal of Truth.

Ultimately, we want a score of whether the agent _seems_ to have correctly responded to one or more queries.

We could rely on human judges for this (at least for offline evals), but even humans might have a difficult time determining these scores (and might even [disagree with each other](https://en.wikipedia.org/wiki/Inter-rater_reliability#Disagreement)).

Maybe the user had no idea what they _really_ were looking for, and just started with the first thing that came to mind.
What is the correct agent response even supposed to be here?

I'll conclude that LLM-as-a-Judge is an increasingly popular state-of-the-art evaluation paraidgm, producing generally robust metrics especially when aggregated, and leave it at that for now.




## ⚖️ Comparison

The 3Ts of evals form a triforce of utility, and work best as a complete set.

Below is a table of considerations for each group:

<div style="overflow-x: auto; margin: 2rem 0; -webkit-overflow-scrolling: touch;">
  <table style="min-width: 900px; width: 100%; table-layout: fixed; border-collapse: collapse;">
    <thead>
      <tr>
        <th style="width: 100px;">Evaluator</th>
        <th style="width: 160px;">What it does</th>
        <th style="width: 160px;">Typical checks</th>
        <th style="width: 290px;">Benefits</th>
        <th style="width: 290px;">Drawbacks</th>
      </tr>
    </thead>
  <tbody>
    <tr>
      <td><strong>Text</strong></td>
      <td>Does the agent respond clearly and appropriately in natural language?</td>
      <td>Keyword coverage, BLEU, ROUGE, embedding similarity</td>
      <td>
        ✅ Fast<br>
        ✅ Targeted<br>
        ✅ Easy to implement<br>
        ✅ Interpretable<br>
        ✅ Quickly verifiable<br>
        ✅ No API dependency<br>
        ✅ Easy to crowdsource<br>
        ✅ Helps shape agent personality / style
      </td>
      <td>
        ⚠️ Doesn't test reasoning<br>
        ⚠️ Keywords aren't robust to typos / semantic variants<br>
        ⚠️ Can be brittle to nomenclature changes<br>
        ⚠️ Not always applicable<br>
        ⚠️ Can only run offline
      </td>
    </tr>
    <tr>
      <td><strong>Tools</strong></td>
      <td>Does the agent use the right tools, with the right arguments, at the right time?</td>
      <td>Tool call sequence match, argument inspection</td>
      <td>
        ✅ Ensures tools are used as intended<br>
        ✅ Can test complex tool call flows<br>
        ✅ Captures reasoning structure<br>
        ✅ Good for regression detection<br>
        ✅ Encourages clean interfaces
      </td>
      <td>
        ⚠️ Requires sophisticated setup<br>
        ⚠️ Tool use ≠ good outcome<br>
        ⚠️ Can penalize valid alternate strategies<br>
        ⚠️ Only applicable if tools are required<br>
        ⚠️ Fragile to interface changes<br>
        ⚠️ Can only run offline
      </td>
    </tr>
    <tr>
      <td><strong>Truth</strong></td>
      <td>Is the final output aligned with the real state of the world or system?</td>
      <td>LLM-as-a-Judge, human annotations</td>
      <td>
        ✅ Holistic notion of agent correctness<br>
        ✅ Best proxy for good UX<br>
        ✅ No ground truth labels needed<br>
        ✅ Extensible beyond text<br>
        ✅ Flexible scoring (e.g. 1–5 scale)<br>
        ✅ Can run online
      </td>
      <td>
        ⚠️ Requires LLM call (API dependency)<br>
        ⚠️ Slower<br>
        ⚠️ More subjective / less interpretable<br>
        ⚠️ Ambiguity in gold standards<br>
        ⚠️ Harder to debug
      </td>
    </tr>
  </tbody>
</table>
</div>


The important thing here is that only one or two isn't quite enough.

My thesis here is that you need at least all three to have a sufficiently complete view of your agent's performance, especially if you want both offline and online evals.


## 🏁 Conclusion

Of course, there are a whole lot more to evals than I've covered here.

Getting a genuinely good example dataset, though seemingly straightforward on the surface, might be the biggest bottleneck of all.

You also will probably want many more evaluators beyond the 3 Ts, to really capture the context or domain that gives your agent and organization an upper hand.

And I didn't even _mention_ the trials and tribulations of doing actual, hardcore Eval-Driven Development.
(Though I might touch on this in a following post.)

But, if you can get to a place where your agent nails all of the 3T's on a rich and representative set of eval examples, it will be in great shape.

And your users will love it.

