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

![1](https://icongr.am/material/numeric-1-circle.svg?color=666666) re:Previous patterns
![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Pattern: Parallel Run
![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Pattern: Decorating Collaborator
![4](https://icongr.am/material/numeric-4-circle.svg?color=666666) Pattern: Change Data Capture (CDC)
![5](https://icongr.am/material/numeric-5-circle.svg?color=666666) Q/A

----
<!-- _class: lead -->

![1 w:256 h:256](https://icongr.am/material/numeric-1-circle.svg?color=ff9900)

# re:Previous patterns

-----
## re: Strangler Fig

"Big-bang migration" ❌  
"Incremental rewrites" ✅

---- 

![img w:800](./resources/1.png)

Don't use when
-   When requests to the back-end system cannot be intercepted.
-   For smaller systems where the complexity of wholesale replacement is low.

----
## re: UI composition

![img w:400](./resources/2.jpeg)

Don't use when
- The UI is simple 
- The cost of maintenance separated UI is high


----
## re: Branch by abstraction

![bg left fit](./resources/3.png)Don't use when
- The change is not deep inside the functionality
- Having feature flag -> not necessary
- Bad ownership model: Team introducing code conflicts, duplication
----
<!-- _class: lead -->

![2 w:256 h:256](https://icongr.am/material/numeric-2-circle.svg?color=ff9900)

# Pattern: Parallel Run

----

### Definition

Problem: You want canary releases to beta users

Both old and new implementation are run at the same time to _mitigate the risk_ of switching to a new service.

Terms: Soft launch, dark launch, canary release, beta testing

----

### **Example:** Credit Derivative Pricing

![ui_composition_widget h:500 w:430](./resources/p5_1.png)

Involve money 💰

-----
### Pros

- Incremental rollout to reduce risk
- Early feedback: beta user can report issues
- Blast radius control


----
### Cons

It is **powerful**, however there are drawbacks
- High complexity: need extra tools to segment users with opt-in, opt-out and manage targeted features
- Higher cost of maintenance: maintain 2 implementations at a time is time consuming
- Confuse user due to poor management

----
<!-- _class: lead -->

![3 w:256 h:256](https://icongr.am/material/numeric-3-circle.svg?color=ff9900)

# Pattern: Decorating Collaborator

----
### Definition

Problem: new features during spinning off a service away from monolith. Want to change behavior without modification to the existing code.

Attach something that the legacy downstream knows nothing about it  
1.  allow the call to primary service (A)
2.  based on the result -> call to external microservice (B) (concept: callback)

----
### **Example:** Loyalty Program of Music Corp

Module `Order` is a legacy module, however we want to add `Loyalty` service

![ui_composition_widget h:400 w:700](./resources/p4_1.png)

----
### Pros

- Very flexible to change the behavior without modify the existing implementation
- Separation of concerns
- Easy to maintain

----
### Cons

- Tight coupling: abusing the decorator may make it less reusable, eventually becomes a bigger module
- Less visible: hard to know the flow
- Lag as the decorator becomes critical path no matter how the behavior is

![img](https://www.explainxkcd.com/wiki/images/0/09/cumulonimbus.png)

----
### Quiz

Given the Music Corp example, why not moving the callback logic to frontend?

----
<!-- _class: lead -->

![4 w:256 h:256](https://icongr.am/material/numeric-4-circle.svg?color=ff9900)

# Pattern: Change Data Capture

----

### Definition

Rather than trying to intercept and act on calls made into the monolith, we react to changes made in a datastore.

The underlying capture system has to be coupled to the monolith’s datastore

-----

### **Example:** Issuing Loyalty Cards

![ui_composition_widget h:300 w:700](./resources/p6_1.png)

💡 Use decorating collaborator to trigger Loyalty card printer at proxy level.

-----

### **Example:** Issuing Loyalty Cards

![ui_composition_widget h:500 w:500](./resources/p6_2.png)

❌ Need to make an addition call to Monolith to query more data

------
### **Example:** Issuing Loyalty Cards

With change data capture
-   Reduce network round trip 
-   Don’t need to touch monolith codebase

![ui_composition_widget h:300 w:700](./resources/p6_3.png)

-----

### Implementations: DB trigger

Although there may also be limitations as to what these triggers can do, some DBMS allow to call web services or custom code (Ex: Oracle support Java code for DB triggers)

![ui_composition_widget h:300 w:700](./resources/p6_4.png)


-----

### Implementations: DB trigger

Pros:
- Quite simple implementation

Cons:
- Using triggers make system harder to understand
- Tougher data change management
- “Having one or two database triggers isn’t terrible. Building a whole system off them is a terrible idea.”


-----

### Implementations:  Transaction log pollers

Inside most databases, certainly all mainstream transactional databases, there exists a transaction log. This file records all data changes.

![ui_composition_widget h:400 w:700](./resources/p6_5.png)

-----

### Implementations:  Transaction log pollers

Pros:
- Have a huge array of support built tools exist.
- The tools may support to place messages onto message broker => useful for async behavior
- Can run off a replica of the transaction log => fewer concerns regarding coupling or contention.

Cons:
- Add significant complexity to your solution
-----
### Implementations:  Batch delta copier

Build a schedule job scanning and handling new data changes

Pros:
- The most simplistic approach

Cons:
- Have to manage timestampt or the last captured row
- Stale data

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
- https://www.confluent.io/learn/change-data-capture/
	- https://github.com/confluentinc/demo-database-modernization
- https://danlebrero.com/2022/02/09/monolith-to-microservices-summary/#ch-3
- https://launchdarkly.com/blog/canary-release-is-the-new-beta/