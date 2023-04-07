---
theme: gaia
paginate: true
transition: fade
style: |
  /* Generic image styling for number icons */
  img:is([alt="1"], [alt="2"], [alt="3"], [alt="4"], [alt="5"], [alt="6"]) {
    height: 64px;
    position: relative;
    top: -0.1em;
    vertical-align: middle;
    width: 64px;
  }
---

![bg](https://i.makeagif.com/media/4-06-2016/DNG21q.gif)


----

# Today's agenda

![1](https://icongr.am/material/numeric-1-circle.svg?color=666666) re:Planning a migration
![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Pattern: Strangler fig pattern
![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Pattern: UI composition
![4](https://icongr.am/material/numeric-4-circle.svg?color=666666) Pattern: Branch by abstraction
![5](https://icongr.am/material/numeric-5-circle.svg?color=666666) Q/A

----
<!-- _class: lead -->

![1 w:256 h:256](https://icongr.am/material/numeric-1-circle.svg?color=ff9900)

# re:Planning a migration

-----
## re:Planning a migration

Ain't better with microservices, it is just an option.
GOAL: Independent deployability

"Big-bang migration" ❌  
"Incremental rewrites" ✅

----
## Approach

Not necessarily creating a new service, we can
- Cut-copy-reimplement
	- Not always a clear cut
	- Reimplement it first, if not at least we have a rollback point
- Refactoring: introduce "seam" concept from Working Effectively with Legacy Code: we can change the behaviour without edit the existing behaviour
	- A modular monolith: good enough?


----
<!-- _class: lead -->

![2 w:256 h:256](https://icongr.am/material/numeric-2-circle.svg?color=ff9900)

# Pattern: Strangler fig pattern

----

### Definition

Martin Fowler wrote an article titled “Strangler Application” in mid 2004 (and “Strangler Fig Application” from early 2019).

![bg left contain](./resources/20230405_strangler_fig.png)


----

![bg contain](./resources/20230405_strangler_fig_tree.png)

Start the migration from "trunk", the body, the huge strangler figs that *slowly* grow into fantastic and beautiful shapes, meanwhile strangling and killing the tree that was their host

-----
### Steps

![strangler_fig_step w:730 h:300](./resources/20230405_strangler_fig_steps.png)

Pros:
- Co-exist with the legacy
- Reversible

----
### Example 1: HTTP Revert Proxy (here dedicated proxy ~ NGINX)

![strangler_fig_step w:375 h:450](./resources/20230405_strangler_fig_example-httpproxy.png)


----
### Example 1: Service Mesh

No central proxy layer: each service has a "local proxy", configured specifically for comms to downstream services

![strangler_fig_service_mesh w:400 h:400](./resources/20230405_strangler_fig_example1_servicemesh.png)

-----

### Example: Message Interception

![strangler_fig_queue](./resources/20230405_strangler_fig_content-based-queue.png)


----
#### Content-based routing
- Intercept all messages intended for the downstream monolith
- Filter the messages on to the appropriate location

![strangler_fig_queue w:750 h:430](./resources/20230405_strangler_fig_content-based-queue_done.png)

-----
#### Selective consumption
A Message Selector

![strangler_fig_queue](./resources/20230405_strangler_fig_selective_consumption.png)

-----
#### Selective consumption

Pros:
- Reduce the need to create an additional queue
Cons:
- Not well supported to route specific "channel" to dedicated subscribers
- Need to change producers as well as changing subscribers -> stop the call

Content-based routing approach is likely to make more sense 👎



-----
### Changing behavior while migrating functionality

An issue with Stranger fig pattern: Adding new functionality while migrating. The longer the migration takes, the harder it can be to enforce a “feature freeze” in this part of the system.

⚠️ try to eliminate changes when migrating functionalities OR lose the ability to rollback

----
<!-- _class: lead -->

![3 w:256 h:256](https://icongr.am/material/numeric-3-circle.svg?color=ff9900)

# Pattern: UI composition

----
### Page-based composition

![bg left fit](./resources/20230405_ui_composition_page-based.png)

Split vertical-to-vertical basis
- Example: The guardian

----
### Widget-oriented composition

Split by widget, component
- ESI (Edge-Side Includes)

![ui_composition_widget h:300 w:800](./resources/20230405_ui_composition_widget.png)

----
### Micro-frontends
SPA: React, Angular, Vue,… → 1 html template (1 page)

- Problem of micro-frontend?
- How we can adopt micro-frontend?

![ui_composition_widget h:300 w:625](./resources/20230405_ui_composition_mfe.jpeg)


----
<!-- _class: lead -->

![4 w:256 h:256](https://icongr.am/material/numeric-4-circle.svg?color=ff9900)

# Pattern: Branch by abstraction

----

### Problem

Strangler pattern need us to intercept calls to our monolith to work.

❓What if the functionality we want to extract is deeper inside the existing monolith?

Here are some approaches

----
### Branching in source version control

With a long-live branch, painful 👎
- Conflicts when merge back to mainline 
- Hard to revert

----
### Branching by abstraction pattern

![bba_abstraction_1 w:800 h:480](./resources/20230405_bba_abstraction_1.png)

----
### Step 1 - Create an abstraction for the functionality to be replaced

![bba_step_1 w:400 h:400](./resources/20230405_bba_step1.png)

----
### Step 2- Change clients of the existing functionality to use the new abstraction

![bba_step_2 w:800 h:400](./resources/20230405_bba_step2.png)

----
### Step 3- Create a new implementation of the abstraction with the reworked functionality

The new implementation will call to our new microservice.

![bba_step_3 w:450 h:400](./resources/20230405_bba_step3.png)

----
### Step 4- Switch over the abstraction to use our new implementation


![bba_step_4_1 w:400 h:400](./resources/20230405_bba_step4_1.png)

----

![bba_step_4_2 w:400 h:400](./resources/20230405_bba_step4_2.png)

----
### Step 5 - Clean up the abstraction and remove the old implementation


![bba_step_5 w:450 h:400](./resources/20230405_bba_step5_1.png)

----
![bba_step_5_2 w:450 h:400](./resources/20230405_bba_step5_2.png)

----

### As a Fallback Mechanism

A variation of the branch by abstraction pattern called _**Verify branch by abstraction**_

![bba_verify w:400 h:350](./resources/20230405_bba_verifybba.png)

----

The downside is that **adds some complexity** to the codebase, **make system harder to understand**, and may **cause data inconsistency**

-----
<!-- 
_class: lead invert 
_transition: in-out
-->

![5 w:256 h:256](https://icongr.am/material/numeric-5-circle.svg?color=ff9900)

# Q/A


---- 

<!-- 
_class: invert 
_transition: in-out
-->

## Quiz:

----


## Miscs
- https://martinfowler.com/articles/break-monolith-into-microservices.html
- https://danlebrero.com/2022/02/09/monolith-to-microservices-summary/#ch-3
- https://www.atlassian.com/engineering/software-engineering-principles-massive-projects