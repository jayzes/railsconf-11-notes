# Joe Ferriss - Testing the Impossible

- Going to walk through testing the copycopter client

## What is Copycopter?
- Solving a few problems
  - Don't care about copy text when developing
  - Product owners do, but don't care about deployments
- Devs put in placeholder text via I18n API
- POs can open up and have changes reflected in prod within 5 minutes
- Design params
  - Automatic
  - Easy to set up
  - Can't affect production performance in any way
- Makes it complex!
  - But got 100% C0 coverage
  - 100% test-driven
  - Didn't drive Joe to kill himself or give up testing
  
## How did we do it?


### Don't mix concerns
- Original version of client
- Application talks to I18n, talks to Sync object which managed everything
- Worked decently well
- Sync class
  - Overly generic name
  - Lots of methods
  - Lots of private methods
  - Ripe for extraction
  - Tests were pretty verbose - 26 examples, 44 seconds (tough for feedback loop)
- Solution: separate concerns
  - Single Responsibility Principle - every obejct shoukld have a single responsibility that is entirely encapsulated by the class
    - Uncle Bob: An object should have single reason to need change (the code)
  - Extracted a Poller class, a Cache class, and a ProcessGuard class
  - Moved to 29 examples in 13.25 seconds
  - Makes everything easier to write and understand
  
### Inherit Less
- Needed to speed up the feedback loop on copy in development (was originally 15 minutes)
  - Checks for new copy and uploads new defaults on every request in development
- Was wrapped in activesupport::Concern and used before/after filters
- Too much boilerplate - more boilerplate than code
- Hard to just test the subclass and not get into the superclass too
- Solution: prefer composition to inheritance
  - Most components should be loosely coupled anyway
  - Mixins are still inheritance and have many of the same problems
  - Refactored into a Rack middleware which is much simpler and easier to test
  
### Don't Depend on Concretions
- If you build up clients, etc as instance variables, stubbing them out is super messy
- Solution: use dependency inversion/dependency injection
  - Separate behavior from dependency resolution
  - Easy to do with defaulted parameters in the constructor
  - Or a configuration class
  
### Avoid class methods
- Original copycopter had a master configuration object
- Tough cause it had to be reset every test
- Leads to interacting tests, which are maddening to figure out
- Solution: Prefer instance methods
  - Change the client constructor/build method to handle configuration directly
  
### Avoid soup
- Good on a winter day, not in your code
- If all of your code is in one place, all of your tests are in one place
- Makes setup and teardown verbose and messy

### Create Seams
- That's what all of these are getting at
- Makes it easy to build up your tests and make them expressive but not verbose

### Testing Design

- Test Scope - once thing at a time vs. a bunch
  - Unit/Component/Isolation
  - Unit/Functional/Integration
  - Test at the level you actually care about
  - Integration to test broad swaths/major paths, focused specs for breadth

- Restrict fixture scope
  - Keep data close to the test it applies to
  - Changing all of your data and all of your tests at the same time sucks
  - Solution: use fresh fixtures
    - Not just DB, but external services, etc.
    - Create everything you need for the test right in the test
    
- Avoid test helpers
  - Especially stubs - can easily end up with a bunch of boilerplate and down a rabbit hole
  - May make you question your sanity
  - Might look into fakes or proxies instead
    - Fake is a full-on replica of your object but with hooks to verify interactions
    - Easy to distribute in gem form
    - Don't take that long to build, and feel much more natural
    - Just plain ruby objects!
    - Easy to integrate with dependency injection
    - If you really want, write an RSpec/Shoulda matcher to make your output more friendly
    
### Rails
- Stay Generic
  - Use middleware where possible
  - Avoid monkeypatching at all costs
  - Keep business logic outside of engine parts
    - Engine testing stuff isn't all well formed, so it's easier if you can avoid it
  - Use Railties only for configuration and initialization
    - Just to glue pieces together
- Build a full integration test
  - Generate an app
  - Install gem
  - Exercise major paths
  - Boot a server if you need to
  - Test across multiple versions
  - Check into capybara and aruba
  - Check out "appraisal" - tool to manage multiple gemfiles for testing (https://github.com/thoughtbot/appraisal)
  
- Build good isolation tests
  - Unit test componets *without* Rails (Separate rake task, don't even load rails)
  - Test Rails-specific components separately
  - Use fakes, especially for Rails components that are tough to test
  
### Don't be afraid
- Don't back down from writing tests around this stuff
- The more you do it, the better you get, the faster you become
- Good practices organizing your code make testing difficult components much more manageable