# Michael Cerna - Inside Groupon
## Process
- Weekly Iterations
- Discuss points as stories
- Process is independent of tools
- Developers need to develop meeting skills - Single Responsibility Principle

## Methodologies
- Work with Obtiva, Pivotal, and In-House devs
- Domain awareness is key - loose DDD
- Inter-department naming is immensely important
  - Otherwise get stuck in trivial naming issues that become big problems
- Features are built around the data model, interface follows from there
- Complexity of deal variations and terms requires a very flexible model
- BDD/Testing - don't care how, just do it
  - Test Pyramid - Wide unit tests, narrow+deep selenium-based integration tests
  - Not big fans of outside-in development
  - No external service calls - use mocks
  - Test remote contracts separately
  - Use Selenium and CUcumbers for actors and users
  
## Architecture
- Domain-driven SOA-fication
- Have avoided performance hotspot-driven splitting to avoid fragmentation
- Favor abstractions and encapsulation before splitting
  - Don't confuse abstraction and performance concerns
- Alternative/best fit paradigms for subsystems
  - Strategy patterns with defined interfaces

## Ruby
- Abuse metaprogramming (not always a good thing)
- Open/close principle with alias_method_chain
  - Make uses independent, pass an options hash or splat, etc.
  - Stuff like logging, timestamping, etc.
  - Don't mess with the original behavior!
  - Avoid coupling (direct and temporal)
- Take cues from Rails for how to extend things
- Bend the language to fit the problem space (not as extreme as you can do with Lisp)

## ActiveRecord Gotchas
  - abort-on-return false can lead to a lot of weirdness and confusion
  - External service calls in callbacks can backfire in terrible ways (keeping transactions open, etc.)
  - Even internal service calls can create major trouble if they do down
  - Transactions can kick your ass
  - The ReachAround pattern (do NOT image search!)
    - Having multiple instances of the same ActiveRecord row present in memory
    - Especially since AR has no identity map
    - Weird inconsistencies, etc.
    - Can happen in callbacks, parent->child->parent demeter violations, etc.
## Code Leakage - http://github.com/groupon
  - AR stuff, syntactic sugar, performance optimizations
  - AR query extensions (array syntax with and/or/not, like queries from DM)
  - Symbol expression library for almost-macro-like chaining of map calls and the like
  
## Q&A
- Training new devs and bringing them on with the same philosophy is difficult
- Want to make it jive but still enforce best practices