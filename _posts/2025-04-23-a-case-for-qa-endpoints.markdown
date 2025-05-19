---
layout: post
title:  "The Case for QA Endpoints: A Faster Way to Debug Production Models"
date:   2025-04-23 8:00:00 -0600
---

> **TL;DR**: In this post, I make the case for building dedicated QA endpoints — simplified, synchronous wrappers around your production logic — to dramatically speed up debugging and evaluation of AI/ML systems.
I motivate with two examples and explore when, why, and how to use them effectively.


## 📢 Has this ever happened to you? 📢


* You build a wonderful ML model in some ML-friendly environment
* You deploy that model to a new environment (Staging, Prod, etc)
* You go to sanity check the model is working as expected, and...

#### ...the results look... kinda _sus_? 👀


### You're not alone.

This happens to the best of us, and a key challenge of ML engineering: ensuring consistent model performance across environments.

I won't get into why this happens or how to achieve parity in _this_ post, but one thing's for sure:

We need a way to **iterate quickly** in order to get a model that performs well where it matters most: **Prod**.


## 🕵️ Case study: The `examgenerator` service

Let's say you're building a product to generate academic materials given some relevant context.
Your premier feature will be creating **multiple choice exams**, which tend to be the most time-consuming for instructors to create.

For example, for the following inputs:

```
Course Title: Introduction to Biology
Topic: Cell Structure
Number of questions: 20
```

your generated exam might look like:

```
1. Which of the following organelles is primarily responsible for generating ATP, the main energy currency of the cell?
a)  Nucleus
b)  Endoplasmic Reticulum
c)  Mitochondria
d)  Golgi Apparatus
Correct Answer: c)

2. The cell wall, a rigid outer layer, is a characteristic feature of which type of cell(s)?
a)  Animal cell
b)  Fungal cell
c)  Protozoan cell
d)  Bacterial cell
Correct Answers: b) and d)

...

20. What is the primary function of the cell membrane?
a)  To store the cell's genetic material.
b)  To synthesize proteins.
c)  To regulate the passage of substances into and out of the cell.
d)  To provide structural support to the cell.
Correct Answer: c)
```

There are some important features here to note:

1. Multiple-choice exams have a _very_ particular format: Numbered lists, with sub-lists, correct answers, and plenty of newlines.
2. Exams can be quite _long_, and might take a while to generate.

Because of (2), you opt for an **async flow**: the client calls your endpoint, providing the relevant model inputs as well as a `Callback-Url` header:

```
> curl -X POST \
  /examgenerator/create \
  -H 'Callback-Url: /some/client/url' \
  -d '{"course_title": "Introduction to Biology", \
    "course_topic": "Cell Structure", \
    "num_questions": 20
  }'

{"success": true, "callback-id": "abc-123"}

```

Then your service eventually POSTs the model output to the provided callback URL with the corresponding callback ID.

No timeouts or dropped connections. Just pure async magic!

Awesome!

You go to deploy your model to Staging, send your example curl w/ a dummy callback URL, and get your response with `"callback-id": "def-456"`.

Cool!

But what does the exam actually look like?

... Guess we'll check the service logs?

Since you know the model generation might take a while, you twiddle your thumbs for a bit, and when you think 20 seconds or so have passed, you open the logs:

```
some-service-controller get logs > logs.txt
```

Then open `logs.txt` and CTRL+F for `def-456`:

```
Model response for callback-id = def-456:
Question: Which of the following organelles is primarily responsible for generating ATP, the main energy currency of the cell?
Correct Answer: Mitochondria

Question: The cell wall, a rigid outer layer, is a characteristic feature of which type of cell?
Correct Answer: Fungal and bacterial cells.

...

```

Gah!
This doesn't look like a good multiple choice exam if there aren't any _choices_!

So we enter our dev flow: make changes to the model in `staging`, and send test curls, save log file to text, open file, ctrl+f, ...

Wait. There has to be a better way to do this.

### 🥁 Enter: the synchronous QA endpoint

You decide to make a new endpoint that takes in the exact same inputs, but just returns the model output synchronously:

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

And even though it might still take a while for the model results to surface synchronously, we've removed the main friction, giving us a much quicker, easier iteration loop until parity is achieved.

(A couple more iteration loops and we realize we accidentally swapped the `temperature` and `top_p` values in our inference engine... 🙃)


## 🕵️ Case Study: A real-time study assistant

Your `examgenerator` service was wildly successful!

So much so that you decide to build out another GenAI feature to solve another academic need: an around-the-clock study buddy.

Because let's face it: TAs have better things to do at 11 PM on a Friday than explain the nuances of contemporary macroeconomic policy to a stressed-out sophomore treating your e-mail like a 90's chat room.

Enter `studycompanion`: an AI agent that uses course syllabi, notes, and other materials to answer student questions with targeted, helpful answers.

You decide to put this agent behind a basic `/chat` endpoint that returns text responses based on certain inputs:

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

 
But, an async solution as you used for `examgenerator` won't cut it here, since your students expect real time results in the chat window.

So, you opt for a **streaming response** -- words just appear in the UI on the fly as the agent generates them, which makes it much less of a deal that it might take a minute or so for the full generation to complete.


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

This looks brilliant in the UI as the words appear in near real-time.

BUT, you soon realize that it's really hard to determine during development when the agent cannot produce an adequate response due to missing course materials:

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


You decide to revive your old `/chat` endpoint to iterate on agent responses in order to actually _read_ what the agent is saying:
```
{
  "reply": "Let me search for relevant materials... sorry, it looks like I don't have knowledge about that topic."
}
```

Hm, so the agent can't find the right materials... oh, you realize you forgot to index past exams for this course.
Easy fix!

Anyways, the vanilla `/chat` lets you look for your "sorry tokens" (`sorry`, `apologize`, etc.) as well as other text content _much_ faster than `/chat/stream` would allow.

So you continue to rely on the vanilla `/chat` endpoint during development, at some point rebranding it `/chat/qa`, and also realize pretty quickly that you can use it to _power evals_, too.

I mean, as fun as it sounds to replicate your front-end stream chunk parser to a separate back-end CI pipeline, why not just keep outputs simple?

It's not like CI needs the same UI magic as your users, anyway.


## 🤗 Why QA endpoints are your friends

The examples above illustrate what we mean by QA endpoints, and why they can make you a Happy Engineer™️.

More formally, a **QA endpoint** is a simplified, synchronous version of a production endpoint, exposing the core logic without the delivery complexity (async, streaming, side effects, etc.).

It exists to improve development and evaluation velocity, not to serve end users.

It’s a test harness that speaks HTTP, or whatever language is used to communicate with your models or services.

Here is a diagram that captures what this generally looks like:

<svg viewBox="0 0 800 780" xmlns="http://www.w3.org/2000/svg">
  <rect width="800" height="780" fill="none" rx="10" ry="10"/>

  <text x="400" y="40" font-family="Arial" font-size="24" text-anchor="middle" font-weight="bold">Shared Components: Production vs. QA Endpoints</text>

  <!-- Legend -->
  <rect x="620" y="70" width="20" height="20" fill="#e3f2fd" stroke="#2196f3" stroke-width="2"/>
  <text x="650" y="85" font-family="Arial" font-size="14">Production Endpoint</text>
  <rect x="620" y="100" width="20" height="20" fill="#e8f5e9" stroke="#4caf50" stroke-width="2"/>
  <text x="650" y="115" font-family="Arial" font-size="14">QA Endpoint</text>
  <rect x="620" y="130" width="20" height="20" fill="#fff3e0" stroke="#ff9800" stroke-width="2"/>
  <text x="650" y="145" font-family="Arial" font-size="14">Shared Components</text>

  <!-- Client -->
  <rect x="325" y="75" width="150" height="60" fill="#eeeeee" stroke="#9e9e9e" stroke-width="2" rx="5" ry="5"/>
  <text x="400" y="110" font-family="Arial" font-size="16" text-anchor="middle" font-weight="bold">Client</text>

  <!-- Updated POST arrows -->
  <path d="M 350 135 L 350 175" stroke="#2196f3" stroke-width="3" marker-end="url(#arrowBlue)"/>
  <text x="330" y="155" font-family="Arial" font-size="14" text-anchor="end">POST /api/v1</text>

  <path d="M 450 135 L 450 175" stroke="#4caf50" stroke-width="3" marker-end="url(#arrowGreen)"/>
  <text x="470" y="155" font-family="Arial" font-size="14" text-anchor="start">POST /api/v1/qa</text>

  <!-- Shared Components -->
  <rect x="200" y="175" width="400" height="360" fill="#fff3e0" stroke="#ff9800" stroke-width="3" rx="10" ry="10"/>
  <text x="400" y="200" font-family="Arial" font-size="18" text-anchor="middle" font-weight="bold">Shared Components</text>

  <rect x="250" y="220" width="300" height="50" fill="#ffe0b2" stroke="#ff9800" stroke-width="2" rx="5" ry="5"/>
  <text x="400" y="250" font-family="Arial" font-size="16" text-anchor="middle" font-weight="bold">Request Handling / Validation</text>

  <rect x="250" y="305" width="300" height="50" fill="#ffe0b2" stroke="#ff9800" stroke-width="2" rx="5" ry="5"/>
  <text x="400" y="335" font-family="Arial" font-size="16" text-anchor="middle" font-weight="bold">Pre-processing</text>

  <rect x="250" y="390" width="300" height="50" fill="#ffe0b2" stroke="#ff9800" stroke-width="2" rx="5" ry="5"/>
  <text x="400" y="420" font-family="Arial" font-size="16" text-anchor="middle" font-weight="bold">Prompting</text>

  <rect x="250" y="475" width="300" height="50" fill="#ffe0b2" stroke="#ff9800" stroke-width="2" rx="5" ry="5"/>
  <text x="400" y="505" font-family="Arial" font-size="16" text-anchor="middle" font-weight="bold">Model Invocation</text>

  <path d="M 400 270 L 400 305" stroke="#ff9800" stroke-width="2" marker-end="url(#arrowOrange)"/>
  <path d="M 400 355 L 400 390" stroke="#ff9800" stroke-width="2" marker-end="url(#arrowOrange)"/>
  <path d="M 400 440 L 400 475" stroke="#ff9800" stroke-width="2" marker-end="url(#arrowOrange)"/>

  <!-- Production & QA Responses -->
  <path d="M 325 525 L 225 565" stroke="#2196f3" stroke-width="3" marker-end="url(#arrowBlue)"/>
  <path d="M 475 525 L 575 565" stroke="#4caf50" stroke-width="3" marker-end="url(#arrowGreen)"/>

  <rect x="100" y="565" width="250" height="95" fill="#e3f2fd" stroke="#2196f3" stroke-width="3" rx="10" ry="10"/>
  <text x="225" y="590" font-family="Arial" font-size="16" text-anchor="middle" font-weight="bold">Production Response</text>
  <rect x="115" y="600" width="220" height="45" fill="#bbdefb" stroke="#1976d2" stroke-width="2" rx="5" ry="5"/>
  <text x="225" y="627" font-family="Arial" font-size="14" text-anchor="middle">Response Handling / Streaming</text>

  <rect x="450" y="565" width="250" height="95" fill="#e8f5e9" stroke="#4caf50" stroke-width="3" rx="10" ry="10"/>
  <text x="575" y="590" font-family="Arial" font-size="16" text-anchor="middle" font-weight="bold">QA Response</text>
  <rect x="475" y="600" width="200" height="45" fill="#c8e6c9" stroke="#388e3c" stroke-width="2" rx="5" ry="5"/>
  <text x="575" y="627" font-family="Arial" font-size="14" text-anchor="middle">Direct Response Return</text>

  <!-- JSON Boxes (slightly lowered) -->
  <rect x="190" y="685" width="70" height="80" fill="#e3f2fd" stroke="#2196f3" stroke-width="2" rx="5" ry="5"/>
  <text x="225" y="705" font-family="Arial" font-size="10" text-anchor="middle" font-weight="bold">JSON</text>
  <text x="225" y="720" font-family="monospace" font-size="8" text-anchor="middle">{"id":"abc",</text>
  <text x="225" y="735" font-family="monospace" font-size="8" text-anchor="middle">"status":</text>
  <text x="225" y="750" font-family="monospace" font-size="8" text-anchor="middle">"pending"}</text>
  <path d="M 225 645 L 225 685" stroke="#2196f3" stroke-width="2" marker-end="url(#arrowBlue)"/>

  <rect x="540" y="685" width="70" height="80" fill="#e8f5e9" stroke="#4caf50" stroke-width="2" rx="5" ry="5"/>
  <text x="575" y="705" font-family="Arial" font-size="10" text-anchor="middle" font-weight="bold">JSON</text>
  <text x="575" y="720" font-family="monospace" font-size="8" text-anchor="middle">{"text":</text>
  <text x="575" y="735" font-family="monospace" font-size="8" text-anchor="middle">"Direct</text>
  <text x="575" y="750" font-family="monospace" font-size="8" text-anchor="middle">response"}</text>
  <path d="M 575 645 L 575 685" stroke="#4caf50" stroke-width="2" marker-end="url(#arrowGreen)"/>

  <!-- Arrow markers -->
  <defs>
    <marker id="arrowBlue" markerWidth="10" markerHeight="10" refX="9" refY="3" orient="auto" markerUnits="strokeWidth">
      <path d="M0,0 L0,6 L9,3 z" fill="#2196f3"/>
    </marker>
    <marker id="arrowGreen" markerWidth="10" markerHeight="10" refX="9" refY="3" orient="auto" markerUnits="strokeWidth">
      <path d="M0,0 L0,6 L9,3 z" fill="#4caf50"/>
    </marker>
    <marker id="arrowOrange" markerWidth="10" markerHeight="10" refX="9" refY="3" orient="auto" markerUnits="strokeWidth">
      <path d="M0,0 L0,6 L9,3 z" fill="#ff9800"/>
    </marker>
  </defs>
</svg>

This structure keeps the vast majority of the logic shared, while letting each endpoint optimize for its delivery context.

And depending on the exact use case, you might be able to get away with common post-processing logic as well.


#### What might this look like in code?

Let's take our `examgenerator` use case.

Our core exam generator logic might look something like:

```python
EXAM_GENERATOR_PROMPT = """
Generate {n} multiple-choice exam questions given the following course title and topic.
Course title: {title}
Course topic: {topic}
"""

class ExamGeneratorCore:
    def __init__(self, model_client):
        self.model_client = model_client

    def _postprocess_output(self, text: str) -> str:
        # normalize formatting, scrub PII, content moderation, etc.
	...

    def generate_exam(self, course_title: str, topic: str, num_questions: int = 15) -> str:
        prompt = EXAM_GENERATOR_PROMPT.format(title=course_title, topic=topic, n=num_questions)
        raw_output = self.model_client.generate(prompt)
        return self._postprocess_output(raw_output)

```

Then our endpoints might look like:

```python
from fastapi import APIRouter, BackgroundTasks, Depends
from pydantic import BaseModel
from uuid import UUID, uuid4

from your_api import GenerateExamRequest
from your_core import ExamGeneratorCore
from your_model_client import ModelClient

router = APIRouter()

class GenerateExamAsyncResponse(BaseModel):
    success: bool
    callback_id: UUID

class GenerateExamQaResponse(BaseModel):
    text: str

def get_exam_generator() -> ExamGeneratorCore:
    return ExamGeneratorCore(model_client=ModelClient())

def run_exam_generation(
    req: GenerateExamRequest,
    core: ExamGeneratorCore
) -> str:
    return core.generate_exam(
        course_title=req.course_title,
        topic=req.course_topic,
        num_questions=req.num_questions,
    )

@router.post("/examgenerator/create", response_model=GenerateExamAsyncResponse)
async def generate_exam_async(
    req: GenerateExamRequest,
    background_tasks: BackgroundTasks,
    core: ExamGeneratorCore = Depends(get_exam_generator)
) -> GenerateExamAsyncResponse:
    callback_id = uuid4()

    def send_callback():
        result = run_exam_generation(req, core)
        # Send result to callback URL...

    background_tasks.add_task(send_callback)
    return GenerateExamAsyncResponse(success=True, callback_id=callback_id)

@router.post("/examgenerator/create/qa", response_model=GenerateExamQaResponse)
async def generate_exam_qa(
    req: GenerateExamRequest,
    core: ExamGeneratorCore = Depends(get_exam_generator)
) -> GenerateExamQaResponse:
    result = run_exam_generation(req, core)
    return GenerateExamQaResponse(text=result)

```


While I've been using LLM-powered systems as examples (they are my home-field advantage, after all), the QA endpoint pattern is just as useful for any complex API where the real response is delayed, streamed, or transformed before delivery, from batch scoring pipelines to analytics exporters.

**So, QA endpoints are your friends, regardless of the systems you're building!**

But, I would be _remiss_ if you didn't ask:

#### ❓ Is adding and maintaining extra endpoints just for QA _really_ worth the engineering cost?

In my experience, _absolutely_.

Especially if you set up your QA endpoints "correctly" to invoke 95%+ of the same logic as your Prod endpoints, as illustrated with the diagram and code example above.

In fact, the secret reason that I push these so hard is that they force you to think through what the _meat_ of your endpoint is, then add only the necessary bells and whistles you need to make things Prod-ready.

And then, your QA endpoint can be exactly what it needs to be for quick-n-easy verification of results, until parity with your data science experiments or intuition is established.
It's a minor footprint with high leverage.

A well-factored system makes QA endpoints cheap to add.
They’re a side effect of good separation between core logic and production scaffolding — not a hack layered on top.

Once this is all established, I promise you will admire the beauty of how this all looks in your API code.
And you'd be surprised by how often having 2 entryways into your core logic with separate end goals will help you catch bugs or issues that would otherwise fly under the radar.

_In production, APIs optimize for real-world constraints; in QA, we optimize for truth._
Separate QA endpoints let us be rigorous about these considerations.

#### ❓ What if the core logic can’t be easily reused?

So far I've assumed that the core logic is easily extractable, but in many situations, this isn’t true.
State management, middleware, side effects, auth, retries, etc. are often interwoven into business logic, sometimes for the worse.

_If your core logic is hard to expose via a QA endpoint, that’s a smell._

QA endpoints don’t just help test better — they expose where your code could be cleaner.



#### ❓ Why not just write tests?

QA endpoints are _not_ a replacement for tests. 

A QA endpoint is a real path through production logic, testing actual integration, not just unit concerns.
It lives in and interacts with the same environment that exposes your model.
Plus, you avoid the headaches and gotchas of dependency mocking.

It is a tool to increase velocity, and fortify your API logic.

But... it also depends on what you mean by tests.

I mentioned evals above.
But what about CI smoke tests that automatically ensure basic model quality is still there?
Seems like a great fit for your QA endpoint(s).

(And please, PLEASE still write tests!!)


#### ❓ As your org or product grows, does the QA endpoint model break down?

_Who owns the QA endpoints?_

_Are they formally tested themselves?_

_How do you avoid them going stale?_

Here is my take:

QA endpoints scale well when you treat them jointly as:

* another test harness: lightweight, composable, and easy to refactor alongside the systems they wrap
* testable features in their own right: equipped with smoke, integration, and e2e tests to ensure they continue serving their correct purpose

You should have observability for them in terms of dashboards, logging, and even alerts for good measure.

You should test both happy and unhappy paths to ensure your error handling is sensible and consistent with your Prod endpoints.

All in all, like any dev tool, they work best when used intentionally, and pruned when no longer needed.
(Though in my experience, my colleagues and I have found good utility in keeping them around for the long haul.)

In other words, cross that bridge when you need to.
It's a good problem to have if the QA endpoints that got you from 0 to 1 ultimately hang in the balance as your rocket takes off. 🚀



#### ❓ What about security and customer data privacy?

This is one of the trickiest parts of QA endpoints, and where a simple “just disable in prod” argument falls apart.

There’s a real tension here:

* On one hand, you need production-like access to customer-trained models to understand their behavior and debug issues effectively.
* On the other, these models often handle sensitive customer data, and QA endpoints -- by design -- may lack the full scaffolding of your production safety mechanisms.

The goal, then, is not to avoid QA endpoints in prod-tier environments, but to build a layered, controlled access model that lets the right people debug the right things, without opening the door to data leaks, security holes, or audit nightmares.

And while you don't want to hinder the dev velocity you're so excited to achieve, due diligence is required here.

Here’s how we approach this in practice:

✅ **Layer 1: Environment Selection**

Customer-trained models should only exist in approved high-tier environments (e.g. staging, prod).

Lower environments (e.g. dev) should use off-the-shelf / public models (preferably of the same architecture as your prod models) where no customer data is in play.
This allows safe iteration and smoke testing before you ever touch real data.

There is a good chance you already have something like this for your Prod endpoints, so just follow suit with the QA counterparts.

✅ **Layer 2: Network Access Control**

Network-level isolation is your strongest shield.

QA endpoints should be inaccessible from the public internet, period. Restrict them to your VPC, VPN, or internal service mesh. This instantly shuts out casual or accidental misuse.

✅ **Layer 3: Authentication, Authorization, and Routing**

QA endpoints often need to live in prod environments, and that means they must be tightly locked down.

Access should require strong authentication and authorization — not spoofable origin headers (though these can work as a temporary workaround if you're still wiring up proper auth).

In addition, your infrastructure (e.g. API Gateway or ingress controller) should explicitly allow traffic to `/qa` endpoints only from trusted paths or identities, such as internal dashboards or specific service roles.

Think of this as your blast radius limiter: even if QA code ships to prod, only authenticated, pre-approved clients can reach it.


✅ **Layer 4: Data Privacy Within the Endpoint**

This one’s easy to overlook.

If your QA endpoint uses production data, it must also use production-grade data handling: masking, redaction, anonymization — whatever safeguards your prod stack applies to PII or sensitive content.

It’s tempting to say “it’s just for internal use,” but QA paths are just as capable of leaking data if mishandled. They should inherit your data hygiene standards, not bypass them.


**Bottom line**

QA endpoints can be responsibly implemented, even for customer-trained models, when they’re protected with layered controls: environment boundaries, network rules, authentication, route restrictions, and proper data hygiene.

They don’t have to be risky. But they do have to be deliberate.

And yes, this adds surface area. QA endpoints must evolve with your systems, and that includes evolving security posture alongside them.

If your network topology changes, your auth system is upgraded, or new data flows are added to your models, QA endpoint controls must stay in sync.

It’s a small but worthwhile price to pay for the ability to debug production behavior safely and quickly.


#### ❓ Okay, I'm on board with endpoints. But why not just add a something like a `qa` flag to your actual endpoints?


Great question!

I have two reasons for ya:

**Reason #1**: Security

Concerning our discussion about security above, grouping your QA code into your Prod endpoints removes the ability to cleanly toggle QA code as needed in higher-tier environments.
It's much more straightforward to disallow an endpoint name rather than a payload flag or HTTP header, and keeps intent intact.

Also, it increases the risk that new or unfamiliar devs accidentally switch QA on, and when users start seeing HTTP error payloads in the UI, we all lose.

**Reason #2**: Response Validation

The nature of your Prod vs QA endpoint outputs are most likely very different: QA output is just direct model results, whereas Prod output can be anything, really.

And if you're using data validators for your endpoint payloads (you should be), things can get very tricky trying to support very different structures within the same endpoint.

Having separate QA endpoints gives you freedom to define the outputs that fit your debugging needs, without battling your production constraints.

### ‼️  I get it now. All is clear. 🧘 Thanks, BOS!

You got it, pal! 🤘

And just for the record, I really am not claiming that QA endpoints are a panacea to all your ML engineering woes.

They certainly have their drawbacks -- the main one being the contrived friction to keep endpoints with different purposes / outputs / etc. in sync with each other.

And maybe this is just not worth it for your needs -- and that's totally fine.

But in a world where bugs can drive us crazy for days, or -- God forbid -- multiple sprints, any tools that might help us **keep our sanity**, and keep us **shipping fast**, are worth a shot.

Happy QA-ing! 🧑‍🔬

