---
layout: post
title:  "A Case for QA Endpoints"
date:   2025-04-23 8:00:00 -0600
---

### Has this ever happened to you?


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

Because of (2), you opt for an async flow: the client calls your endpoint, providing the relevant model inputs as well as a callback URL:

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

You go to deploy your model to a staging env, send your example curl w/ a dummy callback, wait 20 seconds, and get your response with `"callback-id": "def-456".

Cool... guess we'll check the service logs?

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
This doesn't look like a good exam.
We're giving away the correct answers!

So we enter our dev flow: make changes to the model in staging, and send test curls, save log file to text, open file, ctrl+f, ...

Wait. There has to be a better way to do this.

### Enter: the synchronous QA endpoint.

You decide to make a new endpoint that just returns the model output synchronously:

```
> echo -e $(curl -X POST \
  /examgenerator/create/qa \
  -d '{"course_title": "Introduction to Biology", \
    "course_topic": "Cell Structure", \
    "num_questions": 20
  }')

```

