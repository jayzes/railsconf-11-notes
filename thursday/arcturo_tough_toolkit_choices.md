# David Black and Jeremy McAnally - TWTOWTDI - Making those tough toolkit choices

## Flame On
- Not going to get into it
- Teach you to resolve bikeshedding

## It's all Josh Susser's fault
- Post about the tyranny of choice
- Difficult to pick the "right" platform for a greenfield project
- Opinionated choices have gotten less clear

## Making qualitative choices
- What choices need to be made?
- Which choices actually matter?
- What criteria of choice can be identified?

## Handling choice
- Avoid "this sux" and "this rulez"
- Totally fine to like/prefer one
- Example: templating
  - HAML has a ton of sux/rulez discourse, but some real issues that can be identified
  - It exists to protect you from HTML
  - Takes time to get used to
  - Think about:  
    - Is HTML a problem for your team?  
    - Do you have time to get used to HAML?  
    - Is it going to work as a common ground for everyone writing/looking at views?  
    - Do you want to try a spike to try it out?
- Important to factor in organizational realities
  - Training, schemas, staffing plans, etc.
  - Who gets to see what? (who is the most invested)
  - Who gets to change what?
  - Are some decisions already made? (schemas, deployment toolchain, etc)
  - A lot of this seems to be about the database
- Looks for key qualitative differences, not just minor flavor differences
  - Example: liquid
  - Safe templating, not just another templating syntax
  - Actually changes the relationship with the technology
  
## Rails 3 - modularity vs. choice
- Busts this whole thing wide open
- Modularity is expanded but not entirely new (and choice is not always synonymous with modularity)
- Much is primarily of interest to framework builders
- Not everyone is going to write their own ORM

## What if there are no criteria of choice?
- Avoiding decisions - they're fraught with peril
- Make sure you actually need to make it
  - Can you use both/all?
  - Should you actually be using anything at all?  May be easy to write it yourself.
  - Can you defer the decision until more of it shakes out (and maybe have some criteria?)
  
## Dealing with Fallout
- Keep it logical - no emotions or weaseling
  - Get a whiteboard and have folks write out reasons, then erase the crap that doesn't matter
- Take out emotions - get rid of the poison vibe
- Make your benefits analysis extremely honest
- Make the decision and make people stick with it (otherwise there will be lots of backpedaling)
- Learn to be either a diplomat or a tyrant (based on who you're dealing with)
- 3 Ds of ZOMG decisions
  - Decide (the direction)
  - Deliver (the mandate)
  - Discuss (the impact)
  
## Testing framework
- RSpec vs. Test::Unit is largely syntax for most people
- So how do you decide?
  - Democracy or dictatorship
  - Only two choices

## Tough nuts to crack
- Supplemental code
  - Ruby add-ons
  - Rails overrides
  - Project-specific libraries
  - Organization-specific code
- Ruby versions
- Test frameworks (will we ever know if TDD and BDD are significantly different?)
  
## Q&A
Interestings - bundler vs. isolate
Bluepill doesn't work so well in production