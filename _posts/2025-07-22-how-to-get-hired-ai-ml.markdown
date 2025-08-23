---
layout: post
title:  "How To Actually Land Your Next AI/ML Role in 2025"
date:   2025-07-22 7:00:00 -0600
---


> **TL;DR:** In this post, I:
> - discuss themes I navigated during my recent AI / ML job search
> - share strategies for interview expectations / real problems companies are dealing with
> - provide targeted advice for how to demonstrate your value and land your next role
>
> **Who this post is for**: AI/ML engineers navigating the current job market — whether you’re actively looking, passively curious, or just wondering what companies actually care about when hiring in 2025.


## 😱 The bad news: I was laid off last month.

I'm not gonna lie: it shook me.

I spent a full day being confused and existential — suddenly cut off from the work I loved, the stability my family relied on, and teammates I was close to.

I found myself questioning my value, my accomplishments, and my future.

Plus, I had no idea what to do next — and the clock was already ticking.


### But, this isn't a layoff post.

Priorities shift.
Plans change.
Even when you're doing your best work, the bigger picture can move in a sudden new direction that leaves you behind.

I knew layoffs were the name of the tech game, and I knew that my short-term doubts were just noise in my grand career trajectory.

So, after a brief self-pity-party, I was surprisingly eager to dust off my resume and see what cool AI/ML problems other teams were tackling. 

AI was still at the forefront of innovation, and that wouldn't change anytime soon.

What were the _juicy_ problems companies faced that required strong technical leadership to drive success?

#### Spoiler: I wasn't let down.

Handed the opportunity to engage directly with teams facing these challenges, I made a realization:
There are some strong commonalities across the industry in terms of AI / ML solutions companies need, as well as how they assess a candidate's ability to solve those needs.

And I have some thoughts on what you can do as the candidate to demonstrate that _you're_ the person to lead their AI ambitions and convert them to business value.

This post is a reflection on the current AI / ML market, my experiences with it, and lessons learned that might help others navigate similar situations.

### 🍋🥤 This is a lemonade post. 

Not just for me, but anyone looking to **land their next AI / ML role**.


## 📈 What Hiring Managers _Actually_ Want in 2025

Companies want an AI/ML engineer who can:

- Start with the customer, and build with **empathy**
- Design **highly effective AI/ML stacks** to meet real-world needs
- Operate as a **generalist** — equally comfy spiking out messy EDA notebooks as they are debugging in Prod
- Inspire **confidence** in what you build to stakeholders (including yourself!)
- Demonstrate how your AI / ML features will **help their business win**

If you’ve worked in AI/ML in the last decade, none of this should surprise you.

But, my key takeaway was that, to really stand out, you should further have a track record of building:
- End-to-end GenAI features that make smart use of LLMs
- Powerful RAG pipelines grounded in thoughtful knowledge management
- Full-stack, production-ready agents, equipped with memory, tools, auth, feedback — the whole package
- Strong evaluation frameworks for offline and online data
- Holistic workflows for users to define, organize, execute, and analyze tasks 

Clearly, there's industry-wide excitement as AI graduates from "ML can make good predictions" to "agents can _actually_ convert natural language into dozen-step workflows".

We knew this day would come, but how did it get here so _fast_?
And how are things still speeding along?
And how is anyone supposed to keep up with all of it?

There is a lot to know and master to show why you're the top candidate these companies demand.

Much of the tooling is barely a year old, sometimes months.
It’s moving fast.

That’s why I’ve put together this post: to help you focus on the right things, cut through the noise, and show teams why _you’re_ the person to build the future with.

But first, how do you even land interviews in the first place?

## 🤝 How I _Actually_ Got Interviews

It's easy, tempting, and even addicting to shotgun out applications, especially with features like LinkedIn's Easy Apply.
I've been there.

But in reality, my strongest leads were **relationship-driven**, whether it was direct outreach, high-signal referrals, or recruiter pings.

Build a rich network.
Nurture it.
Engage with it meaningfully and often.

You have a lot of great ideas.
Don't be afraid to write about them.
Posting to LinkedIn is intimidating at first, but try adventuring outside your comfort zone.
What might seem obvious to you could be an insightful blog post for someone else.

It can feel difficult to stand out in a sea of others equally interested in networking and growing their career.

Besides writing, my best advice here is to be curious and express genuine interest in others.
Build real relationships, and the rest will follow.



## 🧠 What you'll be grilled on

All themes in this section surfaced in more than one interview, for AI initiatives in highly diverse, essentially unrelated fields.

Think of this as a **study guide**, not an answer key — these are areas you’ll likely be grilled on.

### AI systems

Building AI features involves many layers: 
- ETL pipelines
- Model training and evaluation
- Model serving, monitoring, and ops
- Feedback and iteration

Be ready to analyze a business problem and break it down into the required systems.
And then be ready to deep dive into each system component.

The focus is still quite prominently on **GenAI**, so understand:
- LLMs, RAG, prompting best practices, and hallucination mitigation, guardrails, and evaluation.
- Text-processing basics, from tokenization, to embeddings (more on this below), to context windows.
- **context engineering** (e.g. smart chunking, strategic structuring, window budgeting), why it's _so hot right now_, and how to do it well.

Stay well-read on specific, more cutting edge techniques like RLHF, LoRA, and whatever paper came out last week making its rounds in the AI-verse.

Understand global vs local (per-customer / tenant-specific / federated) learning, and the deep tension of "how do I tailor things per customer?" vs "how do I keep things simple, and ensure data privacy + security is completely respected?"
Related to this is how to intelligently partition and serve per-customer models.

Understand how quantization, distillation, model pruning, and other tricks help you obtain **production-ready models**.
And know how to work with your production models, whether it's upgrading to new versions, [QA-ing the results]({% post_url 2025-05-19-the-case-for-qa-endpoints %}), or monitoring for issues like drift.

### Embeddings and retrieval

Effective retrieval drives the core of good AI / agentic systems: tool selection, knowledge lookup, memory, and any decision-making that requires more than if-statements.

This isn't a new paradigm.

In fact, ML-based retrieval has been around for quite some time.

I've been building retrieval methods for semantic search and adjacent techniques for the better part of the last decade.
Many others have been doing so for a lot longer.

Modern generative LLMs let us turbo-charge how retrieved results are used and surfaced, but without good retrieval, you don't have good AI.

This is why **embeddings are the secret sauce of modern AI**.

They let us transform messy, multidimensional relationships into crisp similarity problems.

This isn't just useful for agentic RAG: embeddings power recommendations, clustering, context compression, and more.

The most interesting problems I encountered during my job search involved leveraging embeddings for problems involving multi-layer, not-necessarily-tabular data relationships.
Think matching customer support tickets to knowledge base articles across different product lines, user types, and contexts - exactly the kind of messy problem that embeddings are well-equipped to solve.

You should:
- understand why embeddings are powerful at a very deep level (and be able to ELI5)
- appreciate how embeddings represent data, allowing for distance and similarity to solve real-world problems
- know how to put this into practice with robust pipelines and performant vector databases


### Agent architecture

If agents are in the job description, know how to design and ship them.

Here are some topics: 

- **Perception**: How your agent obtains / processes input to understand its objectives
- **Planning**: How your agent (powered by one or more LLMs) reasons about its objective, decomposes it into tasks, and decides on actions
- **Tools**: What they are, how your agent selects / calls them, API / auth considerations, human-in-the-loop guardrails
- **Memory**: Short-term vs long-term, knowledge base basics
- **Learning**: How your agent adapts to new information, from self-evaluation to RLHF
- **Multi-agent architectures**, and tradeoffs compared to single-agent
- How agents differ from **workflows**

### Evaluation

Evaluating AI features, especially agentic ones, was a topic brought up in every single interview. 

You might be familiar with my [pre-existing thoughts]({% post_url 2025-06-17-the-3t-framework-for-evals %}) here, and so I won't repeat myself too much.

I will say I was able to lean on the **3T framework** for _every single_ interview, regardless of problem domain or agentic application.
(And yes, agents were discussed for every interview, even if the role wasn't totally agent-focused, per se.)

As for more traditional ML evaluation, be sure to know your metrics, from [classification](https://en.wikipedia.org/wiki/Evaluation_of_binary_classifiers) to [retrieval](https://en.wikipedia.org/wiki/Evaluation_measures_(information_retrieval)). 

### Model selection / tradeoffs

Thinking through model selection and the corresponding tradeoffs is classic ML Engineering.
In fact, it is one of my favorite areas to discuss when conducting an interview myself, and I was heartened to see others leaning on it (and quite heavily).

Be prepared to give a verbal thesis on simplicity vs accuracy, and especially specific considerations regarding quality, latency, interpretability, cost, etc.

This is especially nuanced in the age of generative LLMs which come with their own slew of considerations.


### Error handling
How can you handle error situations?
How _should_ you handle them?

What are the types of errors you might obtain for an AI agent?
Can you cleanly break them down into logic errors, modeling errors, and potentially other categories?

Can you use existing knowledge or context to retry intelligently?

At what point should you just call it and return a generic error?

While not the sexiest questions to tackle, I assure you they are critical, and your interviewers probably agree.


## 🧩 Don't sleep on the fundamentals

### Leetcode ain't dead

To be honest, I thought AI killed the leetcode interview, but I was mistaken.

At least in my experience as of 2025, **Leetcode is very much alive**, and was a component of roughly half of my interview pipelines.

I was slightly worried being off the "LC grind" for so many years could be my demise.

But thankfully, I passed each round!
And I had quite a bit of fun with the problems, being addicted to big-O flash cards in a past life.

Leetcode grinding works, if you have the time, patience, and pain tolerance. 

If grinding isn't realistic, don't panic.
As long as you're spending good time solving meaningful AI / ML problems, you'll naturally have this mindset front and center.

My personal experience with ETL and evaluation pipelines were particularly useful for this, since they force you to reason through similar flavors of correct, efficient logic, data structures, and edge-case handling.

Nonetheless, I suggest you resist the urge to vibe code everything (i.e., writing code without a clear plan)

Understanding your core business logic is non-negotiable, and while vibe coding can be useful, holding on to a **computational state of mind** will continue to pay dividends in interviews and throughout your career.

### Classical stats / ML ain't dead

Despite all the AI hype, **fundamental statistical theory and reasoning** came up in each interview pipeline.
And I'm glad it did.

Even though contemporary models have become heavily abstracted, they are still powered by statistical and probabilistic gears, and understanding these gears will let you master the machine.

Think of it this way: when something goes wrong with your AI / ML feature, and you've ruled out basic logic or infra concerns, the next recourse is debugging a nondeterministic system which can be very painful.
But having a strong handle of your statistical theory will make this a much smoother process, and prove your value to your team.

Here's what actually came up in my interviews:

#### Know your modeling basics.

- Regression and classification should be two of your dearest friends.
- Understand data sampling implications, statistical distributions, unbiasedness, and even experimental design.
- Be ready to discuss feature engineering, data sampling implications, interpretability, accuracy metrics.
And how to relate all of this to stakeholders, and business impact.

  
#### Know your machine learning fundamentals

- Understand loss functions (squared loss, absolute loss, cross entropy — the more, the merrier!), and how they can be tailored to address specific modeling problems.
- Understand regularization, why it works intuitively, and when / how to actually apply it (theoretically and numerically).

#### Understand modern deep learning, deeply

- Understand transformers and attention, and be able to ELI5 and why they're a big deal over their predecessors.
- Even better, understand its technical and theoretical details (yes, I'm talking queries, keys, and values), as well as modern techniques to make it even better


## 🦉 How You Can Stand Out

Here I lay out some thoughts about my day-to-day engineering routine, mindset, and habits that I believe naturally helped me tackle interviews.

**Always start with the user / customer**.
Empathize with UX, make it magical, and show why this will lead to happy customers and good business.

**Document your architectural decisions**, especially tradeoffs. 
You'll be asked why you made certain choices and whether they were correct.
Be ready to answer both of these accurately and honestly.
Hiring managers want to see that you can navigate uncertainty, make informed decisions, and course correct as needed.

Similarly, **keep track of your wins and losses**, and demonstrate what your losses taught you.
Turns out, many other companies are hitting the same hurdles you've already faced, and they want someone who's already learned the hard lessons.

Don't just work on tasks, but **really understand the systems you're building** at a high and low level.
Chase down questions yourself, and write up your learnings.
Be curious about how things work, especially the peculiar or seemingly magical ones.
Challenge assumptions.
Break things.
Then make them unbreakable.

Finally, **sharpen your soft skills**.
I touched on this earlier, but it deserves emphasis.
Ultimately, teams want to hire people they actually want to work with, and this is a huge factor in your interviews.
I think EQ can be just as important as IQ for an AI/ML Engineering role, so practice being a friendly, empathetic, and curious colleague.

## 🧪 What Interviewers Got Right (And Improvement Ideas)

### Sneak peeks to elevate the conversation
My favorite interviews gave me context ahead of time, allowing me to quickly plan my approach before the actual meeting.

For example, for one interview, I was emailed a doc describing the general ML problem to be solved, giving me time to brainstorm before live coding in front of an audience with a ticking clock.

This was a genuinely enjoyable experience, so much so that I kept coding well after the interview ended.

I'd love to see this interview style gain more traction.

### AI Usage

Several interviews explicitly allowed the use of additional tools, including Google, Stack Overflow, and — you guessed it — AI.

At first, it felt like cheating.

But once I leaned in, it nicely mirrored how I actually work: using AI to turn well-defined goals and proper context into working code, fast.

In one interview, I was asked to design and implement a solution involving clustering and generative AI.
Rusty on my `sklearn`, I grabbed the API doc for `KMeans`, piped that into a ChatGPT prompt along with my dataset description and clear end goal, and had a working model ready to build on.

This saved me (and the interviewer!) precious minutes I’d otherwise spend squinting at syntax.

Of course, there's a time and place for AI in interviews.
For example, don't expect it for a Leetcode round.

But it’s encouraging to see interviews evolving to reflect how engineers actually solve problems: quickly, contextually, and with modern tools at their side.

### Clarify interview goals

Interviewers, please try your best to convey what your round aims to assess at the _very_ beginning.
The earlier and clearer the better.
Heck, drop a clarifying line or two in the calendar invite.

You're not giving the candidate an unfair advantage here.
You're setting them up for success so they don't study and prep for the wrong type of content.

For example, “Architecture Interview” can mean a dozen different things.
Be explicit about what you’re looking for.

If it’s how to do agentic tool routing, say so!

ML Microservices?
Great!

No point in making candidates guess on the spot, and losing valuable time aligning on objectives.

Instead, set the tone and contextualized the conversation.

### Move fast!

I had to pass up some otherwise very interesting roles simply because time ran out.

I know that hiring is _tough_: you're juggling multiple candidates, various stakeholders, and fragmented schedules.

Still, I'm curious how we could accelerate things, and maximize the collective bang-for-buck.

**Panel interviews** seem to work wonders here.
Maybe we can lean on more of these.

Moreover, tight coordination and fast communication are non-negotiable — especially when a candidate is juggling multiple offers.

If someone tells you they’re on a deadline, move.
Or you might miss your chance.


## 🥳 The good news: I landed a new role!

After 10 intense days of interviews, I was lucky to have a few great options to choose from.

And with them, something even more valuable: clarity.

I was able to carefully consider each opportunity and make an honest decision about my best path forward.

I knew what I wanted out of my next role.
I knew where I would make the greatest impact.

So I said yes to a "boomerang" offer, returning to my old home at Workday. 🪃

I was thrilled to learn my old team was tackling agentic problems similar to what I've been obsessed with this year.
And the challenges they faced were exactly what I was looking for.

A little ironic, maybe, but a fitting and satisfying ending to my job search journey nonetheless!

Regardless of the boomerang conclusion, I've enjoyed reflecting on the full, transformative experience across the various interview pipelines that led me to accepting my new role with conviction.
I hope I illustrated this experience in a way that can benefit others navigating similar waters.

## Conclusion

Unemployment.
Interviews.
Uncertain futures.


All of it is stressful and overwhelming.

But you're not alone.

Approach your job search with curiosity, confidence, and a builder's mindset.

Not only will it energize you, but it will make the experience less _work_ and more _fun_.

And there is a lot of fun to be had.
There's no shortage of interesting work out there, and fantastic teams eager to welcome talented folks like you on board.

I hope this post gives you a more complete picture of what real teams are looking for in their next AI / ML hire.
And while I can't give you all the answers (not that either of us would want that, right?), I can do my best to share my experience, highlight relevant focus areas, and guide you towards mastering the right things.
If you found this helpful — or have your own tips or interview stories — I’d love to hear them.

#### TL;DR for the road:
- **Build systems**, not just models.
- **Master embeddings** — the secret sauce of AI.
- **Hone your soft skills**: EQ matters as much as IQ.
- **Your past hurdles are your greatest asset**. Share the hard-won lessons.

Happy hunting.
You got this! 🙌
