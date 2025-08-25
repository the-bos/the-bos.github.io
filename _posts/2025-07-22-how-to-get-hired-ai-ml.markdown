---
layout: post
title:  "How To Actually Land Your Next AI/ML Role in 2025"
date:   2025-07-22 7:00:00 -0600
---


> **TL;DR:**
> 
> In this post, I discuss themes I navigated during my recent AI/ML job search, demystify interview expectations / real problems companies currently face, and provide targeted strategies for how to land your next role with confidence.
>
> **Who this post is for:**
> 
> AI/ML engineers navigating the current job market — whether you’re actively hunting, passively curious, or just wondering what companies actually care about in 2025.


## 😱 The bad news: I was laid off last month.

I'm not gonna lie: it shook me.

I spent a full day being confused and existential, suddenly cut off from the work I loved, the stability my family relied on, and teammates I was close to.

I found myself questioning my value, my accomplishments, and my future.

Plus, I had no idea what to do next — and the clock was already ticking.


### But, this isn't a layoff post.

Priorities change.
Plans fall apart.

Even when you're doing your best work, the bigger picture can suddenly shift directions, leaving you behind.

I knew layoffs were the name of the tech game, and I knew that my short-term doubts were just noise in my grand career trajectory.

So, after a brief self-pity-party, I was surprisingly eager to dust off my resume and see what cool AI/ML problems other teams were tackling. 

AI was still at the forefront of innovation, and that wouldn't change anytime soon.

What were the juicy problems companies faced, and how could an engineer like me help?

#### Spoiler: I wasn't let down

Talking with teams actively tackling these challenges, I realized something:

There are **strong patterns** across the industry — both in the AI/ML problems companies face, and how they assess whether candidates can solve them.

So I’ve organized some thoughts to help you spot these patterns, master the solutions, and turn tough interview pipelines into real offers.

What started as a layoff could’ve been just a sour chapter in my story.

Instead, it became an enlightening experience.

So, yeah —

### 🍋🥤 This is a lemonade post. 

Not just for me, but anyone looking to **land their next AI/ML role**.

This post is a reflection on today’s AI/ML market, the lessons I took away, and practical takeaways to help others navigate similar waters.


## 📈 What Hiring Managers _Actually_ Want in 2025

Companies want an AI/ML engineer who can:

- Start with the customer, and build with **empathy**
- Design **highly effective AI/ML stacks** to meet real-world needs
- Operate as a **generalist** — equally comfy spiking out messy EDA notebooks as they are debugging in Prod
- Inspire **confidence** in what they're building to stakeholders (including themselves!)
- Demonstrate how these AI/ML features will **help their business win**

If you’ve worked in AI/ML in the last decade, none of this should surprise you.

But, my key takeaway was that, to _really_ stand out, you should further have a track record of building:
- End-to-end **GenAI features** that make smart use of LLMs
- Powerful **RAG pipelines** grounded in thoughtful knowledge management
- Full-stack, production-ready **AI agents**, equipped with memory, tools, auth, feedback — the whole package
- Strong **evaluation frameworks** for offline and online data
- **Holistic workspaces** for users to define, organize, execute, and analyze their tasks 

Clearly, there's industry-wide excitement as AI graduates from "ML can make good predictions" to "agents can _actually_ convert natural language into dozen-step workflows".

We knew this day would come, but how is it already here?

Plus, much of the tooling is barely a year old, sometimes months.
It’s moving fast.

How is anyone supposed to keep up with all of it?

That's why I wrote this: to highlight the right things, cut through the noise, and help you show teams why _you’re_ the person to build the future with.

But before we get into all that — how do you even _land_ interviews in the first place?

## 🤝 How I _Actually_ Got Interviews

It's easy, tempting, and even addicting to shotgun out applications, especially with features like LinkedIn's Easy Apply™.
I've been there.

But in reality, my strongest leads were **relationship-driven**, whether it was direct outreach, strong referrals, or recruiter pings.

Keep building a rich network.
Engage with it meaningfully and often.

For example, we all have lots of great stories and ideas.

Don't be afraid to **write about them**!

Posting to your social network can be intimidating at first, but take that step out of your comfort zone — it only gets easier from there.

What might seem obvious to you could be a lightbulb moment for someone else.

It can feel difficult to stand out in a sea of others equally interested in networking and growing their career.

So share the unique perspective that got you where you are.
Be a voice others look forward to hearing.

Further, be curious and express genuine interest in others.
Build real relationships, and the rest will follow.



## 🧠 What You'll Need to Know

All themes in this section surfaced in multiple interviews, for AI initiatives in diverse, essentially unrelated fields.

Treat these as areas you’ll likely be grilled on.

Think of this as a **study guide**, not an answer key.

### AI systems

Building AI features involves many layers, such as: 
- ETL pipelines
- Model training and evaluation
- Model serving, monitoring, and ops
- Feedback and iteration

Be ready to analyze a business problem and break it down into the required components.
And then be ready to deep dive into each component.

The focus is still quite prominently on **GenAI**, so understand:
- LLMs, RAG, prompting best practices, hallucination mitigation, guardrails, and evaluation.
- Text-processing basics: tokenization, embeddings (more on this below), context windows.
- **Context engineering** (e.g. smart chunking, strategic structuring, window budgeting), why it's top of mind for every company, and how to do it well.
  - Hint: Start with [this article](https://cognition.ai/blog/dont-build-multi-agents?ref=blog.langchain.com) that kicked off the conversation.

Stay well-read on specific, more cutting edge techniques like RLHF, edge AI, and whatever paper came out last week making its rounds in the AI-verse.

Understand global vs local (per-customer / tenant-specific / federated) learning — and the tension between tailoring intelligently vs keeping things simple and privacy-safe.
Related to this is how to skillfully partition and serve per-customer models.

Understand how quantization, distillation, model pruning, and other tricks help you obtain **production-ready models**.
And know how to work with your production models, whether it's upgrading versions, [QA-ing the results]({% post_url 2025-05-19-the-case-for-qa-endpoints %}), or monitoring for issues like drift.

And be sure to understand specific sub-domains for the role. (For example, computer vision will require its own set of fundamental skills.)


### Embeddings and retrieval

Effective retrieval drives the core of good AI / agentic systems: tool selection, knowledge lookup, memory, and any decision-making that requires more than if-statements.

This isn't a new paradigm.

In fact, ML-based retrieval has been around for quite some time.

I've been building retrieval methods for semantic search and adjacent techniques for the better part of the last decade.
Many others have been doing so for a lot longer.

Modern generative LLMs let us turbo-charge how retrieved results are used and surfaced, but without good retrieval, you don't have good AI.

This is why **embeddings are the secret sauce of modern AI**.

They let us transform messy, multidimensional relationships into crisp similarity problems, and hence powerful retrieval methods.

This isn't just useful for agentic RAG: embeddings power recommendations, clustering, query understanding, and more.

The most interesting problems I encountered during my job search involved leveraging embeddings for problems involving multi-layer, not-necessarily-tabular data relationships.
Think matching customer support tickets to knowledge base articles across different product lines, user types, and contexts — exactly the kind of messy problem that embeddings are well-equipped to solve.

You should:
- understand why embeddings are powerful at a very deep level (and be able to ELI5)
- appreciate how embeddings represent data, allowing for distance and similarity to solve real-world problems
- know how to put this into practice with robust pipelines, performant vector databases, and service integration


### Agent architecture

If agents are in the job description, know how to design and ship them.

Here are some common topics to master for this:

- **Perception**: How your agent obtains / processes input to understand its objectives
- **Planning**: How your agent (powered by one or more LLMs) reasons about its objective, decomposes it into tasks, and decides on actions
- **Tools**: What they are, how your agent selects / calls them, API / auth considerations, human-in-the-loop guardrails
- **Memory**: Short-term vs long-term, knowledge bases, conversation profiling
- **Learning**: How your agent adapts to new information, from self-evaluation to RLHF
- **Multi-agent architectures**, and tradeoffs compared to single-agent
- How agents differ from **workflows**

This merely scratches the surface of this new and exciting field, but having a solid handle here will put you in good shape.

### Evaluation

Evaluating AI/ML, especially agentic features, was a topic brought up in every single interview I had. 

You might be familiar with my [pre-existing thoughts]({% post_url 2025-06-17-the-3t-framework-for-evals %}) here, and so I won't repeat myself too much.

I will say I was able to lean on the **3T framework** for _every single_ interview, regardless of problem domain or agentic application.
(And yes, agents came up in every interview, even when the role wasn't strictly agent-focused.)

If you want another framework to help, keep in mind the **3 Rs**: you want to evaluate against a
- **r**ich dataset (lots of data!)
- of **r**ealistic examples (looks like actual data!)
- that are **r**epresentative of your full user base

As for more traditional ML evaluation, be sure to know your metrics, from [classification](https://en.wikipedia.org/wiki/Evaluation_of_binary_classifiers) to [retrieval](https://en.wikipedia.org/wiki/Evaluation_measures_(information_retrieval)). 

### Model selection / tradeoffs

Thinking through model selection and the corresponding tradeoffs is classic ML Engineering.

Be prepared to give a verbal thesis on simplicity vs quality, and especially specific considerations regarding accuracy, latency, interpretability, cost, etc.

This is especially nuanced in the age of generative LLMs which pushes these concerns to extremes.


### Error handling
How can you handle error situations?
How _should_ you handle them?

What are the types of errors you might obtain for an AI agent?
Can you cleanly break them down into logic errors, modeling errors, and potentially other categories?

Can you use existing knowledge or context to retry intelligently?

At what point should you just call it and return a generic error?

While not the sexiest questions to tackle, I assure you they are critical, and your interviewers probably agree.


## 🧩 Don't sleep on the fundamentals

### LeetCode ain't dead

To be honest, I thought AI killed the LeetCode interview. I was mistaken.

Even in 2025, **LeetCode is very much alive**, and was a component of roughly half of my interview pipelines.

I was worried being off the "LC grind" for so many years could be my demise.

Turns out, my personal experience building ETL and evaluation pipelines were particularly useful here, since such pipelines force you to reason through similar flavors of correct (and efficient) logic, data structures, and edge-case handling.

I even had quite a bit of fun with the problems, having been addicted to big-O flash cards in a past life.

LeetCode grinding works, provided you have the time, patience, and pain tolerance.

If grinding isn't realistic, don't panic.
As long as you're spending good time solving meaningful AI/ML problems, you'll naturally have this mindset front and center.

Nonetheless, I suggest you resist the urge to [vibe code](https://en.wikipedia.org/wiki/Vibe_coding) everything.

Understanding your core business logic is non-negotiable, and while vibe coding can be useful, holding on to a **computational state of mind** will continue to pay dividends in interviews and throughout your career.


### Classical stats / ML ain't dead

Despite all the AI hype, **fundamental statistical theory and reasoning** came up in each interview pipeline.

Even though contemporary models have become heavily abstracted, they are still powered by statistical and probabilistic gears, and understanding these gears will let you master the machine.

When your RAG system starts hallucinating, you'll diagnose with bias-variance thinking.
When your agent's tool selection gets wonky, you'll need to understand precision/recall tradeoffs.

#### Know your modeling basics

- Know regression and classification inside and out.
- Understand data sampling implications, statistical distributions, unbiasedness, and even experimental design.
- Understand loss functions (squared vs absolute loss, cross entropy — the more, the merrier!), and how they can be tailored to address specific modeling problems.
- Understand regularization, why it works intuitively (hint: think bias-variance tradeoff), and when / how to actually apply it.
- Be ready to discuss feature engineering, interpretability, and performance metrics.
- And tie all of this to stakeholders and business impact.

#### Understand modern deep learning, deeply

- Understand transformers and attention, why they beat their predecessors, and be able to ELI5.
- Understand their technical and theoretical details (yes, I'm talking queries, keys, and values), as well as modern techniques to make it even better
- Understand fine-tuning and cutting-edge techniques like LoRA / PEFT and DPO


## 🌟 How You Can Stand Out

Here I lay out some thoughts about my day-to-day engineering routine, mindset, and habits that came in clutch come interview time.

**Always start with the user / customer**.
Empathize with UX, make it magical, and show why this will lead to happy customers and good business.

**Document your architectural decisions**, especially tradeoffs. 
And regularly review these docs.
You'll be asked why you made certain choices and whether they were correct.
Be ready to answer both of these accurately and honestly.
Hiring managers want to see that you can navigate uncertainty, make informed decisions, and course-correct as needed.

Similarly, **keep track of your wins and losses**, and demonstrate what your losses taught you.
Turns out, many other companies are hitting the same hurdles you've already faced, and they want someone who's already learned the hard lessons.

Don't just check off tasks on a sprint board, but **really understand the systems you're building** at a high and low level.
Chase down questions yourself, and write up your learnings.
Be curious about how things work, especially the peculiar or seemingly magical ones.
Challenge assumptions.
Break things.
Then make them unbreakable.

Finally, **sharpen your soft skills**.
I touched on this earlier, but it deserves emphasis.
Ultimately, teams want to hire people they actually want to work with.
I think EQ can be just as important as IQ for an AI/ML Engineering role, so practice being a friendly, empathetic, and curious colleague.

## 🧪 What Interviewers Got Right (And Improvement Ideas)

### Sneak peeks to elevate the conversation
My favorite interviews gave me **context ahead of time**, allowing me to quickly plan my approach before the actual meeting.

For one interview, I was emailed a doc 15 minutes in advance describing the general ML problem to be solved, giving me time to brainstorm before live coding in front of an audience and ticking clock.

This was a genuinely enjoyable experience, so much so that I kept coding well after the interview ended.

I'd love to see this interview style gain more traction.

### AI usage

Several interviews explicitly allowed the use of additional tools, including Google, Stack Overflow, and — you guessed it — AI.

At first, it felt like cheating.

But once I leaned in, I realized it nicely mirrored how I actually work: using AI to turn well-defined goals and proper context into working code, fast.

In one interview, I was asked to design and implement a solution involving clustering and generative labeling.
Rusty on my `sklearn`, I grabbed the API doc for `KMeans`, piped that into a ChatGPT prompt along with my dataset description and clear end goal, and instantly had a working model ready to build on.

This saved me (and the interviewer!) precious minutes I’d otherwise spend squinting at syntax.

Of course, there's a time and place for AI in interviews.

For example, don't expect it for a LeetCode round.

But it’s encouraging to see interviews evolving to reflect how engineers actually solve problems: quickly, contextually, and with modern tools at their side.

### Clarify interview goals

Interviewers, please try your best to convey what your round aims to assess at the _very_ beginning.
The earlier and clearer the better.
Heck, drop a clarifying line or two in the calendar invite.

You're not giving the candidate an unfair advantage here.
You're setting the interview up for success.

For example, “Architecture Interview” can mean a dozen different things.
Be explicit about what you’re looking for.

If it’s how to do agentic tool routing, say so!
ML microservices?
Great!

No point in making candidates guess on the spot, losing valuable time aligning on objectives.

Instead, set the tone and contextualize the conversation.

### Move fast!

I had to pass up some otherwise very interesting roles simply because time ran out.

I know that hiring is _tough_: you're juggling multiple candidates, various stakeholders, and fragmented schedules.

Still, I'm curious how we could accelerate things and maximize the collective bang-for-buck.

**Panel interviews** seem to work wonders here.
Maybe we can lean on more of these.

Moreover, tight coordination and fast communication are non-negotiable — especially when a candidate is juggling multiple offers.

If someone tells you they’re on a deadline, take action!
Or you might miss your chance.


## 🥳 The good news: I landed a new role!

After 10 intense days of interviews, I was lucky to have a few great options to choose from.

And with them, something incredibly valuable: **clarity**.

I was able to carefully consider each opportunity and make an honest decision about my best path forward.

I knew what I wanted out of my next role.
I knew where I would make the greatest impact.

So I said yes to a **boomerang offer**, returning to my old home at Workday. 🪃

I was thrilled to learn my old team was tackling agentic problems similar to what I've been obsessed with this year.
And the challenges they faced were exactly what I was looking for.

A little ironic, maybe, but a fitting and satisfying ending to my job search journey nonetheless!

Regardless of the boomerang conclusion, I've enjoyed reflecting on the full, transformative experience across the various interview pipelines that led me to accepting my new role with conviction.

I hope I illustrated this experience in a way that can benefit others navigating their own unique job searches.

## 🏁 Conclusion

Unemployment.
Interviews.
Uncertain futures.


All of it is stressful and overwhelming.

But you're not alone.

Approach your job search with **curiosity, confidence, and a builder's mindset**.

It'll energize you, making the whole process less _work_ and more _fun_.

And there is a lot of fun to be had.
There's no shortage of interesting work out there, and fantastic teams eager to welcome talented folks like you on board.

I hope this post gives you a more complete picture of what real teams are looking for in their next AI/ML hire.

And while I can't give you all the answers (not that either of us would want that, right?), I can do my best to share my experience, highlight relevant focus areas, and guide you towards mastering the right things.

If you found this helpful — or have your own tips or interview stories — I’d love to hear them!

#### TL;DR for the road:
- **Build systems**, not just models.
- **Focus on how you think**, not just what you’ve built.
- **Master embeddings** — the secret sauce of AI.
- **Hone your soft skills**: EQ matters as much as IQ.
- **Your past hurdles are your greatest asset**. Share the hard-won lessons.

Happy hunting.
You got this! 🙌
