---
layout: post
title:  "A Case for QA Endpoints"
date:   2025-04-23 8:00:00 -0600
---

### 📢 Has this ever happened to you? 📢


* You build a wonderful ML model in some ML-friendly environment
* You deploy that model to some other environment (Staging, Prod, etc)
* You go to sanity check the deployed model, and...

#### ...the results look... kinda _sus_? 👀


### You're not alone.

This happens to the best of us, and IMO one of the key challenges of ML: ensuring consistent model performance across environments.

I won't get into why this happens or how to achieve this consistency in _this_ post, but one thing's for sure:

**We need a way to iterate quickly in order to get a model that performs well where it matters most: Prod.**


### Case study: The `examgenerator` service

Let's say you're building a product to generate academic exam content given some relevant context.

For example, for the following inputs:

```
Course Title: Introduction to Biology
Topic: Cell Structure
Number of questions: 20
```

Your generated exam might look like:

```
1. Which of the following organelles is primarily responsible for generating ATP, the main energy currency of the cell?
a)  Nucleus
b)  Endoplasmic Reticulum
c)  Mitochondria
d)  Golgi Apparatus
Correct Answer: c)

2. The cell wall, a rigid outer layer, is a characteristic feature of which type of cell?
a)  Animal cell
b)  Fungal cell
c)  Protozoan cell
d)  Bacterial cell
Correct Answer: b) and d)

...

20. What is the primary function of the cell membrane?
a)  To store the cell's genetic material.
b)  To synthesize proteins.
c)  To regulate the passage of substances into and out of the cell.
d)  To provide structural support to the cell.
Correct Answer: c)
```

There are some things to note here:

1. Multiple-choice exams have a very particular format: Numbered lists, with sub-lists, correct answers, and plenty of newlines.
2. Exams can be quite long, and might take a while to generate.

Because of (2), you opt for an **async flow**: the client calls your endpoint, providing the relevant model inputs as well as a callback URL:

```
> curl -X POST \
  /examgenerator/create \
  -H 'Callback-URL: /some/client/url' \
  -d '{"course_title": "Introduction to Biology", \
    "course_topic": "Cell Structure", \
    "num_questions": 20
  }'

{"success": true, "callback-id": "abc-123"}

```

Then your service eventually POSTs the model output to the provided callback URL with the corresponding callback ID.

No timeouts. No dropped connections. Just pure async magic.

Awesome!

You go to deploy your model to `staging`, send your example curl w/ a dummy callback endpoint, wait 20 seconds, and get your response with `"callback-id": "def-456"`.

Cool!

But what does the exam actually look like?

... Guess we'll check the service logs?

```
some-service-controller get logs > logs.txt
```

Then open `logs.txt` and CTRL+F for `def-456`:

```
Question: Which of the following organelles is primarily responsible for generating ATP, the main energy currency of the cell?
Correct Answer: Mitochondria

Question: The cell wall, a rigid outer layer, is a characteristic feature of which type of cell?
Correct Answer: Fungal and bacterial cells.

...

```

Gah!
This doesn't look like a good multiple choice exam if there aren't any choices!

So we enter our dev flow: make changes to the model in `staging`, and send test curls, save log file to text, open file, ctrl+f, ...

Wait. There has to be a better way to do this.

### Enter: the synchronous QA endpoint.

You decide to make a new endpoint that just returns the model output synchronously:

```
> echo -e $(curl -X POST \
  /examgenerator/create/qa \
  -d '{"course_title": "Introduction to Biology", \
    "course_topic": "Cell Structure", \
    "num_questions": 5
  }')


{"text": "Question: Which of the following organelles is primarily responsible for generating ATP, the main energy currency of the cell?
a)  Nucleus b)  Endoplasmic Reticulum c)  Mitochondria d)  Golgi Apparatus Correct Answer: c)
...
}
```

Cool!

Model quality isn't quite there, but we can see it's improving.
And we have a much quicker, easier iteration loop to keep going until parity is achieved.
(A couple more iteration loops and we realize we accidentally swapped the `temperature` and `top_p` values... 🙃)



### Case Study: A real-time study assistant

Your `examgenerator` service was wildly successful!

So much so that you decide to build out some more GenAI features to solve another academic need: the need for an around-the-clock study buddy.
Because let's face it: TAs have better things to do at 11 PM on a Friday than explain the nuances of contemporary macroeconomic policy to a stressed-out sophomore over e-mail.

Enter `studycompanion`: an AI agent that uses a course's syllabi, notes, and other materials to answer student questions with targeted, helpful answers.


```
> curl -N -X POST https://your-domain.com/studycompanion/chat \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "u123",
    "course": "Macroeconomics 201",
    "conversation_id": "a1b2c3",
    "message": "Can you explain the difference between nominal and real GDP?"
  }'


{
  "reply": "**Great question!**

**Nominal GDP** measures the total value of all goods and services produced in an economy using current prices during the time of measurement. It does not account for inflation or deflation.

**Real GDP**, on the other hand, adjusts for changes in the price level. It reflects the value of all goods and services at constant prices (from a base year), allowing economists to compare economic output over time without the distortion of price changes.

**In short:**\n- Nominal GDP = not adjusted for inflation\n- Real GDP = adjusted for inflation\n\nThis makes **Real GDP** a better indicator for tracking actual growth in an economy."
}

```


Neat, huh?
The agent can also answer follow-ups to previous questions which is _really_ where it shines:

```
> curl -N -X POST https://your-domain.com/studycompanion/chat \
  -H "Content-Type: application/json" \
{
  "user_id": "u123",
  "course": "Macroeconomics 201",
  "conversation_id": "a1b2c3",
  "message": "Got it. So if real GDP is more accurate for comparing economic performance, what causes it to rise or fall over time?"
}

{
  "reply": "**Great follow-up! Real GDP is a key measure of a country’s economic health, and several core factors influence whether it rises or falls over time.**

### 🔺 What Causes Real GDP to Increase?
1. **Increased Consumer Spending**: When households spend more on goods and services, businesses see more revenue, often leading to expansion and job creation.
2. **Business Investment**: Spending on new factories, machinery, and technology boosts productivity and output, fueling real GDP growth.
3. **Government Spending**: Public investments—such as on infrastructure, education, or defense—inject money into the economy, raising demand and output.
4. **Net Exports Rise**: If a country exports more than it imports, the resulting trade surplus contributes positively to GDP.
5. **Technological Innovation**: Better technology improves productivity, allowing the same inputs to yield more output.

### 🔻 What Causes Real GDP to Decline?
1. **High Inflation with Stagnant Wages**: When prices rise faster than incomes, people reduce spending, which can slow production.
2. **Rising Interest Rates**: Higher borrowing costs can reduce both consumer spending (e.g. on homes or cars) and business investment.
3. **Unemployment**: Fewer people working means less income and demand, which suppresses economic activity.
4. **External Shocks**: Events like pandemics, wars, or supply chain disruptions can severely impact production and trade.
5. **Decreased Consumer or Business Confidence**: When people or companies expect a downturn, they often cut back on spending and investment preemptively.

### 🧠 Summary:
Real GDP reflects the economy’s output adjusted for inflation. It rises when spending, investment, and productivity increase—and falls when demand weakens or shocks occur. Economists watch these trends to assess cycles of **growth, recession, or recovery**.

Would you like to explore how this connects to business cycles or unemployment next?"
}


```

While this is really slick, you quickly realize that the long responses _again_ lead to really high latency.

 
But, an async solution as you used for `examgenerator` won't cut it here, since your students expect real time results.

So, you opt for a **streaming response** -- words just appear in the UI on the fly, which makes it much less of a deal that it might take a minute or so for the full generation to complete.


For example, the first user query above would lead to streamed chunks that might look like:


```
> curl -N -X POST https://your-domain.com/studycompanion/chat/stream \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "u123",
    "course": "Macroeconomics 201",
    "conversation_id": "a1b2c3",
    "message": "Can you explain the difference between nominal and real GDP?"
  }'

data: **No
data: mina
data: l 
data: GDP**
data:  is 
data: the 
data: m
data: eas
data: ure 
data: of 
data: a 
data: country
data: ’
data: s 
data: ec
data: ono
data: mic 
data: ou
data: tput 
data: u
data: sing 
data: cu
data: rrent 
data: pr
data: ices.

...
```

This works great in the UI -- the words just appear magically.

BUT, you realize that it's really hard to determine during development when the agent cannot produce an adequate response due to missing course materials:

```
> curl -N -X POST https://your-domain.com/studycompanion/chat/stream \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "u123",
    "course": "Macroeconomics 201",
    "conversation_id": "a1b2c3",
    "message": "What are Prof Valdez's three big topics for exam 1?"
  }'

data: Let 
data: me 
data: sea
data: rch 
data: for 
data: rel
data: eva
data: nt 
data: mat
data: eri
data: als
data: . s
data: orr
data: y, 
data: it 
data: lo
data: oks 
data: lik
data: e 
```


You decide to revive your old `/chat` endpoint to iterate on agent responses.
```
{
  "reply": "Let me search for relevant materials... sorry, it looks like I don't have knowledge about that topic."
}
```

Hm, so the agent can't find the right materials... oh, you realize you forgot to index past exams for this course.
Easy fix.

Anyways, the vanilla `/chat` lets you look for your "sorry tokens" (`sorry`, `apologize`, etc.) _much_ faster than `/chat/stream` would allow.
So you continue to rely on the vanilla `/chat` endpoint during development, at some point rebranding it `/chat/qa`, and also realize pretty quickly that you can use it to **power your evals** as well.
I mean, as fun as it sounds to replicate your front-end stream chunk parser to your eval pipeline running in a separate CI pipeline, why not just keep outputs simple?
It's not like CI needs the same UI magic as your users, anyway.


### QA endpoints are your friend!

The examples above illustrate what we mean by QA endpoints, and why they make an AI Engineer's life easy.


But, I would be _remiss_ if you didn't ask:

#### ❓ Is adding and maintaining extra endpoints just for QA _really_ worth the engineering cost?

I argue: absolutely.

Especially if you set up your QA endpoints "correctly" in that they invoke 95%+ of the same logic as your Prod endpoints.
This can mean inheriting from the Prod endpoint class, abstracting core functionality into defs, you name it.

In fact, the secret reason that I push these so hard is that they force you to think through what the _meat_ of your endpoint is, and then add whatever prod bells and whistles you need to make things Prod-ready.
And then, your QA endpoint is just whatever it needs to be to let you easily and quickly verify AI or ML results, and establish parity with your data science experiments or intuition.
They're a minor footprint with high leverage.

Once this is all established, I promise you will admire the beauty of how this all looks in your API code.
And you'd be surprised by how often having 2 entryways into your core logic with separate end goals will help you catch bugs or issues that would otherwise fly under the radar.

_In production, APIs optimize for real-world constraints; in QA, we optimize for truth._
Separate QA endpoints let us be rigorous about these considerations.


#### ❓ Why not just write tests?

QA endpoints are _not_ a replacement for tests. 

A QA endpoint is a real path through production logic, testing real integration, not just unit concerns.
It lives in and interacts with the same environment that exposes your model.
Plus, you avoid the headaches and gotchas of dependency mocking.

They are a tool to increase velocity, and ideally make your API logic more robust.

But... it also depends on what you mean by tests.

I mentioned evals above.
But what about CI smoke tests that automatically ensure basic model quality is still there?
Seems like a great fit for your QA endpoint(s).

(And please, PLEASE still write tests.)



#### ❓ What about security? Aren't these extra QA endpoints by design missing the bells and whistles that make them production-safe?

That is a _great_ point.

But, there is an easy solution: Just disable them in Prod.
I mean, you are already strict about which endpoints you expose in Prod, right?
(If not, you really should be.)


#### ❓ Okay, I'm on board with endpoints. Why not just add a something like a `qa` flag to your actual endpoints?


Great questions!

I have two reasons for ya:

**Reason #1**: Security

Concerning our discussion about security above, grouping your QA code into your Prod endpoints removes the ability to cleanly toggle QA code off in Prod.
It's much more straightforward to disallow an endpoint name rather than a payload flag or HTTP header, and keeps intent intact.

Also, it increases the risk that new or unfamiliar devs accidentally switch QA on, and when your user starts seeing HTTP error payloads in the UI, we all lose.

**Reason #2**: Response Validation

The shapes of your Prod vs QA endpoints outputs are most likely very different: QA output is often just direct model results, whereas Prod output can be anything, really.

In both of our examples, the QA endpoints returned very different outputs compared to their Prod counterparts, whether it's an async job ID, streaming events, or whatever else.

And if you're using data validators for your endpoint payloads (you should be), things can get very tricky trying to support very different structures within the same endpoint.

Formalizing QA endpoints gives you freedom to define the shapes you need for productive QA.

#### ‼️  I get it now. All is clear. Thanks, BOS!

You got it, pal! 🤘

And just for the record, I really am not claiming that QA endpoints are a panacea to your ML engineering woes.

They certainly have their drawbacks -- the main one being the contrived friction to keep endpoints with different purposes / outputs / etc. in sync with each other.

And maybe this is just not worth it for your pipeline -- and that's totally fine.

But in a world where bugs can drive us crazy for days, or -- God forbid -- multiple sprints, any tools that might help us keep our sanity, and keep us shipping fast, are worth a shot.

Happy QA-ing!

