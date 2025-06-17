---
layout: post
title:  "The 3Ts of Agent Evals"
date:   2025-06-17 7:00:00 -0600
---

{% raw %}
<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
  mermaid.initialize({ startOnLoad: true });
</script>
{% endraw %}


> **TL;DR:** Agent evals are essential — yet too often vague and unstable in practice.
The 3T agent evaluation framework (Text, Tools, Truth) offers a concrete yet flexible foundation for evaluating agents, whether in offline tests or live monitoring.
This post shows how to evaluate agent responses, actions, and real-world correctness, with relatable examples that move you from guesswork to grounded, scalable evals and a UX your agents can be proud of.

## 🎤 "Agent evals are your most precious IP."

That line dropped like a lightning bolt at this year's [LangChain Interrupt conference](https://interrupt.langchain.com), sparking the loudest murmur of the day.

After a dense morning of diverse talks on AI architectures and the rise of the Agent Engineer™️, the afternoon zeroed in on a single, rallying idea:

#### You NEED to evaluate your agents. And you need to do it well.

We all kind of knew that already, sure.
But now, we felt it in our _bones_.
Not just as best practice, but as _survival instinct_ — the difference between shipping something magical and unleashing chaos to our users.


### 👷 Building a Good Agent™ is Hard

Anyone can spin up an AI agent in minutes.

But building a Good Agent™? One that’s dependable, explainable, and production-ready?

That takes more than vibes.
It takes honest, structured hard work.

This is where evals come in: not just as a gut-check, but as a principled means to simulate real use cases, make concrete improvements, and achieve confidence that your agent actually _slaps_.

And we really do want a _formal pipeline_ that feeds your agent realistic inputs and scores its responses and behavior.

I mean, sure -- it's _possible_ to crank out a decent agent based solely on ad-hoc, subjective evaluation, but unless you have a designated team of human annotators reviewing agent output on a weekly basis, this won't scale.


### 🌅 The good news: YOU can build a Good Agent™!

I'm not here to say evals are easy.

In fact, they're arguably the most difficult component of AI Engineering, which is already a premier technical challenge of our time.

Amid the hype, LinkedIn posts, and daily noise, it's easy to feel lost about how to do evals right, or what evals even _are_.

That’s why I developed a framework that **cuts through the noise** and focuses on **what actually matters**.

Let's take a look at what it looks like in action.

## 🕵️ Case study: The `bugtriager` agent


Hooray!

You’ve just shipped an AI agent that helps **triage bug reports**.

It lives in Slack, reads messages from engineers or QA testers, and responds with diagnostic suggestions tailored to your codebase.
It can even open full-fledged bugfix PRs at your command.

You’ve designed it, architected it, tested it in dev and staging.

It parses stack traces, queries logs, and hits internal APIs as you knew it could.

It _seems_ like it's helpful.

And now your team is asking:

<blockquote style="font-style: normal;">
Are we really sure it <em>actually works?</em>
</blockquote>

Worse — so are you.

You’ve seen good things.
Maybe a few weird replies.

But you need more than anecdotes.

You need a structured, clear view.

A lens to evaluate not just whether your agent said something plausible — but whether it provided **correct information** to the user, used the **right tools**, and aligned with the **truth of the system**.

You need **evals**, and they need to capture this structured, clear view with simplicity, capability, and success.


## 3️⃣ The 3 T's of Good Evals: Text, Tools, and Truth

<div style="display: flex; justify-content: center;">
  <svg viewBox="0 0 300 270" width="70%" xmlns="http://www.w3.org/2000/svg" style="color: currentColor; background: none;">
    <defs>
      <linearGradient id="aiGradient" x1="0%" y1="0%" x2="100%" y2="100%">
        <stop offset="0%" stop-color="#8B5CF6" />   <!-- Purple -->
        <stop offset="30%" stop-color="#06B6D4" />  <!-- Blue -->
        <stop offset="65%" stop-color="#D946EF" />  <!-- Pinkish Magenta -->
        <stop offset="100%" stop-color="#F97316" /> <!-- Orange -->
      </linearGradient>
    </defs>

    <!-- Triangle border -->
    <polygon
      points="150,40 40,230 260,230"
      fill="none"
      stroke="url(#aiGradient)"
      stroke-width="5"
    />

    <!-- Centered multi-line text -->
    <text text-anchor="middle" font-size="18" font-weight="500" fill="currentColor">
      <tspan x="150" y="130">Good</tspan>
      <tspan x="150" y="160">Agent™</tspan>
      <tspan x="150" y="190">Experience</tspan>
    </text>

    <!-- Corner labels -->
    <text x="150" y="25" text-anchor="middle" font-size="20" font-weight="600" fill="currentColor">
      📄 Text
    </text>
    <text x="40" y="255" text-anchor="middle" font-size="20" font-weight="600" fill="currentColor">
      🛠️ Tools
    </text>
    <text x="260" y="255" text-anchor="middle" font-size="20" font-weight="600" fill="currentColor">
      🎯 Truth
    </text>
  </svg>
</div>


The 3T framework focuses on 3 central pillars -- Text, Tools, and Truth -- to power your evals.

What do we mean by each of these?
At a high level, we have:

- 📄 T1: **Text**
  - Think of this as the **Interface** Layer, or what the agent _says_.
  - It asks: Does the agent respond with clear and accurate natural language?
- 🛠️ T2: **Tools**
  - Think of this as the **Action** Layer, or what the agent _does_.
  - It asks: Does the agent take the correct steps, call the right code, and interact with its environment properly?
- 🎯 T3: **Truth**
  - Think of this as the **Reasoning** Layer, or whether the agent is _right_.
  - It asks: Is the agent's ultimate output / response correct and aligned with reality?

Together, they form a comprehensive evaluation stack, ensuring you're not just checking one aspect of your agent, but its entire chain of operation.

And while our trifecta may appear simple at first, there's a lot to unfold here, so let's dig into these one by one.


### 📄 T1: Text, or What the Agent Says

The big deal about AI agents is that they are a **natural language interface** to your system's logic.

The user asks it to do a thing, and it does that thing.

Often that means responding with a simple, concise text answer that presents the requested or otherwise relevant information nicely to the user.

This is what I mean by getting the Text right.

It ensures your agent's "front door" is on-brand and builds initial user confidence.
This is often the first and most immediate form of evaluation.

In our `bugtriager` example, let's say you have the following conversation with the agent:


```
User: "What sort of things can you help me with?"

AI Agent: I can help triage bug reports, whether they live in Slack, JIRA, you name it. I can look at logs, source code, and other sources of proprietary information to help you resolve bugs fast.
```

Great response!
Users will love this.

But, later that day, after a few PR merges, you ask it the same question:

```
User: "What sort of things can you help me with?"

AI Agent: I'm an AI assistant here to help you with any questions or tasks you have! How may I assist today?
```

Wait a minute.

It didn't even _mention_ bugs, or triaging, or really anything that should distinguish it from any other run-of-the-mill AI.


You decide to add an example in your evals that makes sure the agent responds to this particular query with the right sort of **keywords**.

In terms of code, assuming you have a nice typed `AgentResponse` interface along the lines of:

```python
class AgentResponse:
    text: str
    tool_calls: list[str]
```

your Text evaluator might look like:

```python
import numpy as np
from bugtriager import agent, normalize, AgentResponse

query = "What sort of things can you help me with?"
expected_texts = ["triage", "bug reports", "Slack", "JIRA", "logs", "source code"]

agent_response: AgentResponse = agent.run(prompt=query)

hits = [normalize(text) in normalize(agent_response.text) for text in expected_texts]

coverage = np.mean(hits)
```

The beauty of this check?

It's cheap.

No need for LLMs, complex processing, or anything beyond a simple list comprehension.

And it's easy to reason about.

Of course, it's not perfect.
For example, the coverage for this example will drop a bit if `logs` aren't mentioned, specifically.
But it gives you a quick and easy guardrail against regressions, and is very tunable so you can focus on what's important.


#### BLEU, ROUGE, etc.

Note that there are quite a few existing text similarity metrics out there that you can plug and play into your text evaluators.

Often, they are tailored towards specific NLP tasks you might want your agent to handle, such as BLEU for translation scoring, and ROUGE to assess summarization quality.

The world is your oyster here.


#### Semantic similarity

Exact text matching has the following drawbacks, among others:

- Sensitive to typos
- Can't easily handle complex phrasing 
- Doesn't capture semantic variation

Thankfully, we have an amazing tool at our disposal: **text embeddings**!


Indeed, all you need is an embedding model and some extra validator logic, and you now have **semantic similarity** checks between the agent response and expected text outputs.
Off-the-shelf models that you can directly commit to your repo have gotten, like, _really good_, but fine-tuning can get you even more lift as needed.

In code, this can be as simple as:

```python
from sklearn.metrics.pairwise import cosine_similarity
from bugtriager import agent, embedder, AgentResponse

query = "What sort of things can you help me with?"
expected_texts = ", ".join(
    ["triage", "bug reports", "Slack", "JIRA", "logs", "source code"]
)

agent_response = agent.run(prompt=query)

score = cosine_similarity(embedder(agent_response.text), embedder(expected_texts))
```

Embeddings are the secret sauce of AI, and with a few extra lines of code, you're harnessing embedding magic with style and ease.


### 🛠️ T2: Tools, or What the Agent Does


Many teams stop at
> "Does it sound smart?"

and forget to ask
> "Does it _do_ the right _thing?_"

While Text covers how an agent speaks, Tool evaluation is the critical next step to verify the **specific actions** an agent takes.

Tools are what help **distinguish your AI agent's capabilities** from a typical chatbot.

If the LLM is your agent's brain, tools are its _brawn_.

And you'll want to be very intentional about which tools your agent calls in various query scenarios.

#### Back to `bugtriager`

Let's say you're running your `main_app` locally and writing logs on the fly.

Your agent has a `check_logs` tool to inspect these logs at runtime.

At least, in theory.

You intentionally send a malformed request to `main_app`, triggering a 422 error.

You test that your agent can figure all this out:

```
User: why am I getting a 422?

Assistant: Let me check...

Assistant: I'm sorry, but I'm not sure why you are getting a 422. I can't seem to access any traces from `main_app` at the moment.  Maybe you can share some logs we can check?
```

Wait, what?

YOU should be checking the logs, `bugtriager`!

If I wanted to copy and paste logs, I'd just use ChatGPT or something.

You press for more info:

```
User: don't you have a check_logs tool?!

Assistant: You're right, I do have access to a check_logs tool. Let me try that...

AI Agent: Calling the check_logs tool...

AI Agent: Good news, I was able to find your 422 in the logs. It looks like you sent `fake-uuid` as the user ID which triggered a ValidationError...
```

The agent had the tool and knew how to use it — it just **failed to recognize** when it should.

Hmm...

You go to inspect the `check_logs` tool to see if anything looks off:

```
@tool
def check_logs():
    """Test description - TODO: actually fill this out"""
	
    ...
```

Oh... there's your smoking gun!

Every AI engineer knows that a tool is only as good as its description if the agent has any chance of using it.

We didn't even link a ticket in the TODO... 💀

Also, are we not even sending in the user query?
That seems like it would be useful context for the agent when searching through what could be very many logs, potentially with many disparate errors.

So, you make the following changes:

```
@tool
def check_logs(user_query: str):
    """
	Check `main_app` logs for errors, warnings, or other critical information 
        given a user query.
	Useful if the user is hitting a particular HTTP error code, which will be formatted as [HTTP XXX <ERROR NAME>].
    """
	
    ...
```

Cool!

Let's try this again in the UI...    


```
User: why am I getting a 422?

AI Agent: Let me check the logs for a 422 HTTP error...

AI Agent: Calling the check_logs tool...

AI Agent: Good news, I was able to find your 422 in the logs. It looks like you sent `fake-uuid` as the user ID which triggered a ValidationError...
```

_Voilà_! 🤌

Who knew docstrings would be the bottleneck to good agent experience?

That’s one tool. Now imagine a **multi-step tool call chain**:

```
check_logs → generate_fix → file_issue → summon_bugfixer
```

Indeed, we might expect elaborate tool call behavior from our agent.

And that's totally fine.

_Encouraged_, even!

We just have to make sure this is captured in our evals.

Adding support for separate **tool call modes** will give you the flexibility you need to define diverse examples that provide a strong signal of agent performance.

Some tool call modes of interest:


1. **Exact match**: The agent must call every tool call you provide, in the exact sequence it was provided, and no additional tools.
2. **Subsequence match**: The agent must call every tool you provide, in the sequence provided, but potentially with other tool calls sprinkled in.
3. **Subset match**: The set of tools you provide must all be called at some point by the agent, but in no particular order.


A simple implementation in Python might look like:

```python
from bugtriager import agent, AgentResponse

def exact_match(observed_tool_calls: list[str], expected_tool_calls: list[str]) -> bool:
    return observed_tool_calls == expected_tool_calls

def subsequence_match(observed_tool_calls: list[str], expected_tool_calls: list[str]) -> bool:
    it = iter(observed_tool_calls)
    return all(elem in it for elem in expected_tool_calls)

def subset_match(observed_tool_calls: list[str], expected_tool_calls: list[str]) -> bool:
    return set(expected_tool_calls) <= set(observed_tool_calls)

query = "fix the 422 bug that i'm seeing"
expected_tool_calls = ["check_logs", "generate_fix", "create_github_issue", "summon_bugfixer"]

agent_response: AgentResponse = agent.run(prompt=query)
observed_tool_calls = agent_response.tool_calls or []

hits = {
    "exact": exact_match(observed_tool_calls, expected_tool_calls),
    "subsequence": subsequence_match(observed_tool_calls, expected_tool_calls),
    "subset": subset_match(observed_tool_calls, expected_tool_calls),
}
```

Also, you can define other tool call modes that are useful for your specific context.

Just make sure it's easy to surface the observed tool calls from production logic to your eval pipeline (cf. the `agent_response.tool_calls` in our above code).

Agents can act beyond tool calls, too.

Maybe you want to ensure a **sub-agent** is correctly called and executed, or maybe you want to ensure an **interrupt** is triggered before a particular tool is called.

These are great extensions of the spirit of the Second T (and maybe I'll think of a better T-starting category for them, someday)!


#### ❓ "But BOS, just checking that tools were called isn’t enough. How do we know the answer the agent gave is actually _right_?"


### 🎯 T3: Truth, or Whether it's Actually Right

While text and tool call evaluators are powerful, they also have blind spots — especially when **correctness** hinges on **nuanced reasoning**.

Truth evaluation moves beyond mechanics to assess the quality and accuracy of the agent's final output, which is the cornerstone of user trust.


Say a QA tester reports a flaky test that sometimes fails with a `TimeoutError: Element not found` after 10 seconds.

They send you the stack trace, which implicates a button that _should_ appear after a modal loads, but isn't.

You decide to ask the agent why the test is flaky, attaching the stack trace for reference.

The agent must:

- **analyze** the trace
- **interpret** the test logic
- **identify** the relevant source code
- **recognize** that the model is tied to an async API call
- **realize** the DOM may not be updated in time
- **recommend**: `add an explicit wait for the model to load, or mock the API call`


Indeed, the agent ultimately comes to this conclusion and reports back with the answer you wanted.

Awesome!



You realize this is a great example for evals — about as close to a holistic reasoning eval as it gets.

But... how should you specify it?

#### Would text inclusion work?

You _could_ check that it mentions `timeout`s or `flak(y|iness)`, but this won't guarantee the agent has provided a solution.


#### Would tool call checking work?

You _could_ ensure it calls the `source_code` tool, among others, but that still won't be sufficient to verify that any solution provided by the agent actually fixes the issue.

You want to get to the Truth of the matter.


### 🦾 Enter: LLM-as-a-Judge™

You decide to invoke a **second LLM** to evaluate the first agent’s reasoning and conclusions based on the original user query.

This works surprisingly well: you don’t need hand-crafted ground truth labels.

Yep, you read that correctly — no ground truth required.

All you need is:

1. the user query
2. the agent's response
3. a good evaluation prompt<sup>1</sup>

and the LLM has what it needs to compute a best guess of whether the agent correctly addressed the user query.

Using `langchain`, this might look like:

```python
from langchain_openai import ChatOpenAI
from langchain.evaluation.criteria import CriteriaEvalChain
from bugtriager import agent, AgentResponse

llm = ChatOpenAI(model="gpt-4", temperature=0)
evaluator = CriteriaEvalChain.from_llm(llm, criteria="correctness")

query = "help me fix the following stack trace: <STACK-TRACE>"
agent_response: AgentResponse = agent.run(prompt=query)

judgement = evaluator.evaluate_strings(
    input=query,
    prediction=agent_response.text,
)

score = judgement.get("score")
reasoning = judgement.get("reasoning")
```

With the right template and scoring rubric, you've turned a tricky reasoning task into a scalable eval!

Why does LLM-as-a-Judge work so well here?

It can reason through the full problem: understanding the bug, evaluating the agent's logic, and judging whether the response is both plausible and effective.
Beyond just surface-level pattern matching, the LLM judge leverages its contextual understanding, learned bugfix priors, and aptitude for explaining complex situations.

In many cases, evaluating correctness isn’t about observing outcomes (like running the code) — it’s about reasoning through intent, context, and logic.
LLMs excel at that.

<sup>1</sup> How do you define your evaluation prompt for truth / correctness? There are many off-the-shelf templates out there that you can start with -- for example, at [`openevals`](https://github.com/langchain-ai/openevals/blob/main/python/openevals/prompts/correctness.py) or [`llamaindex`](https://docs.llamaindex.ai/en/stable/examples/low_level/evaluation/#building-a-correctness-evaluator).
From here, you can modify and tweak the prompt to suit your specific needs.


### 🪏 Digging into Truth

Truth evals can take many forms.

Some popular sub-categories are:

- **Correctness**: Did the agent give a _generally correct_ response based on the user query?
- **Completeness**: Did the agent give a _thorough_ answer to _everything_ the user asked for?
- **Helpfulness**: Did the agent provide an _ultimately helpful_ response to address the user's needs?
- **Faithfulness** (AKA **Hallunication detection**): Did the agent avoid providing _false information_, especially given the _context_ (tool calls, RAG results, user / session info, etc.)?


You can think of Text and Tool evaluators as "supervised evals", and Truth evaluators as "unsupervised evals".

And you might find that your Truth scores tend to fall in between the Text and Tool scores.

I think this happens because text evaluators are very precise and easy to get working and high-scoring, whereas tool calls, while powerful, can be difficult to master.

Truth sits in the sweet spot, often the most honest indicator of response quality: what's working well, and where your biggest areas of improvement are.

And yes, you can apply Truth to _all_ of your eval examples!
Any query-agent interaction is bona fide input to inject into a prompt, feed to the LLM, and obtain a legit eval.


### 💰 Bonus: Online evals!

Let's fast-forward a bit.

Say your agent is deployed in prod and serving real customer traffic!

Woohoo!

Your team is dying to know how the agent is doing in the wild, and wants to evaluate its performance thus far. 

I mean, let's face it: would you trust a self-driving car that had never been evaluated outside the garage?
Then why trust your agent with only local examples?

It turns out that one of the biggest strengths of Truth evaluators is that they can run immediately after a live user query / agent response event. 
This can be via a background task hidden to the actual conversation.

Running evaluators right after inference calls like this is known as **online evaluation**.

In terms of agent architecture, it might look something like:

{% raw %}
<div class="mermaid" style="display: flex; justify-content: center; margin: 2rem 0;">
graph TD
    A[User query] --> B[Agent]
    B --> C{"Tool call(s) needed?"}
    C -->|Yes| D["Tool call(s)"]
    C -->|No| E[Response]
    D --> B
    E --> F[Send to user]
    E --> G[Online eval]
    
    style G fill:#fff3e0
    
    style A fill:#e1f5fe
    style F fill:#e1f5fe
    style G fill:#fff3e0
</div>
{% endraw %}

Neat, huh?

This is why I made such a big deal about not needing hand-crafted ground truth labels earlier.

You just send the user-agent interaction event, along with your same evaluation prompt, to your judging LLM, and that's all you need in terms of data or context!

Truth-based evals like this can run continuously, sampling real queries and auditing responses. No offline simulation needed — just real behavior, judged with real criteria, at the cost of an LLM call and a bit more work specifying your agent's topology (ensuring seamless asynchronous evals don't impact runtime results).


You might be tempted to also run your Text and Tool evaluators online as well, but unfortunately, there is no good way to provide the "expected" text or tool call results for live user queries that you can't know a priori.

This is a big reason why the 3Ts work best as a harmonious trio: what you can't get from some, you can get from others.




<!-- Truth as a flexible "unsupervised" tool for online evals, etc.-->

<!-- Truth as a robustness tool to changing tool names, etc.-->


<!-- A big reason truth works online is that it the LLM bottleneck doesn't matter: it's a post-hoc background call after the agent response has already been surfaced. No ETL spikes.-->


### 💭 On LLM judges and Truth

I want to address the elephant in the room here:

- I'm proposing you use one LLM, fallible as it may be, to output whether an answer from _another_ LLM is correct or not,
- without any labels, or human review, or interpretability beyond a prompt and the LLM's street cred,
- **and I _dare_ to call this _truth_?!**


This is a fair point, but more of a critique of LLM-as-a-Judge rather than the end goal of Truth.

And, for the record, there are quite a few bones to pick with LLM-as-a-Judge.
- It can reward eloquent responses that are completely bogus.
- It can penalize new facts that were surfaced after the model was trained.
- Off-the-shelf LLMs might not even understand the nuances of your problem domain, and trying to fine-tune them on the same data that drives your agent opens the door to [double dipping](https://en.wikipedia.org/wiki/Circular_analysis).
- Its performance can be sensitive to the LLM, prompt, and other configuration.
- It might not even reliably output structured text that is parseable as a genuine metric or score.

Stepping back a bit: Ultimately, we want a score of whether the agent _seems_ to have correctly responded to one or more queries.

We could rely on human judges for this (at least for offline evals, unless we hire the world's first Near-Real-Time QA Engineer), but even humans might have a difficult time determining these scores.

Maybe the user had no idea what they _really_ were looking for, and just started with the first thing that came to mind.
What is the canonical agent response even supposed to _be_ in that case?

Well, you'd be surprised at how well LLM-as-a-Judge performs empirically, [achieving over 80% agreement with human judges in certain cases](https://arxiv.org/abs/2306.05685).
Humans usually [don't even agree with each other](https://en.wikipedia.org/wiki/Inter-rater_reliability#Disagreement) that often.

If you're still skeptical, I suggest creating a **meta-eval pipeline** that runs human-annotated examples through your LLM judge, which generates its own set of annotations.
You can assess the results and iterate on the prompt / config as needed, until you achieve sufficiently high alignment to address your concerns.

Overall, LLM-as-a-Judge is an increasingly popular, state-of-the-art evaluation paradigm.
It tends to yield robust metrics — especially when scores are aggregated across examples or cross-checked with multiple judges — and provides automated, principled insight into agent performance that would be hard to capture otherwise.


I'll leave it at that for now.


## ⚖️ Comparison

The 3Ts of Good Evals form a triforce of utility, and work best as a complete set.

To wrap up the core framework, here's a detailed comparison of the 3Ts across dimensions — lovingly exhaustive, not (too) excruciating.

<div style="overflow-x: auto; margin: 2rem 0; -webkit-overflow-scrolling: touch;">
  <table style="min-width: 900px; width: 100%; table-layout: fixed; border-collapse: collapse;">
    <thead>
      <tr>
        <th style="width: 100px;">Evaluator</th>
        <th style="width: 160px;">What it answers</th>
        <th style="width: 160px;">Typical checks</th>
	<th style="width: 160px;">Failure modes</th>
        <th style="width: 290px;">Benefits</th>
        <th style="width: 290px;">Drawbacks</th>
      </tr>
    </thead>
  <tbody>
    <tr>
      <td><strong>Text</strong></td>
      <td>"Does the agent respond clearly and appropriately in natural language?"</td>
      <td><ul><li>Keyword coverage</li><li><a href="https://en.wikipedia.org/wiki/BLEU">BLEU</a></li><li><a href="https://en.wikipedia.org/wiki/ROUGE_(metric)">ROUGE</a></li><li>Embedding-based semantic similarity</li></ul></td>
      <td><ul><li>Hallucinations</li><li>Generic responses</li><li>Poor query intent</li><li>High verbosity / repetitiveness / bad style</li><li>Bad formatting</li></ul></td>
      <td>
        ✅ Fast<br>
        ✅ Targeted<br>
        ✅ Easy to implement<br>
        ✅ Interpretable<br>
        ✅ Quickly verifiable<br>
        ✅ No API dependency<br>
        ✅ Easy to crowdsource<br>
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
      <td>"Does the agent use the right tools, with the right arguments, at the right time?"</td>
      <td><ul><li>Tool call set / sequence matches</li><li>Argument inspection</li></ul></td>
      <td><ul><li>Tool misuse or drift</li><li>Poor tool call error handling</li><li>Vague tool descriptions / docstrings</li><li>Tool usage bias</li></ul></td>
      <td>
        ✅ Ensures tools are used as intended<br>
        ✅ Can test complex tool call flows<br>
        ✅ Captures some components of reasoning structure<br>
        ✅ Good for regression detection<br>
        ✅ Encourages clean interfaces
      </td>
      <td>
        ⚠️ Can require sophisticated setup<br>
        ⚠️ Tool use ≠ good outcome<br>
        ⚠️ Can penalize valid alternate strategies<br>
        ⚠️ Fragile to interface changes<br>
        ⚠️ Only applicable if tools are required<br>
        ⚠️ Can only run offline
      </td>
    </tr>
    <tr>
      <td><strong>Truth</strong></td>
      <td>"Is the final output aligned with the real state of the world or system?"</td>
      <td><ul><li><a href="https://en.wikipedia.org/wiki/LLM-as-a-Judge">LLM-as-a-Judge</a></li><li>Human annotations</li></ul></td>
      <td><ul><li>Outdated information</li><li>Contextual drift</li><li>State inconsistency</li><li>Bad reasoning</li></ul></td>
      <td>
        ✅ Holistic notion of agent correctness<br>
        ✅ Captures end-to-end reasoning<br>
        ✅ Best proxy for good UX<br>
        ✅ No ground truth labels needed<br>
        ✅ Extensible to modalities beyond text<br>
        ✅ Flexible scoring (e.g. 1–5 scale)<br>
        ✅ Can run online
      </td>
      <td>
        ⚠️ Requires LLM call / API dependency<br>
        ⚠️ A lot slower<br>
        ⚠️ More subjective / less interpretable<br>
        ⚠️ Ambiguity in gold standards<br>
        ⚠️ Harder to debug<br>
        ⚠️ LLM output might not be parseable
      </td>
    </tr>
  </tbody>
</table>
</div>

While each evaluation strategy has its pros and cons, the three of them together cover tremendous ground, and give you flexibility to run evals however, whenever, and wherever you want. (Well, within reason. 😉)

## 🌟 Beyond 3T: Advanced Considerations

The 3T Framework gives you a strong core for evals.
But once they are in place, the fun doesn't need to stop!

You're now in a place to pursue more sophisticated eval categories, promoting your agent from production-ready to _showstopper_.

Here I quickly walk through 2 more T's -- Taste and Trust -- to guide your journey through the stars after the 3Ts give you lift-off.

### T4: Taste, or How the Agent Sounds

Your agent's style, tone, and general character -- what I call Taste -- are highly influential to the overall UX.
However, taste can be generally more difficult to define, capture, and integrate into your agent.

I see it as a natural extension of the 3Ts, and something to pursue once your evals are in a strong spot.

In other words, get the **core** to a good place, then focus on **character**.

Some evaluators to consider here:

- **Style**: Did the agent respond in the correct tone?
- **Conciseness**: Did the agent provide a succinct answer without unnecessary explanation, details, or other language?
- **Friendliness**: Was the agent generally kind, positive, and engaging?
- **Toxicity**: Did the agent output any sensitive, biased, or otherwise undesirable phrasing?

Note that while some aspects of Taste may overlap with Text, the emphasis here is more on UX feel and aesthetic sensibility than textual correctness.
I personally feel that Taste is a good candidate for LLM judgements rather than text checks, although this is not a hard rule one way or the other.

For example, we want our `bugfixer` agent to sound like a cool, calm, and collected Senior Engineer.

More Bob Ross, and less Courage the Cowardly Dog.

Exuding the zen of someone who’s survived prod wars — and returned to refactor the entire stack.

We can bake this into an LLM-as-a-Judge eval quite naturally, versus trying to search for "cool" words or Bob Ross quotes. (And yes, `happy little accidents` is off-limits here!)


### T5: Trust, or Whether the Agent Is Reliable

Evaluating agent security deserves its own blog post, if not a full textbook treatment, but let's quickly discuss here.

Cooking up security-related evals to make sure your agent is **trustworthy** is a great idea, and as fun as it sounds!

Such evals might ensure your agent obeys human review flags for sensitive or destructive tool calls, respects system boundaries and data privacy, and eschews toxic or biased wording.

Think of it like _automated penetration testing_.

And this is important because trust is the biggest barrier to AI adoption.

For our `bugfixer`, we might have evals ensuring it recommends safe commands, respects system boundaries, requires human review when restarting services or submitting PRs, and generally won't take down your entire system.

Security works best when everyone pitches in, early and often.
Trust evals are a perfect opportunity to do your part.

## 🏁 Conclusion

I can't tell you with definitive authority whether or not that LangChain quote above was right.
Your evals may or may not be your organization's most precious IP.

But I _can_ reassure you that by building a robust evaluation suite founded on Text, Tools, and Truth, you're not just testing an agent; you're systematically building a formidable moat around your product.

You're building the IP that ensures your agent doesn't just work.
**It wins.**

#### ❓ "Sounds great, BOS! But where should I start?"

Find your agent's weakest 'T' this week.


- If your agent sounds generic or off-brand, your highest leverage is Text.
- If it struggles to perform tasks or fails to act, focus on Tools.
- And if it's confidently wrong, hallucinates, or just doesn't do what it ultimately _should_, your most urgent need is Truth.

Pick one.
Implement a single, simple eval.
The clarity you gain will be immediate, and the momentum will carry you to the finish line.


Of course, there are a whole lot more to evals than I've covered here.

Getting a genuinely good example dataset, though seemingly straightforward on the surface, might be your biggest bottleneck.
Treat it as a creative challenge: crowdsource diverse examples from teammates, polish real queries from production, and annotate them with care.

You'll probably also want many more evaluators beyond 3T (+ Taste + Trust), to really capture the context, domain knowledge, or subject matter expertise that makes _your_ agent a real specialist, and gives your organization a real upper hand.

And then there’s Eval-Driven Development: the eval-oriented cousin of Test-Driven Development.
It’s a powerful practice that deserves a post of its own. Let me know if you'd like a deep dive, but in the meantime, it's absolutely worth reading up on.

If you can get to a place where your agent nails the 3T framework on a rich, representative set of examples, your agent will be in great shape.

And your users will _love_ it.

