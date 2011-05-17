# Double Shipping Software for Profit
- Enterprise software is on a whole different scale - $100 a month is nothing
  - Doesn't have to be boring
  - Can you double ship and profit on both sides of the equation in different marketplaces?
- Github:FI
  - Self-contained local install
  - Up and going in 10-15 minutes
- Competitors
- Long-running branches
  - Minimize the "buts" in different products
- Curious motherfuckers

## Why do it?
  - Evangelism of social coding
  - Would exist even without Git
  - Increases developer happiness
  - Lots o' money (even though the sales cycle is a pain in the ass)
  
## Why not?
  - Major cultural shift for a small company
  
## What is FI?
  - Built on JRuby (different from Github.com)
  - On its own branch - too divergent from the main codebase
  - Installer (managed by a 3rd party currently, looking to bring in-house)
  - Special features (LDAP, auth hooks, integrations, server-side hooks, etc)
  - One developer (Originally PJ's side project, Zach for about a year now, 3-4 hires coming soon)
  
## Keeping secret code secret is a challenge 
  - Don't want to have an FI hacker/reverse engineer compromise Github.com
  - Use jrubyc to compile down to Java bytecode and only deliver that to clients
  - Aside: rubinius is awesome!
  - Can do it on MRI, but it's super hacky and not really worth it
  
## Sales
  - Profits are good, but hard to get.
  - Need to hire someone who really gets into enterprise sales (technical but not slimy)
  
## Support
  - Bugs can be easily introduced and fixed online, but harder in shipped software
  - Way more layers and time (weeks!)
  - More humans, more time, more money
  - Make customers pay to scale it!
  
## Key point: differences suck
  - Especially differences in codebases
  - FI doesn't need multiple servers, proxied services, etc.
  - End up with lots of conditionals - should really just handle in the main adapter - handle higher in the abstraction layer
    - Strategy pattern is your friend
    - RepoCreater example - inherit from a base strategy and get rid of what you don't need
    - Lets you provide a standardized API between products - make it easier on developers
  - Feature flipping makes things less frightening - e.g. live launch of Issues 2.0 at CodeConf
  
## Mistakes
  - Separate branches - now have to sift through a 20K line diff
  - Documentation - necessary if they're not getting the code
  - Versioning - different in shipped software than a web product.  be semantic.