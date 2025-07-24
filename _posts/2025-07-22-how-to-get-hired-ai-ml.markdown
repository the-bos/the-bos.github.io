---
layout: post
title:  "How to Get Hired as an AI / ML Engineer: Lessons from Getting Laid Off and Bouncing Back Stronger"
date:   2025-07-22 7:00:00 -0600
---


> **TL;DR:** In this post I discuss themes I navigated during my recent AI / ML job search, strategies for interview expectations / real problems companies are dealing with, and targeted advice how to demonstrate your value and land your next role.

> Who this post is for: This post is for AI/ML engineers navigating the current job market — whether you’re actively looking, passively curious, or just wondering what companies actually care about when hiring in 2025.


## 😱 The bad news: I was laid off earlier this month.

As someone who hates relinquishing control, I spent a full day being shaken, confused, and existential.

Shaken because I was suddenly detached from the work I was obsessed with, the stability my family and I were accustomed to, and the people at work I'd grown close to.

Confused because, well, isn't AI supposed to be _so hot right now_?
It seems like, for many companies, AI is _the_ highest priority as they race to capitalize on the promises that it will positively reform modern business — a promise I believe in wholeheartedly.

And existential because, to be honest, being laid off made me question my value, my accomplishments, and my future.
Plus, what the heck was I going to do next?
And never mind the fact that the clock was ticking!

Yet, layoffs can happen to anyone.

This marked the most unexpected, pivotal turn in my career.


### But, this isn't a layoff post.

I knew layoffs were the name of the tech game, and I knew that my short-term doubts were just noise in my grand career trajectory.

And so, by the next day I was strangely eager to dust off my resume, put myself back out there, and see what sort of awesome AI / ML things other teams were working on.
What were the juicy problems companies faced that required strong technical leadership to drive success?

**Spoiler: I wasn't let down.**

Not only did I get a front row seat to these challenges, but I made a realization:
There are some strong commonalities across the industry in terms of AI / ML solutions companies need, how they assess a candidate's ability to solve those needs, and how candidates can demonstrate that _they_ can solve these problems.


So, this post is a reflection on the current AI / ML market, my experiences with it, and lessons learned that might help others navigate similar situations.

### 🍋🥤 This is a lemonade post. 

#### For myself, but more importantly, for anyone looking to land their next AI / ML role.


## 📈 What the market demands (and why it's changing)


From what I gather, the market wants an AI / ML engineer who can:

- Understand and empathize with customer needs
- Translate those needs to a highly effective AI / ML stack
- Perform as an AI / ML generalist — equally comfy spiking out messy EDA notebooks as they are debugging AI services in Prod
- Give stakeholders (including yourself!) confidence in what you built
- Demonstrate how your AI / ML features help your business win

If you've been active in AI / ML over the last decade, these requirements should look very familiar.

But my key takeaway was that, to really stand out, you should further have a track record of building:
- Full-stack, production-ready agents, equipped with auth, memory, human-in-the-loop, user feedback, etc.
- Intricate RAG systems, often with a robust knowledge base component
- Strong evaluation frameworks on both offline and online data
- Holistic workflows for users to define, organize, execute, and analyze tasks 

Clearly, there is industry-wide enthusiasm of watching AI finally graduate from "ML can make good predictions" to "agents can _actually_ convert simple natural language into dozen-step workflows".

We knew this day would come, but how did it get here so _fast_?
And how are things still speeding along?
And how is anyone supposed to keep up with all of it?

Nonetheless, there is a lot to know and master to be the top candidate these companies demand.

To help cut through the noise, I discuss specific themes that surfaced across companies and roles, to help you understand what to focus on mastering if you want to land your next role.


## 🧠 Technical themes you'll be grilled on

These themes surfaced in more than one interview, for AI initiatives in highly diverse, essentially unrelated fields.

Treat these as areas where you're likely to be grilled.

### AI systems
From ETL, to training, to evaluation, be ready to analyze a problem and propose a sick solution
Understand global vs federated learning, and the deep tension of "how do I tailor things per customer?" vs "how do I ensure data privacy + security is completely respected?"

### Embeddings and retrieval

Retrieval is the core of good agentic systems.

I find it interesting that ML-based retrieval has been around for quite some time.
I've been building retrieval methods for semantic search, etc for the better part of the last decade.

But generative LLMs have given us a way to surface retrieved results in a clean natural language interface.

All in all, embeddings are the secret sauce of modern AI.

This is especially the case for agentic retrieval, but also for clustering, recommendation, fine-tuning, context compression, and so many other applications.

You should prioritize understanding why embeddings are powerful at a very deep level.
You should be able to rely on embeddings as a representation of data, allowing you to use distance and similarity to solve real-world problems in a shockingly clean mathematical formulation.

The most interesting problems I encountered during my job search involved leveraging embeddings for problems involving multi-layer, not-necessarily-tabular data relationships. 
I challenge you to pursue this topic with curiosity, as I think it can be a real game-changer for many companies facing these deep, messy problems, and can be handled with surprising elegance.

### Evals

(Shameless self-promotion warning!)

Evaluating AI features, especially agentic ones, was a topic brought up in every single interview. 

You might be familiar with my pre-existing thoughts here, and so I won't get too into the weeds here.

But I will say I was able to lean on the 3T framework for every. single. interview.
Regardless of problem domain or agentic application.

### Error handling
How do you handle error situations? Can you use existing knowledge or context to retry intelligently? When should you call it and just return an error?

### Model selection / tradeoffs
Be prepared to give a verbal thesis on simplicity vs accuracy, and especially specific considerations regarding quality, latency, interpretability, cost, etc.

## 🧩 Don't sleep on the fundamentals

### Leetcode is still alive and kicking

- I was worried being off the grind for so many years would be my demise
- But, as long as you're solving meaningful AI / ML problems, I think you'll naturally have this mindset front and center
- Needless to say, resist the urge to vibe code everything. Understanding your core business logic is non-negotiable.


### Understand modern deep learning, deeply

- Understand attention, and be able to ELI5
  - Better yet, understand its technical and theoretical details, and modern techniques to make it even better
- Understand loss functions (the more the merrier)


### Classical stats / ML isn't dead

- Know your stats fundamentals
  - E.g. confusion matrix quantities
- Know your machine learning fundamentals
- Understand data sampling implications, statistical distributions, unbiasedness, and even experimental design.


## 🦉 Tips and advice

- Document your architectural decisions, especially tradeoffs. Keep track of your wins and losses (and demonstrate what your losses taught you the hard way). This is great material to leverage during interviews. Turns out, many other companies are hitting the same hurdles you've hit yourself.
- Don't just work on tasks, but really understand the systems you're building at a high and low level. Be curious about how things work. Challenge assumptions. Poke at things until they break, then make things unbreakable.


## 🤝 What ultimately led to interviews

It's relatively easy to submit applications you see on LinkedIn, etc., as many as you come across that fit what you're looking for.
But in reality my strongest leads were relationship-driven, whether it was direct outreach, high-signal referrals, or recruiter pings.
It's never been more important to build a rich network, and critically, to maintain it well.
If you can focus on personability, relationship-building, and genuine interest in others, you'll find yourself surrounded my connections who care about you.


## Bonus: What I loved, and would love to see more of, from hiring pipelines

- Hands-on coding tasks
  - My favorite interviews gave me some small amount of "heads up" to gather context before the interview.
  - Think being emailed a doc describing the problem, giving me time to brainstorm before live coding in front of an audience
  - This was genuinely a thrilling experience, so much so that I found myself building on the code well after the interviewer was over :lol:
- Specific expectations per interview round
  - Interviewers, please try your best to convey what a given interview round is assessing at the very beginning
  - E.g., if you're looking for specifics on how to solve agentic tool-routing, please don't label it as a "Architectural Design" interview, or at least make the goal crystal clear.
  - Otherwise, your candidate will waste both of your time talking about service architecture when really you want to discuss agent-internal logic
- Greater efficiency
  - I know recruiting is difficult, especially when you juggle multiple candidates with the schedules of multiple busy stakeholders
  - But, I had to pass up several otherwise very interesting roles simply because time ran out
  - I'm curious how we could speed things along to really get maximum bang-for-buck from a handful of interviews rather than relying on separate signals from the many various stakeholders.
  - More panel interviews might be a good thing here.
  - And recruiters: if a candidate informs you of another offer, please communicate that with the relevant folks ASAP.
  - That way, you give both parties a shot at landing a deal, rather than leading to surprise, confusion, and disappointment.


## 🥳 The good news: I secured a fantastic offer within 2 weeks!

And yes, it happened to be a 🪃 offer back to my old home at Workday. 😉

I was thrilled to see my old team was tackling agentic problems similar to what I've been obsessed with.
And I had the opportunity to return, pop back in like I'd never left, and pretend that the previous half-year was but a fever dream.

My old boss called it destiny, and it's hard to argue against that.

BUT, boomerang aside,  what I want to focus on here is the full experience across multiple pipelines for drastically different companies that ultimately led to this offer and my decision to accept it.

## Conclusion

TODO