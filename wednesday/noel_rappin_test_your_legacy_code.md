## Noel Rappin - Test Your Legacy Code

- You get handed a 'rescue' project
- What do you do first?
  - Look over the code
  - Loudly exclaim: the last person to work on this was an idiot! (7 out of 10 times it was you)
  - Now what?
  - Shake your fist for a minute - get it out of your system, then get back to work
  - There may be more to the situation than you were aware of - poison doesn't help

# What is a legacy application?
- Code without tests
- From Michael Feathers' 2004 "Dealing with Legacy Code" - mostly Java, still good advice/strategies
- Not the most useful definition because of the amount of testing and increasing prevalence of same between then and now
- Many projects today have tests, but *feel* legacy
- Lack of tests is indicative of code based on lost requirements
  - Some are written down, some aren't
  - They may still have been important pressures at the time they were written
  - Many are implicit (design choices, etc.)
- Really difficult to come into a codebase and decipher these things
- You don't know what "correct" behavior is
  - This is what makes testing these apps challenging but very necessary
  
# What is your goal?
- Respect working code - do no harm
- Deliver features (maybe including bugfixes)
- Keep a sustainable pace (you can't continue to make things worse)
- So, you need to improve code quality over time!

# The Boy Scout Rule
- "Leave the campsite as good or better than when you found it"
- Same goes for code - iterate improvement over time

# You need to make a change to your rescue project
- Goofus and Gallant analogy
- "First, let's write tests that just cover the whole app"
  - Why not?  "It's a small world but I wouldn't want to paint it"
  - You're trying to keep a sustainable pace for features
  - Not adding value to the software
  - It's not a quick win
  - You can/will introduce bugs (do no harm!) as a result of making it testable
  - Not the TDD process at its most effective
- "I'll just make that change and drop it in"
  - It's setting a bad precedent - part of your job is establishing trust in the TDD process, and you're undermining that
  - It doesn't actually make anything better
- Both Goofus behavior but understandable instinctive responses
- So what's a Gallant developer to do?
  - Use TDD, but draw a line in the sand and apply them to new changes only
  - Red, green, refactor
  - Over time, let your coverage and quality grow organically on the parts of the system that are changing
  - Don't waste time testing things that are working and not changing
- Not that easy - why not?
  - You don't know what to test
  - Existing behavior is unidentified
  - It's hard to isolate objects under test if the tests weren't written first
  - A tip - git is your friend, go into a branch for your changes
    - If you break something you don't intend to, git bisect is your friend
    - Check in frequently and atomically to separate changes
    - Saves you hours and hours

# Cucumber
- A black box test, so it is independent of the code under test
- Can cover big swaths of the app very quickly
- Downside is that it's really slow compared to unit tests - makes test driving difficult.  Creates a lot of objects.
- It's also not great at isolating where (specifically) a problem is
- Decent for regressions
- May be hard to integrate with older apps (pre 2.3)

# Test-driven Exploration
- Like TDD with cheating
- At the unit level
- Red -> Make it green -> Refactor
- Nice way to capture behavior for regression tests
-  Code is the source of truth
  - The problem is that in legacy apps, the code is the source of truth, whereas the tests should in that role
  - Want to incrementally move that direction

# Mock testing
- They're fast and allow unit testing
- Can be difficult to set up (dependencies, etc)
  - If you need too many mocks, they become brittle and impede later refactoring

# Isolation
- Only touch existing methods to call your new methods
- Dispatch from the tops of old methods where possible
- Nice way to measure progress as you see code transition over to the new modules
- Kill the old code over time

# Seams
- Change behavior without touching code
- Sort of a last report technique - in case of emergency, break glass
  - A lever to get in and work with the code
  - Subclass an existing class, override as needed, and inject into the appropriate place in the test
  - Technically a specific case of a mock object
- Suffers from the same brittleness concerns as mocks
- Useful when regular mocks don't provide quite enough depth

# Additional defaulted method arguments
- Nice to not have to change existing function calls
- Don't go overboard with it - easy to make a messy signature

# Singleton classes
- attach methods to a particular instance
- instance = Purchase.new; instance.def b; puts 'hi'; end;

# Use a pebble
- An object that defines method missing and dumps out caller information to trace
- Michael Feathers has a blog post about it
- Sort of like a focused, detailed stacktrace

# What about bad tests?
- If the project already has tests and they suck
- Make sure the suite runs and passes
  - Fix breakage in here first so you have a sane place to start
- Remember that tests are code and subject to the same refactoring and maintenance needs
  - Just because they have the same characters doesn't mean they're duplicated
  - See D. Chelimsky talk
- 5 minute rule
  - Look at a test and if you can't figure out what it does in 5 minutes, delete it (or comment it out)
  - Nice way to triage and separate good from bad
# When to refactor?
- Simple refactors can be done anytime the tests are green
  - Especially if you can't test anything without refactoring
  - Watch out - nothing is so simple that you can't screw it up
- More complex refactors shoudl be tied to actual new requests
- "Turbulence" gem - flog complexity over commits
  - Complicated things that are constantly being changed should be refactored
  
# Summary
- Respect working code
- Test at as high a level as you can
- Leave things better than you found them
- Isolate the code under tests
- Consider the cost of change against the cost of not making the change (may be lower than you think)