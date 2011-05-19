# Yehuda - Building Rich Apps

## If all I'm doing is sending JSON back and forth, what's the point of Rails?  Wouldn't Sinatra or something like that be better?
- Rails is more than the surface area (HTML)
- Hides a lot of complexity very well
- Rewind to 2006 - DHH's REST keynote
  - More than just HTML
  - JSON, XML, iCal
  - Rails as not just an HTML geenrator
- Much has been built to this end in the last few years, particularly for Rails 3
  - Richness is not immediately evident
## Rails *is* a rich HTTP abstraction
- Worthwhile to read the HTTP spec (and the HTML spec)
  - Sidenote - even IE is compliant with most of the spec
- Fat model, skinny controller - typical rails app is very heavy on domain logic
  - Background jobs
  - ActiveRecord
  - Authentication
  - Authorization
- Think about what percentage of your app is straight HTML rendering before you make the decision to use Rails v. Sinatra
## ActionDispatch
- Much cleaner in Rails 3 (thanks to Josh Peek)
- Middleware - getting everything to work together cleanly (Rack::Cookies, Rack::Cache) is non-trivial, edge cases are maddening
  - Rapidly turns into a massive headache
  - Turns into a half-assed implementation of Rails
  - Ex. session - multiple stores
  - Ex. standards mode (X-UA-Compatible)
  - Ex. signed and permanent cookies
  - Ex. parameters (JSON/XML/Nested)
  - Ex. routing engine, caching, reloading, security (IP spoofing, timing attacks - google it), etc.
  - Stuff that users of the framework really should not have to think about
- The reality is that ActionView is a tiny part of Rails and its ecosystem, especially in the context of your application.  And HTTP is non-trivial.
- Rails is designed for this!
## Architecture
- Normal MVC
  - Browser <-> Router <-> Controller <-> View (authentication/authorization happening in the controller)
- Rich client MVC is very similar, but the controller packages up the state in a serializable format for transport
  - Replace view layer with a representation layout
- State of JS templating in 2005 was not good
  - Rails helpers were too attractive, so people punted
- Much better now
  - Eco, mustache, handlebars, dust, etc.
  - No reason not to run your templates in the client
  - Don't let the thought of moving your actionview layer to the client scare you
  - The state transfer mechanism is the same
## "API"
- Guidance is missing
- Compare to AR - foreign key conventions, etc.
- For example 
  - how do you nest child resources?
  - Naming/structuring keys is up to you
  - Error reporting/formatting is up to you
- Makes it very difficult to write a single client - they end up being very ad-hoc
- Strobe is building a pure-API server to package a lot of this up
- ActiveResource requires way too much configuration - not very Railsy
  - Difficult to make it work for both web and internal APIs
  
- Rules for API design
  - Root needs to have 1 or more keys that indicate the type of what you're representing
    - Think of it like a URL
    - Lets the client figure out what's in the packet and stand alone
  - No nested resources
    - Difficult to infer the structure of resources or guess
    - Use URL params if needed
    - Remember that this is a computer protocol - humans won't see it
    - An alternative could be using *only* nested resources, but has limitations around enumerating all
  - Response-nested children should follow the same rules
  - Don't do a mix of the nested object/list of IDs approach - pick one!
     - Want to make it detectable
- Ultimate goal is one client for all of your resources
  - Convention over configuration!
  - Canary is: Conventional ActiveResource should be possible - work most of the time exactly like AR does
  
## Bulk
- Latency and response size are important considerations in mobile apps
- No current conventions for doing subset bulk operations - right now it's one or all
  - Don't want to build special actions all over the place
  - Ex. mark all done - 100 requests (like an N+1 but multiplied) is unacceptable
  - Ideally would like to have it possible in 1 call
- Solution - bulk_api plugin (http://github.com/drogus/bulk_api) - same guy as Rails 3.1 engines
  - Principles
    - 1 true way to represent resource
    - All requests go through a single endpoint (little ugly, but doesn't map nicely to REST)
    - GET to fetch /api/bulk?posts=all, /api/bulk?posts=1,2
    - POST with a nested hash of things to create
    - PUT to update
    - Batched updates
  - Conventions
    - Proxy directly to ActiveRecord absent any other configuration
    - Use ApplicationResource to customize authentication/authorization with hooks
    - Can't really use controllers for this which is why it's a separate object
    - Can use controller-like customizations if you want (but you shouldn't)
- We're at the point where this should be easy
  - Like the rest of Rails - convention over configuration
  - Simple cases shouldn't cost anything
  - ...but be powerful enough for advanced cases
  - Machine protocols should be optimized for efficiency and consistency, not human aesthetics
  - Rails is great for JSON APIs
  
## Q&A
- Cool stuff in Chrome around multi-request compression and SPDY
- API versions is kind of unnecessary if you have a decent representation
  - Conceptual differences do fundamentally require changes