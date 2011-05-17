## ActiveSupport: What We Should Know About What We Don't with Bryan Liles
- Coding Effectively With Legacy Code book by Mike Feathers (great, highly recommended)
- History Lesson
  - RubyFacets - ruby core extensions library (around since 2005)
    - Split into core/more
    - String interpolation, symbolize_keys, time manipulations, random letter picker, memoization, combinators
  - Ruby doesn't have recursive functions by default (not a higher order language) - Y Combinators fake this
- Why do we need ActiveSupport if we have facets?
  - "Namespace collisions"?
  - "Too much code"?
  - "NIH"?
  - Not really sure
- History
  - Extracted from ActionPark and ActiveRecord in 2005
  - "Cow".pluralize => "kine"
  
- Accessors
  - Like beer!
  - mattr_accessor and cattr_accessor
  - Nice for configuration
  - Can customize what methods are generator (instance_writer => false)
  - Are in facets as well (donated back by Jamis)
  - attr_accessor_with_default <- cool!

- Benchmarks
  - benchmark do...end
  - Exactly like Rails request logging
  
- Callbacks
  - define_callbacks and run_callbacks
  - Can use set_callback to do a AOP sort of thing

- Mixins/Concerns
  - Replace the included hook song-and-dance
  - Much cleaner and nicer

- Configurable
  - x.config.some_key = "value"
  
- Instrumentation
  - Notifications - can subscribe and push
  - Nice way to abstract out error handling logic

- Other stuff
  - Random strings (hex, etc.)
  - GZip compression
  - In AS 3, can use bite-sized chunks so you don't load the whole thing
    - Pattern copied from facets

- Facets is on Github now
