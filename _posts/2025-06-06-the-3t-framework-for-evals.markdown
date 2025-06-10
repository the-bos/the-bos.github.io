---
layout: post
title:  "The 3T Framework for Agent Evals: Text, Tools, and Truth"
date:   2025-06-06 8:00:00 -0600
---

{% raw %}
<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
  mermaid.initialize({ startOnLoad: true });
</script>
{% endraw %}

> **TL;DR**: In this post, I introduce the 3 T's of evals -- Text, Tools, and Truth -- and show why they comprise a powerful core for evals.
I distinguish between offline and online evals, make a case for why you should do do both, and demonstrate how the 3Ts have your back here.




## 🎤 "Agent evals are your most precious IP."

This line was dropped on center stage at this year's [LangChain Interrupt conference](https://interrupt.langchain.com), sparking the loudest murmur in the crowd I'd heard that day.

After an insightful first morning of talks and keynotes covering agent architectures and conceptualizing the Agent Engineer™️ as an emergent role, the entire afternoon was dedicated to instilling -- no, _shamelessly indoctrinating_ -- the following idea into everyone's brains:

#### You NEED to do evals, and you NEED to do them well.

This was something a lot of us kind of knew, but didn’t really _feel in our bones_ until that moment.

And I think it's because, frankly, evals seem kind of _hard_, at least to do well.

And also, why not just focus on building out the shiny new features we were promised we could prioritize after successfully releasing Beta, features we _know_ our users will love?

Yet, the post-conference chatter over drinks seemed to center around the same question: are evals really as critical as everyone seems to be insisting?


## 💥 The hard truth: Anyone can build an AI agent.

Indeed, there are a wealth of resources, learning materials, and open source tooling to help anyone with an internet connection and a smoot of curiosity to stand up their very own agent -- and do it astonishingly fast!

#### But it's a _lot_ harder to build a **Good Agent™**.


Well, what is a good agent?

Is it _magic_?

No.
It's **hard work**.

And I want to echo the idea that a lot of the hard work _is_ manifest in your evals.

When I say evals, I mean a dedicated pipeline that provides your agent with example user queries (or other supported inputs), and scores the agent responses / general behavior.

I mean, sure -- it's _possible_ to crank out a decent agent based solely on subjective evaluation, gut instincts, and pure vibes, but unless you have a designated team of human annotators reviewing agent output on a weekly basis, this likely won't keep you afloat for too long.


## 🌅 The good news: YOU can build a Good Agent™!

I'm not here to say evals are easy.

In fact, they're arguably the most difficult component of AI Engineering, which is already a premier technical challenges of our time.

Between the daily articles, the LinkedIn posts, and all of the hype, buzz, and commotion, it's easy to feel lost in the noise, confused about how to do evals right, or what evals even _are_.

Fear not, however, as I'm excited to share a formulation that has helped me in my day-to-day, and just might give you some clarity around evals as well.

## 🕵️ Case study: The `bugtriager` agent


Imagine you’ve just shipped an AI agent that helps triage bug reports.
It lives in Slack, reads messages from engineers and QA testers, and responds with diagnostic suggestions — maybe with proposed fixes, or even a button you can press to open a corresponding PR.

You’ve designed it, architected it, tested it in dev and staging.

Maybe it parses stack traces, queries logs, and hits internal APIs as you knew it could.
Maybe it’s even starting to sound helpful.

But now your team is asking:

<blockquote style="font-style: normal;">
"How do we know it <em>really works</em>?"
</blockquote>

And a more unsettling voice creeps in from your own head:

<blockquote style="font-style: normal;">
  "Wait... how do <em>I</em> know it really works??"
</blockquote>


You’ve seen a few promising replies, and a couple weird ones.
You’ve read the logs and monitored the dashboards.
But you need more than anecdotes.

You need a structured, clear view.

A lens to evaluate not just whether your agent said something plausible — but whether it provided **correct information** to the user, used the **right tools**, and aligned with the **truth of the system**.

You need **evals**, and they need to capture this structured, clear view with simplicity, capability, and success.


## 3️⃣ The 3 T's of Good Evals: Text, Tools, and Truth

### 📄 T1: Text

The big deal about AI agents is that they are a **natural language interface** between the user and the code driving your product.

The user asks it to do a thing, and it does that thing.

Sometimes that thing is a simple text answer, without even talking about tools, actions, or anything too fancy.

In our `bugtriager` example, let's say you have the following conversation with the agent:


```
User: "What sort of things can you help me with?"

AI Agent: I can help triage bug reports, whether they live in Slack, JIRA, you name it. I can look at logs, source code, and other sources of proprietary information to help you resolve bugs fast.
```

Great response!
Users will love this.

But, later that day, after some changes were pushed, you ask it the same question to double check everything is still good:

```
User: "What sort of things can you help me with?"

AI Agent: I'm an AI assistant here to help you with any questions or tasks you have! How may I assist today?
```

Wait a minute.

It didn't even _mention_ bugs, or triaging, or really anything that should distinguish it from any other run-of-the-mill AI.


You decide to add an example in your evals that makes sure the agent responds to this particular query with the right sort of **keywords**.

In terms of code: assuming you have a nice typed `AgentResponse` class as so:

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

The key thing to note here is how cheap this basic **text inclusion** is. 
No need for LLMs, complex processing, or anything beyond a simple list comprehension.

Of course, it's not perfect (e.g., the coverage for this example will drop from 100% to 83% if `logs` aren't mentioned, specifically), but it gives you a quick and easy guardrail against regressions.

And it's easy to reason about.

_And_ you can tune or weight things as needed to capture what is truly important, vs what is a good to have.


#### BLEU, ROUGE, etc.

Note that there are quite a few existing text similarity metrics out there that you can plug and play into your text evaluators.

Often, they are tailored towards specific NLP tasks you might want your agent to handle.

For example, you can use BLEU for translation scoring, and ROGUE to assess summarization quality.

The world is your oyster here.


#### Semantic similarity

Exact text matching has the following drawbacks, among others:

- Sensitive to typos
- Can't easily handle complex phrasing 
- Doesn't capture semantic variation

Thankfully, we have an amazing tool at our disposal: text embeddings!


Indeed, all you need is an **embedding model** (off-the-shelf models that you can directly commit to your repo have gotten, like, really good, but fine-tuning can always get you even more lift), and some extra validator logic, and you now have **semantic similarity** checks between the agent response and expected text outputs.

In code, this can be as simple as:

```python
import numpy as np
from bugtriager import agent, embedder, AgentResponse

def cosine_similarity(a, b):
    return np.dot(a, b)/(np.linalg.norm(a)*np.linalg.norm(b))

query = "What sort of things can you help me with?"
expected_texts = ", ".join(
    ["triage", "bug reports", "Slack", "JIRA", "logs", "source code"]
)

agent_response = agent.run(prompt=query)

score = cosine_similarity(embedder(agent_response.text), embedder(expected_texts)]
```



Let's go!



### 🛠️ T2: Tools

Many teams stop at "does it sound smart?"

But they need to start asking: "does it _do_ the right _thing?_"

Tools are what help **distinguish your AI agent's capabilities** from a typical chatbot.

If the LLM is your agent's brain, tools are its _brawn_.

It goes without saying that you'll want to be very intentional about which tools your agent calls in various query scenarios.

Going back to our `bugtriager`:

You have your main app running at `main_app`, saving logs to a logfile on the fly.

Your agent has a `check_logs` tool to inspect these logs at runtime.

At least, in theory.

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
User: don't you have a check_logs tool?!

Assistant: You're right, I do. Let me try that...

AI Agent: Good news, I was able to find your 422 in the logs. It looks like you sent `fake-uuid` as the user ID which triggered a ValidationError...
```

Okay, so the agent has the tool, and can use the tool, but didn't seem like the tool was an appropriate choice based on my query...

Hmm...

You go to inspect the `check_logs` tool to see if anything looks strange.

```
@tool
def check_logs():
    """Test description - TODO: actually fill this out"""
	
    ...
```

Oh... there's your smoking gun!

Everyone knows that a tool is only as good as its description if the agent has any chance of using it.

We didn't even link a ticket in the TODO... 💀

Also, are we not even sending in the user query?
That seems like it would be useful context for the agent when searching through what could be very many logs, potentially with more errors than are relevant to what the user's asking about.

So, you make the following changes:

```
@tool
def check_logs(user_query: str):
    """
	Check the `main_app` logs for errors, warnings, or other critical information 
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

_Encouraged_, even!

We just have to make sure this is captured in our evals.

Adding support for separate **tool call modes** will give you the flexibility you need to define diverse examples that provide a strong signal to agent performance.

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

def subset_match(observed_tools: list [str], expected_tools: list[str]) -> bool:
    return set(expected_tools) <= set(observed_tools)

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

The cool thing here is that you can define other tool call modes that are useful for your specific context.

Just make sure it's easy to surface the observed tool calls from production logic to your eval pipeline (cf. the `agent_response.tool_calls` in our above code).

It's worth noting that agent actions aren't confined to tool calls, necessarily.
For example, maybe you want to ensure a **sub-agent** is correctly called and executed, or maybe you want to ensure an **interrupt** is triggered before a particular tool is called.
These are great extensions of the spirit of the Second T (and maybe I'll think of a better T-starting category name for them, someday)!


#### ❓ But BOS, just checking that tools were called isn’t enough. It _can't_ be enough. How do we know the answer the agent gave is actually _true_?


### 🎯 T3: Truth

While text and tool call evaluators are powerful, they also have blind spots — especially when correctness hinges on nuanced reasoning.


Let's say a QA tester reports a flaky test that sometimes fails with a `TimeoutError: Element not found` after 10 seconds.

They send you the stack trace, which implicates a button that _should_ appear after a modal loads, but isn't.

You decide to ask the agent why the test is flaky, attaching the stack trace for reference.

The agent must:

- **analyze** the trace
- **interpret** the test logic
- **identify** the relevant source code
- **recognize** that the model is tied to an async API call
- **realize** the DOM may not be updated in time
- **conclude**: `add an explicit wait for the model to load, or mock the API call`


Indeed, the agent ultimately comes to this conclusion and reports back with the answer you wanted.

Awesome!



You realize this is a great example for evals — about as close to a holistic reasoning eval as it gets.

But... how should you specify it?

#### Would text inclusion work?

You _could_ check that it mentions `timeout`s or `flak(y|iness)`, but this won't guarantee that its solution will actually fix the test.


#### Would tool call checking work?

You _could_ ensure it calls the `source_code` tool, among others, but that won't be sufficient to verify the agent's **correctness**.

Or what I'm calling **truth**.


#### 🦾 Enter: LLM-as-a-Judge™

You decide to invoke a **second LLM** to evaluate the first agent’s reasoning and conclusions based on the original user query.

This works surprisingly well: you don’t need hand-crafted ground truth labels.

Let me repeat that: *you don’t need hand-crafted ground truth labels*!

All you need is:

1. the user query
2. the agent's response
3. a (good) evaluation prompt<sup>1</sup>

and the LLM has what it needs to compute a best guess of whether the agent correctly addressed the user query.

In code, this could be as simple as:

```python
import numpy as np
from some_awesome_llms import LLMJudge
from bugtriager import agent, AgentResponse, EVALUATION_PROMPT

query = "help me fix the following stack trace: <STACK-TRACE>"

agent_response: AgentResponse = agent.run(prompt=query)

evaluation_prompt = EVALUATION_PROMPT.format(
    user_query=query,
    agent_text=agent_response.text,
)

judgement = LLMJudge(prompt=evaluation_prompt)
```


With the right template and scoring rubric, you've turned a tricky reasoning task into a scalable eval!

And yes, you can apply this to _all_ of your examples!
Any user-agent interaction is bona fide input to inject into a prompt, feed to the LLM, and obtain a legit eval.

You can think of Text and Tool evaluators as "supervised evals", and Truth evaluators as "unsupervised evals".

And you might find that your truth / correctness scores tend to fall in between the text and tool scores.

I think this happens because text evaluators are very precise and easy to get working and high-scoring, whereas tool calls, while powerful, can be more difficult to obtain excellence on across the board.

Truth gives you a nice juicy middle that might be the most honest indicator of response quality: what's working well, and where your biggest areas of improvement are.

<sup>1</sup> How do you define your evaluation prompt for truth / correctness? There are many off-the-shelf templates out there that you can start with -- for example, at [`openevals`](https://github.com/langchain-ai/openevals/blob/main/python/openevals/prompts/correctness.py) or [`llamaindex`](https://docs.llamaindex.ai/en/stable/examples/low_level/evaluation/#building-a-correctness-evaluator).
From here, you can modify and tweak the prompt to suit your specific needs.


#### 💰 Bonus: Online evals!

Let's fast-forward a bit and say your agent is deployed in prod and serving real customer traffic!
Woohoo!

Your team is natuarally very curious how the agent is doing in the wild. 

I mean, let's face it: would you trust a self-driving car that had never been evaluated outside the garage?
Then why trust your agent with only local examples?

It turns out that one of the biggest strengths of the truth evaluator is that it can run immediately after a live user query / agent response event (as a background event hidden to the actual conversation).

Running evaluators right after inference calls like this is known as **online evaluation**.

In terms of agent architecture, it might look something like:

{% raw %}
<div class="mermaid" style="display: flex; justify-content: center; margin: 2rem 0;">
graph TD
    A[User query] --> B[Agent]
    B --> C{Tool call?}
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

You just send this event, along with your same evaluation prompt, to your judging LLM, and that's it!
You suddenly have more eval examples and correctness labels, at the cost of an LLM call and a bit more work specifying your agent's topology.

Truth-based evals like this can run continuously, sampling real queries and auditing responses. No offline simulation needed — just real behavior, judged with real criteria.

You might be tempted to also run your Text and Tool evaluators online as well, but unfortunately, there is no good way to provide the "expected" text or tool call results for live user queries that you can't know a priori.

This is a big reason why the 3Ts work best as a harmonious trio: what you can't get from some, you can get from others.




<!-- Truth as a flexible "unsupervised" tool for online evals, etc.-->

<!-- Truth as a robustness tool to changing tool names, etc.-->


<!-- A big reason truth works online is that it the LLM bottleneck doesn't matter: it's a post-hoc background call after the agent response has already been surfaced. No ETL spikes.-->


#### 📝 A note on truth

I want to address the elephant in the room here:

- I'm proposing you use one LLM, falliable as it may be, to output whether an answer from _another_ LLM is correct or not,
- without any labels, or human review, or intrepretability beyond our prompt and the LLM's street cred,
- and I _dare_ to call this _truth_?


This is a fair point, but more of a critique of LLM-as-a-Judge rather than the end goal of Truth.

And, for the record, there are quite a few bones to pick with LLM-as-a-Judge.
- It can reward eloquent responses that are completely bogus.
- It can penalize new facts that were surfaced after the model was trained.
- Off-the-shelf LLMs might not even understand the nuances of your problem domain, and trying to fine-tune them on the same data that drives your agent opens the door to [double dipping](https://en.wikipedia.org/wiki/Circular_analysis).

Ultimately, we want a score of whether the agent _seems_ to have correctly responded to one or more queries.

We could rely on human judges for this (at least for offline evals, unless we hire the world's first Near-Real-Time QA Engineer), but even humans might have a difficult time determining these scores.

Maybe the user had no idea what they _really_ were looking for, and just started with the first thing that came to mind.
What is the canonical agent response even supposed to _be_ in that case?

Well, you'd be surprised at how well LLM-as-a-Judge performs empirically, [achieving over 80% agreement with human judges in certain casees](https://arxiv.org/abs/2306.05685).
Humans usually [don't even agree with each other](https://en.wikipedia.org/wiki/Inter-rater_reliability#Disagreement) that often (anyone who's had even one holiday dinner with relatives can confirm).

Overall, LLM-as-a-Judge is an increasingly popular, state-of-the-art evaluation paradigm, producing generally robust metrics especially when aggregated, and gives us automated, principled insight into agent performance that would otherwise be difficult to capture.

I'll leave it at that for now.


## ⚖️ Comparison

The 3Ts of Good Evals form a triforce of utility, and work best as a complete set.

Below is a table of considerations for each group:

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
      <td><ul><li>Keyword coverage</li><li>BLEU</li><li>ROUGE</li><li>Embedding-based semantic similarity</li></ul></td>
      <td><ul><li>Hallucinations</li><li>Generic responses</li><li>Poor query intent</li><li>High verbosity / repetitiveness / bad style</li><li>Bad formatting</li></ul></td>
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
      <td>"Does the agent use the right tools, with the right arguments, at the right time?"</td>
      <td><ul><li>Tool call sequence match</li><li>Argument inspection</li></ul></td>
      <td><ul><li>Tool misuse or drift</li><li>Poor tool call error handling</li><li>Vague tool descriptions / docstrings</li><li>Tool usage bias</li></ul></td>
      <td>
        ✅ Ensures tools are used as intended<br>
        ✅ Can test complex tool call flows<br>
        ✅ Captures reasoning structure<br>
        ✅ Good for regression detection<br>
        ✅ Encourages clean interfaces
      </td>
      <td>
        ⚠️ Can require sophisticated setup<br>
        ⚠️ Tool use ≠ good outcome<br>
        ⚠️ Can penalize valid alternate strategies<br>
        ⚠️ Only applicable if tools are required<br>
        ⚠️ Fragile to interface changes<br>
        ⚠️ Can only run offline
      </td>
    </tr>
    <tr>
      <td><strong>Truth</strong></td>
      <td>"Is the final output aligned with the real state of the world or system?"</td>
      <td><ul><li>LLM-as-a-Judge</li><li>Human annotations</li></ul></td>
      <td><ul><li>Outdated information</li><li>Contextual drift</li><li>State inconsistency</li><li>Bad reasoning</li></ul></td>
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

While each evaluation strategy has its pros and cons, the three of them together cover quite a bit of ground, and give you flexibility to run evals however, whenever, and wherever you want (within reason!).


## 🏁 Conclusion

I can't tell you with definitive authority whether or not the quote from the LangChain stage was right.
Your evals may or may not be your organization's most precious IP.

But I _can_ reassure you that by building a robust evaluation suite founded on Text, Tools, and Truth, you're not just testing an agent; you're systematically building a formiddable moat around your product.

You're building the IP that ensures your agent doesn't just work.
**It wins.**

Try implementing just one 3T eval missing from your stack this week.
You might be surprised by what you learn.

Of course, there are a whole lot more to evals than I've covered here.

Getting a genuinely good example dataset, though seemingly straightforward on the surface, might be your biggest bottleneck.

You also will probably want many more evaluators beyond 3T, to really capture the context, domain knowledge, or subject matter expertise that makes _your_ agent a real specialist, and gives your organization a real upper hand.

And I didn't even _mention_ the trials and tribulations of doing actual, hardcore Eval-Driven Development.
(Though I might touch on this in a following post.)

But, if you can get to a place where your agent nails the 3T framework on a rich, representative set of examples, your agent will be in great shape.

And your users will _love_ it.


<!-- TODO: discuss text for chatbots, and how to generalize-->
