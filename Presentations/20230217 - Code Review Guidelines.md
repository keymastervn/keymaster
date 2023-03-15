
# Code Review Guidelines

A lightning talk about doing code review **properly**

![[Pasted image 20230217151601.png|300]]

--- 

## Why code review?
FAQ in many interviews: how do you think about code review?

> above code review ~> peer code review

--- 

### For knowledge sharing

A platform so A can navigate B through ideas with "right" or "wrong"

--- 

### For quality keeper 1/2
Most managers care about the output, means the **features** in term of functionalities/non-functionalities more than people.

--- 

### For quality keeper 2/2

Of course, it does. Super effective in a no QAs organisation.

- Spot out coding issues eg. conventions, race conditions
- Maintainability, someone won't feel f\*ck up when taking ownership
- Error prone on boundary cases
- Etc...

---
### The most organic way to build relationships in the office
As working on the same thing, trust me bro.

---
### To validate yourself
A life in fulfillment is a life in adequate verification

---
## Who involves in code review?

Of course, code `reviewer` and `reviewee`

---

# Code Review for Reviewer

![[Pasted image 20230217143643.png|400]]

---
## What is the code doing?

*It usually begins with a ticket.*

Ask yourself
- Do you have the right background info?
- Do you fully understand why this code is needed?
- Do you understand how this code fits into the project?

---
## Test the code

Ask yourself
- Does the code have appropriate unit-tests?
- Does CI generate warnings or errors?
- Does the code have any real results in staging?

--- 
## Inspect the code 1/2

Check and ask the owner
- Convention, styles
- Implementation matches to expectation
	- Changes that break existing features
	- Changes that block future enhancement
	- Changes that is repeated, not DRY

---
## Inspect the code 2/2

- Insights, *yes, everything can be insighted subjectively*
	- Well-design
	- Complexity
	- Consistency
	- Documentation in needed

--- 

# Code Review for Reviewee
Why not doing self-review first?

--- 

## Self-Review Checklist
*it depends on your domain: backend, frontend, infra, architect*

Let's take a [look](https://employmenthero.atlassian.net/wiki/spaces/DE/pages/2482962686/Self-Review+Checklist), TLDR;

---

## Self-elaborate Comments
<s>Show, don't tell</s>
Tell, don't ask

It's a sign that you care about your reviewer

--- 

# Ethnic
Don't be a bit too dark, software must be **happy ending** more than *drama*.

Rule 1: Always be constructive
Rule 2: Most of the time, you welcome replies as constructive feedback

--- 

## Represent your self
❌
This code block is not understandable
This code block is so confusing
✅
I can't understand this code block, could you please explain what's the purpose behind?
I am confused about the reason this line of code is placed here?

---

## Be rationale
❌
Backend is not ready yet so this will crash the system if you have it merged
✅
For this release, I'd suggest you have the backend PR merged first, then following up by this one
🌟(Better)
As the backend is not ready yet, how about adding a flag to control and release once it is ready? To me it is safer.

---

## Question not demand
❌
You are using hash which is not efficient, you will change to set, won't you?
✅
AFAIK, using a hash is neither a solid interface nor bad memory GC, the Set data structure is well supported for this kind of requirement.
🌟(Better)
Here is benchmark whereas Set is x% efficient in iops/mem, also less LOC. \`\`\`suggestion 

---

## Be objective
❌
You should use this function when getting to init the component.
✅
In order to handle errors better, let's use this function as it has rich content on errors so you can render it to the user, or treat the unhappy cases differently.

--- 

## Avoid judgemental words
❌
Function XYZ is obviously the same as ABC
✅
I believe function ABC can provide the same functionality could you check it, please?

---

## Expect misunderstandings
❌
I can't understand what this code does... totally confused! Why?
✅
Can you elaborate on this piece of code? I guess it yields the same result but I can't make sure of it.

---

# Be a part of the Culture
Code review is hard.
My code reviewer is a gift.

So, I will send it to another one, says the *Law of Cause and Effect*

---

# More reads
- [# How to write code review comments](https://google.github.io/eng-practices/review/reviewer/comments.html)
- [Awesome code review's good reads](https://github.com/joho/awesome-code-review#articles)
- [Sandi Metz' Rules For Developers](https://thoughtbot.com/blog/sandi-metz-rules-for-developers)

<style>
  /* To help export pages line by lines */
  @media print { hr { page-break-after: always; } }
</style>
