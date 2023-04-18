## The Original 12 Factors

From Heroku: you don't have to worry about infrastructure. Cloud friendly means Heroku friendly
Fact: you should care, so how to "cloud friendly"?

Follow-up purpose: build a cloud-native application
- declarative formats for automation and setup
- clean contract with the underlying operating sys‐tem
- dynamically scalable


## A minute for the old lessons
- Codebase
- Dependencies
- Configuration
- Backing Services
- Build, release, run
- Processes
- Port binding
- Concurrency
- Disposability
- Dev/prod parity
- Logs
- Admin processes

Alright, let's see the new 15 factors 

## Beyond

### One codebase, one app

old - codebase: One codebase tracked in revision control, many deploys.

scenario: many applications can use the same codebase
issue: 
- impossible to automate the build
- lack of discipline, as violate Conway's law
approach: should go microservices
tool: eg. Github, CI

### API first

scenario: team does not talk in API
issue:
- not clear ownership, collaboration sucks
- don't know where to get in
- platform-agnostic: applicable for "mobile-first"
approach: build APIs as public contracts
tool: API Blueprint, server mocks

### Dependency management

scenario: Mommy servers (lol, mainapp) has everything
issue: if not control dependencies properly
- app will crash
- developer unwantedly overrides code
approach: vendoring, never relies on implicit existence of system-wide packages
tool: Bundler (Ruby), Gradle (Java)

### Design, Build, Release, Run

Design is crucial, at least
scenario: waterfall -> very adhoc, lack of awareness
issue: 
- no proper CD, troubleshooting "it works on my machine"
  - how to know least deployable unit?
approach: high-level design that is used to inform everything

Release
issue: when thing goes wrong, how to rollback?
approach: immutable artifact, a release

Run
scenario: cloud runtime is wild, keep it alive, dynamic scaling and fault tolerance
issue: 
tool: DevOps, Docker, cloud provider has supported

### Configuration, Credentials, and Code

Treat configuration, credentials, and code as volatile substances that explode when combined

scenario: keep configuration (non-sensitive) with credentials (sensitive) at a public place (codebase)
issue: lose sensitive credentials, mining BTC
approach: should keep configuration separate from code and credentials
tool: Vault

approach: externalizing configuration
Treat your app like open source

### Logs

Logs should be treated as event streams
approach: log rotation
tool (to deal with logs): ELK (ElasticSearch, Logstash, and Kibana)

### Disposability

Dante Must Die
A cloud-native application’s processes are disposable, which means they can be started or stopped rapidly
An application cannot scale, deploy, release, or recover rapidly if it cannot start rapidly and shut down gracefully

scenario: downtime when an enterprise app deploy
issue: denial of service (during startup) or can't autoscale within the period

### Backing Services
Factor 4 states that you should treat backing services as bound resources
Not only externalize configuration, but also bound resources eg. database.

Possible to attach and detach bound resources at will, without re-deploying the application

/// TBD