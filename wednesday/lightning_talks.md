# Lightning Talks

## Freelancing on Rails - A Year in Review (John McCaffrey)
- Freelancing career started at Railsconf last year
- Simple side projects are part of Rails' DNA
- Lends itself to end-to-end work, remote work, agile tasks
- Bill as 1099
- Rate = salary / 2080 * 1.5
- Why do it?
  - Focus on stuff that I like to do
  - Helped increase skills in a variety of areas
  - Have to learn on the fly
  - Can't get complacent
  - Need to get to know the problem space
  - Need to stay current/stay ahead
    - Correlation between what you know and what you can charge
    - Look at Rails Rumble toolsets for inspiration/fast building
    - Invest in yourself
  - Developer a network of expert friends
- Tools
  - Freshbooks, teamviewer
- Raised rate 20% over the course of the year - raise confidence, learn a ton

## Phil Nash - Mint Digital (Agency)
- Performance
  - What about people on old connections, remote locations, etc?
  - Size matters
    - Minimize requests, bytes, disk reads
- Introducing the asset hat gem
  - Minify and bundle code
  - CDN support
  - Load JS from Google CDN where possible
  - Async loading via LabJS
  - Looking to add CDNJS, Head.js, etc.
- Tested and used on multiple client sites
- Install gem, use asset_hat helpers, set up assets.yml
- http://www.logicalfriday.com/

## Steve Klabnick - Literate Coding
- Rubyists/Railsists value pretty *a lot*
- Beautiful code in Ruby vs. ugly code in java
- Best practices - variables, whitespace, OO, comments, etc.
- Programming languages are still languages (a la Chomsky) and should be treated as such
- Composing programs to communicate effectively, as opposed to simply writing them
- Who are you writing for?
  - Programs have multiple audiences
  - You, others, and the computer need to know what's going on
- Tools that are focusing on this
  - Docco is one
  - Something we should continue to focus on
  
## Ryan Bigg - Ways to help out noobs
- I'm famous online
- Job board is crazy here
- RailsCamps in Australia - great way to get people involved at a community level
  - Americans should do this more often
- Write better documentation
  - Don't shotgun blast with "Rails docs suck" - be specific or write a patch
- Make people feel welcome
  - No "RTFM" - leave that for the PHP crowd
  
# Ryan McGeary - "Vendor Everything" still applies
- Why?
  - Make it easy to jump around between projects
  - Bundler and RVM help out, but conventions have yet to be settled
- Don't .gitignore your .rvmrc!
- RVM gemsets are overrated
  - Add little value to application work
  - Use bundler with --path instead
  - Use vendor/ruby and .gitignore it
  - Effectively an anonymopus gemset
- Package your gems in vendor/cache
  - Make install fast and keeps external deps to a minimum
  
# Jake Scruggs - Why Startups are Ruining My Lie
- Bite me - just lost my 4th dev in less than a year, on a 4 person team
- Over-focusing on startups generates some bad attitudes
  - Isn't the point of a startup to get to profitability?
  - Want to have hobbies outside of programming
  - The point of all of these principles, patterns, and practices is to get to an unsurprising codebase
- Working hard to make things stable, awesome, and profitable
  - But then we're not a startup anymore
  - Sustainable pace is bullshit - cap with real hours
- Zero chance of getting rich quick
- Say we care about all of these good practices, but makes things boring?  Why is everyone leaving for startups?
- Bonus: Cron timer saying "Time for exercise" - pomodoro with pushups
- Double bonus: 1.9.2 + puppet = problems
- Text to noise (github project) - hear problems with your logs based on regex

## Dean Radcliffe - Landlond (Multi-Tenancy with AR)
- All about scoping - you don't want to fuck it up
- Most of the time we use explicit object scoping, which is error-prone and not DRY
- Landlord lets you dynamically define partitions and set the current conditions
- Uses Rails' default scopes underpinnings
- Can use HTTP initializers to look per request

## Corey Haines - Fast Rails Tests
- Super fast RSpec
- Test-first vs. test-driven
- Change the test
- Change the design
   - Isolate from DB, bundler, rails
   - Bundle exec takes 1.5 seconds to load
   - Use gemsets and keep them clean
   - Don't load Rails
   - Don't let code use AR finders
   - AR interactions only within model methods - expose an API (domain-specific semantic)
   - Stub meaningful methods, create non-fragile tests

## Greg Bell - ActiveAdmin
- New project - admin framework for Rails
- Need something more than CRUD-based tools
- Complexity of administration is related to complexity of the app
- Production app administration != CRUD
- Goals:
  - Fast for devs
  - Usable by ops staff
  - Encode UI best practices
  - Needs to be crazy extensible
- Uses devise by default - plug and play
- Generate resources under app/admin/<name>
- http://activeadmin.info/

# Raimonds Simanovskis - Multidimensional Data Analysis in JRuby
- SQL is good for detailed data queries
- Gets really complex for analytical queries
- Dsitributed map-reduce?  Not so much
- Multidomensional model more appropriate
  - Cubes, dimensional hierarchies and levels, measures
  - OLAP technologies
  - Pentaho Mondrian - writren in Java
  - mondrian-olap gem to encapsulate JARs and a DSL for creating queries nicely (like ARel)
    - can do more complex queries by hand if needed
    - can define the OLAP schema to map cubes to your relational schema
    - used for eazybi.com - BI as a service app