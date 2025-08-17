---
layout: post
title:  "How To Land Your Next AI / ML Role"
date:   2025-07-22 7:00:00 -0600
---


> **TL;DR:** In this post, I:
> - discuss themes I navigated during my recent AI / ML job search
> - share strategies for interview expectations / real problems companies are dealing with
> - provide targeted advice for how to demonstrate your value and land your next role.

> Who this post is for: This post is for AI/ML engineers navigating the current job market — whether you’re actively looking, passively curious, or just wondering what companies actually care about when hiring in 2025.


## 😱 The bad news: I was laid off last month.

I'm not gonna lie: it shook me.

I spent a full day being confused and existential.

I was suddenly detached from the work I was obsessed with, the stability my family and I were accustomed to, and the people at work I'd grown close to.

I found myself questioning my value, my accomplishments, and my future.

Plus, I had no idea what to do next — and the clock was already ticking.

**Yet, layoffs can happen to anyone.**

Priorities shift.
Plans change.
Even when you're doing your best work, the bigger picture can move in a sudden new direction that leaves you behind.


### But, this isn't a layoff post.

I knew layoffs were the name of the tech game, and I knew that my short-term doubts were just noise in my grand career trajectory.

And so, by the next day I was surprisingly eager to dust off my resume, put myself back out there, and see what sort of awesome AI / ML things other teams were working on.

AI was still at the forefront of innovation, and that wouldn't change anytime soon.

So, I began to wonder: what were the _juicy_ problems companies faced that required strong technical leadership to drive success?

**Spoiler: I wasn't let down.**

Not only did I enjoy a front row seat to these challenges, but I made a realization:
There are some strong commonalities across the industry in terms of AI / ML solutions companies need, as well as how they assess a candidate's ability to solve those needs.

And I have some thoughts on what you can do as the candidate to demonstrate that _you're_ the person to lead their AI ambitions and convert them to business value.

So, this post is a reflection on the current AI / ML market, my experiences with it, and lessons learned that might help others navigate similar situations.

### 🍋🥤 This is a lemonade post. 

For not only myself, but anyone looking to **land their next AI / ML role**.


## 📈 What Hiring Managers _Actually_ Want in 2025

Companies want an AI / ML engineer who can:

- Understand and empathize with **customer needs**
- Translate those needs to a **highly effective AI/ML stack**
- Perform as a **generalist** — equally comfy spiking out messy EDA notebooks as they are debugging AI services in Prod
- Give stakeholders (including yourself!) **confidence** in what you built
- Demonstrate how your AI / ML features will **help their business win**

If you've been active in AI / ML over the last decade, these requirements should look very familiar.

But, my key takeaway was that, to really stand out, you should further have a track record of building:
- End-to-end GenAI features that leverage LLMs in a smart, effective, and efficient way
- State-of-the-art retrieval systems (e.g. RAG), often with a robust knowledge base component
- Full-stack, production-ready agents, equipped with auth, memory, human-in-the-loop, user feedback, etc.
- Strong evaluation frameworks on both offline and online data
- Holistic workflows for users to define, organize, execute, and analyze tasks 

Clearly, there is industry-wide enthusiasm of watching AI finally graduate from "ML can make good predictions" to "agents can _actually_ convert simple natural language into dozen-step workflows".

We knew this day would come, but how did it get here so _fast_?
And how are things still speeding along?
And how is anyone supposed to keep up with all of it?

Nonetheless, there is a lot to know and master to show why you're the top candidate these companies demand.

And to make things even more difficult, a lot of the required technology has only been around for a very short while — mere months, in some cases.

**But don't worry. I'm here to help!**

To cut through the noise, I discuss specific themes that surfaced across companies and roles, to help you understand what to focus on mastering if you want to land your next role.

But first, how do you even land interviews in the first place?

## 🤝 How I _Actually_ Got Interviews

It's surprisingly easy (borderline addicting) to continually submit applications for roles you come across on LinkedIn, etc. — as many as you can find that match what you're looking for.

But in reality, my strongest leads were **relationship-driven**, whether it was direct outreach, high-signal referrals, or recruiter pings.

I won't repeat that one platitude, but it seems to still hold water.

Still, it's never been more important to build a rich network, and critically, to engage with it meaningfully and often.

It can feel difficult to stand out in a sea of others equally interested in networking and growing their career.

I'm no social media expert or psychologist, but here's my advice:
focus on personability, relationship-building, and genuine interest in others, and you'll find yourself surrounded by connections who you care about, and who care about you.
And the rest will follow naturally.



## 🧠 What you'll be grilled on

All themes in this section surfaced in more than one interview, for AI initiatives in highly diverse, essentially unrelated fields.

Treat these as areas on which you're likely to be grilled.

You surely won't hit every point below exactly as I did, but I'm aiming for completeness here, so treat this as a **study guide** rather than an answer key.

### AI systems

There are many components and pipelines involved in a legit, end-to-end AI / ML feature: ETL, model training, evaluation, model serving, monitoring, feedback, etc.

Be ready to analyze a business problem and break it down into the required systems.
And then be ready to deep dive into each system component.

The focus is still quite prominently on **GenAI**, so understand:
- LLMs, RAG, prompting best practices, and hallucination mitigation, guardrails, and evaluation.
- the basics of how text gets processed, from tokenization, to embeddings (more on this below), to context windows.
- **context engineering**, why it's _so hot right now_, and how to do it well.

I further suggest reading up on specific, more cutting edge techniques like RLHF, LoRA, and whatever paper came out last week making its rounds in the AI-verse.

Understand global vs local (per-customer / tenant-specific / federated) learning, and the deep tension of "how do I tailor things per customer?" vs "how do I keep things simple, and ensure data privacy + security is completely respected?"
Auxiliary to this is how to intelligently partition and serve per-customer models.

Understand how quantization, distillation, model pruning, and other tricks help you obtain **production-ready models**.
And know how to work with your production models, whether it's upgrading to new versions, [QA-ing the results]({% post_url 2025-05-19-the-case-for-qa-endpoints %}), or monitoring for issues like drift.

### Embeddings and retrieval

Effective retrieval drives the core of good AI / agentic systems, from tool selection to knowledge lookup to really any decision-making that requires more than if-statements.

And make no mistake: this isn't a new paradigm.
In fact, ML-based retrieval has been around for quite some time.

I've been building retrieval methods for semantic search and adjacent techniques for the better part of the last decade, and many others have been doing so for a lot longer.

Modern generative LLMs let us turbo-charge how retrieved results are used and surfaced, but without good retrieval, you don't have good AI.

All in all, **embeddings are the secret sauce of modern AI**.

This not agentic retrieval (which gets the spotlight these days), but also for clustering, recommendation, fine-tuning, context compression, and so many other applications.

You should:
- prioritize understanding why embeddings are powerful at a very deep level
- understand embeddings as a representation of data, allowing you to use distance and similarity to solve real-world problems in a shockingly clean mathematical formulation.
- know how to put this into practice with robust embedding creation pipelines and performant vector databases.

The most interesting problems I encountered during my job search involved leveraging embeddings for problems involving multi-layer, not-necessarily-tabular data relationships. 
I challenge you to pursue this topic with curiosity, as I think it can be a real game-changer for many companies facing these deep, messy problems, and can be handled with surprising elegance.

### Agent Architecture

If AI agents are in the job description, you'll want to know how to design, build, and productionize them.

Here are some topics: 

- **Perception**: How your agent obtains / processes input to understand its objectives
- **Planning**: How your agent (powered by one or more LLMs) reasons about its objective, decomposes it into tasks, and decides on actions
- **Tools**: What they are, how your agent selects / calls them, API / auth considerations, HitL guardrails
- **Memory**: Short-term (conversation) vs long-term (knowledge base) 
- **Learning**: How your agent adapts to new information, from self-evaluation to RLHF
- **Multi-agent architectures**, and tradeoffs compared to single-agent
- How agents differ from **workflows**

### Evaluation

Evaluating AI features, especially agentic ones, was a topic brought up in every single interview. 

You might be familiar with my [pre-existing thoughts]({% post_url 2025-06-17-the-3t-framework-for-evals %}) here, and so I won't repeat myself too much.

But I will say I was able to lean on the **3T framework** for _every single_ interview.
Regardless of problem domain or agentic application. (And yes, agents were discussed for every interview, even if the role wasn't totally agent-focused, per se.)

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
And I had quite a bit of fun with the problems.
They made me feel like a young, starry-eyed student again, mesmerized by algorithmic beauty and addicted to big-O flash cards.

My take here: grinding out leetcode problems is a great way to prepare for these interviews, if you have the time, patience, and pain tolerance. 

But if you can't commit to the grind, I think as long as you're spending good time solving meaningful AI / ML problems, you'll naturally have this mindset front and center.

My personal experience with ETL and evaluation pipelines were particularly useful for this, since they force you to reason though similar flavors of correct, efficient logic, data structures, and edge-case handling.

Nonetheless, I suggest you resist the urge to vibe code everything.

Understanding your core business logic is non-negotiable, and while vibe coding can be useful, holding on to a **computational state of mind** will continue to pay dividends in interviews and throughout your career.

### Classical stats / ML ain't dead

Regardless of the job title I applied for, **fundamental statistical theory and reasoning** showed up in some form or another.
And I'd be remiss if it didn't.

Even though contemporary models have become heavily abstracted, they are still powered by statistical and probabilistic gears, and understanding these gears will let you master the machine.

Think of it this way: when something goes wrong with your AI / ML feature, and you've ruled out basic logic or infra concerns, the next recourse is debugging a nondeterministic system which can be very painful.
But having a strong handle of your statistical theory will make this a much smoother process, and prove your value to your team.

So, what should you focus on?

While most of us don't have time to brush up on the equivalent of a 4-year Statistics degree, I'll share some concepts that came up for me.

#### Know your modeling basics.

- Regression and classification should be two of your dearest friends.
- Understand data sampling implications, statistical distributions, unbiasedness, and even experimental design.
- Be ready to discuss feature engineering, data sampling implications, interpretability, accuracy metrics.
And how to relate all of this to stakeholders, and business impact.

  
#### Know your machine learning fundamentals

- Understand loss functions (the more, the merrier), and how they can be tailored to address specific modeling problems.
- Understand regularization, why it works intuitively, and when / how to actually apply it (theoretically and numerically).

#### Understand modern deep learning, deeply

- Understand transformers and attention, and be able to ELI5 and why they're a big deal over their predecessors.
- Even better, understand its technical and theoretical details (yes, I'm talking queries, keys, and values), as well as modern techniques to make it even better


## 🦉 How You Can Stand Out

Here I lay out some thoughts about my day-to-day engineering routine, mindset, and habits that I believe naturally helped me tackle interviews.

**Always start with the user / customer**. Empathize with UX, make it magical, and show why this will lead to happy customers (and profit!)

**Document your architectural decisions**, especially tradeoffs. 
You'll be asked why you made certain choices and whether they were correct.
Be ready to answer both of these accurately and honestly.
Hiring managers want to see that you can navigate uncertainty, make informed decisions, and course correct as needed.

Similarly, **keep track of your wins and losses**, and demonstrate what your losses taught you.
This is _great_ material to leverage during interviews.
Turns out, many other companies are hitting the same hurdles you've hit yourself.
They want you to bring and distill your lessons learned, which ultimately cuts them the cost of learning them manually.

Don't just work on tasks, but **really understand the systems you're building** at a high and low level.
I know this is easier said than done, but my advice is to give yourself capacity to be able to chase down questions yourself, and write up your learnings.
Be curious about how things work, especially the peculiar or apparently-magical ones.
Challenge assumptions.
Poke at things until they break, then make them unbreakable.

Finally, **sharpen your soft skills**.
I touched on this earlier, but it deserves emphasis.
Ultimately, teams want to hire people they actually want to work with, and this is a huge factor in your interviews.
I think EQ can be just as important as IQ for an AI/ML Engineering role, so practice being a friendly, empathetic, and curious colleague.

## 🧪 What Interviewers Got Right (And Improvement Ideas)

### Hands-on coding tasks
My favorite interviews gave me some small amount of "heads up" to gather context and quickly plan my approach before the actual meeting began.

For example, for one interview, I was emailed a doc describing the general ML problem to be solved, giving me time to brainstorm before live coding in front of an audience with a ticking clock.

This was genuinely a thrilling experience, so much so that I found myself extending the code well after the interviewer was over! 😆

### Specific expectations per interview round

Interviewers, please try your best to convey what a given interview round aims to assess at the _very_ beginning.
The earlier and clearer the better.
Heck, drop a clarifying line or two in the calendar invite.

You don't need to be so specific as to give the candidate an unfair advantage, but why not set them up for success when otherwise they'll just be studying and preparing for the wrong type of content?

For example, if you're looking for specifics on how to solve agentic knowledge lookup, please make the goal crystal clear at the very beginning (and avoid tagging it with vague titles like "Architecture Design Interview").
Otherwise, your candidate will waste both of your time talking through unnecessarily high-level arch diagrams, sequence flow, etc. when really you want to discuss agent-specific internal logic.

### Greater efficiency

I had to pass up some otherwise very interesting roles simply because time ran out.

I know recruiting is difficult, especially when you're juggling multiple candidates with the schedules of multiple busy stakeholders.

But it seems odd to me that speed is still an issue here, in a world that has never moved faster.

I'm curious how we could accelerate things so that time isn't what kills your offer.

How can we maximize the collective bang-for-buck from a smaller set of interviews rather than relying on separate signals from the many various stakeholders across a fragmented schedule?

**Panel interviews** seem to work wonders here.
Maybe we can lean on more of these.

And recruiters: if a candidate informs you of another offer, _please_ communicate that with the relevant folks ASAP, especially if the offer is time-sensitive. 
That way, you give both parties a shot at closing the deal, rather than leading to surprise, confusion, and mutual disappointment.


## 🥳 The good news: I landed a new role!

After 10 intense days of interviews, I was lucky to have a few great options to choose from.

And as it turns out, I ultimately accepted a 🪃 offer back to my old home at Workday. 😉

I was thrilled to learn my old team was tackling agentic problems similar to what I've been obsessed with this year.
And luck turns out I had the opportunity to pop back in like I'd never left, and pretend that the previous half-year was but a fever dream.

My manager-again called it destiny, and it's hard to argue against that.

But, boomerang aside, I've enjoyed reflecting on the full experience across multiple serious interview pipelines that led me to accepting my new role.
And I hope I illustrated this experience in a way that can benefit others navigating similar waters.

It was a thrill of a ride (especially the late-stage negotiation phase), and a blessing to have landed on my feet in an environment that I love and thrive in.

## Conclusion

Unemployment is stressful.
Interviews are stressful.
Uncertain futures are stressful.

But I encourage you to approach your job search with curiosity, enthusiasm, and confidence.

Not only does it lock in a powerful mindset, but it makes the experience less _work_ and more _fun_.

And there is a lot of fun to be had.
There's no shortage of interesting work out there, and exciting teams eager to welcome talented folks like yourself on board.

I hope this post gives you a more complete picture of what real teams are looking for in their next AI / ML hire.
And while I can't give you all the answers (not that either of us would want that, right?), I can do my best to share my experience, highlight relevant focus areas, and guide you towards mastering the right things.

So, remember:
- Build systems, not just models.
- Embeddings are the secret sauce of AI.
- EQ matters as much as IQ.
- And your past challenges? They’re someone else’s roadmap, and you can steer the ship.

Happy hunting.
You got this! 🙌
