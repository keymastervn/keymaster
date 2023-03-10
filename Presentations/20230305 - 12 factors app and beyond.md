### What for

![[Pasted image 20230310113643.png|300]]

A methodology for *building software-as-a-service* apps
Coined by Heroku's co-founder Adam Wiggins

----

### What is S-a-a-S (1)

https://dev.to/cloudtech/iaas-vs-paas-vs-saas-41d2

![[Pasted image 20230310114159.png|300]]

----

### What is S-a-a-S (2)

The analogy

![[Pasted image 20230310114241.png|300]]

What's next: ![[Pasted image 20230310114507.png|20]] 
Doesn't mean there is no bus, but the bus is hidden/visible on demand :lol:

----

### Factor - codebase (1)

Source is always tracked in a version control system. 1 source -> n deploy
Purpose: SVC
![[Pasted image 20230310114859.png|300]]
*Misconception*: n source -> n app is not an app but distributed systems
*Evolve*: monorepo, microservices 

----

### Factor - dependencies (2)

Explicitly declare and isolate dependencies.
Purpose: SVC

![[Pasted image 20230310115100.png|300]]
*Misconception*: curl (os contrib libgnu) is "not" a dependency if you love native HTTP client
*Evolve*: Dockerfile or CD tools eg. Ansible, Chef

----

### Factor - configurations (3)

Ensure configuration is separate from the application code so we can multi-env deploy.
Purpose: SVC

![[Pasted image 20230310115402.png|300]]
*Evolve*: Vault, service discovery systems or CD tools eg. Ansible, Chef

----

### Factor - backing services (4)

Subsystems, including third-party systems. 
Purpose: Reusability (David on Goliath)

![[Pasted image 20230310120217.png|300]]

----

### Factor - build + release + run (5)

Purpose: SVC, Release management

![[Pasted image 20230310115657.png|300]]
*Misconception*: release production hotfix -> edit runtime code (ssh) 🔪😵
*Evolve*: CI/CD tools, GitOps

----

### Factor - Processes (6)

Processes are stateless and [share-nothing](http://en.wikipedia.org/wiki/Shared_nothing_architecture). Data must be persisted in stateful backing services.
Purpose: scaling

![[Pasted image 20230310120724.png|300]]
*Misconception*: Yaml files, runtime artifact, websocket (stateful)

----

### Factor - Port Binding (7)

Each application should have a specific port mapping. 
Purpose: scaling, security

![[Pasted image 20230310120939.png|300]]
*Evolve*: containerization (k8s), service mesh

----

### Factor - Concurrency (8)

With (7) port binding + load balancer, then application's horizontal scaling.
Purpose: scaling

![[Pasted image 20230310121031.png|200]]
*Evolve*: containerization (k8s)

---- 

### Factor - Disposability (9)
Maximize robustness with fast startup and graceful shutdown. Purpose: resiliency, entropy
- minimize startup time
- [SIGTERM](http://en.wikipedia.org/wiki/SIGTERM)
- against sudden death by hardware
![[Pasted image 20230310133550.png|300]]
*Evolve*: exactly-one delivery, dead-letter queue

----

### Factor - Dev/prod parity (10)
Keep development, staging, and production as similar as possible to reduce *time gap, personnel gap, tools gap*. Purpose: CD
![[Pasted image 20230310134048.png|300]]
*Misconception*: Should [take a read](https://12factor.net/dev-prod-parity) because it is commonly understood incompletely
Evolve: containerization, CI/CD

----

### Factor - Log (11) (1/2)

![[Pasted image 20230310134116.png|300]]

---- 

### Factor - Log (11) (2/2)
Applications are event streams. Purpose: observability, debug

----

### Factor - Admin processes (12)
Isolate tasks from runtime dependency: `rails console`, `rake db:migrate` or some tasks
Purpose: debug, deployment, subsystem
![[Pasted image 20230310135037.png|250]]
Recommend [Read-eval-print-loop](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)

----

### Does it get wrong?

----

## Thank you