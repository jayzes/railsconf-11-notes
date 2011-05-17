# Why You Should Never Use an ORM?
- What are you, crazy?
- "Any intelligent fool can make things bigger, more complex, and violent" - Einstein
- John's path of violence
  - Twitter gem
  - Led to HTTParty (reusable HTTP request logic)
  - Led to HappyMapper (make XML more bearable)
  - Led to MongoMapper (apply principles to DBs too)
  - Led to ToyStore (talk to any database)
- Realize that object mappers are not always the best solution to the problem
- Tried going without on Gaug.es
  - Went a lot further than thought possible
  - ORM is in there now, but it's pretty light
  
# 3 points - think about your interface, data, and code

## Interface
- ORMs too often lead to interface laziness (create, update_attributes, etc.)
  - Build out helpers (add_owner vs. the raw ORM update call)
  - Reveal intention!
  - Avoid magic strings
  - Or use presenters to abstract objects that aren't directly DB-backed - fat model can go too far
  - Hasheritis
  
## Data
  - ORMs sometimes hide creative ways you can store and retrieve your data
  - Put some thought into it
  - Failure and Triumph - move fast and break things
    - Gauges stores content for a page - originally a single document with a single day's information
    - Super easy to query
    - A big site (20k pages) installed gauges, and suddenly the response times tanked - had to add a switch to turn off sites
    - Add indexes for reading and writing (upserts) and blew out into multiple documents
    - Kept field names short to save space in DB - hard to do well in an ORM
    - Roll data up monthly (range-based partitioning) in collections and delete the write index
    - Fight the current fire and move on to the next thing
  - URLs are big and ugly
    - Maybe we should hash them to save RAM in the index?
    - Zlib.crc32 is nice and easy
  - Every bit counts! (literally)
    - Don't just think of your DB as a big hash in the cloud
    - Ex. strings for state - unnecessary and wasteful.  Easier to change later if you have an abstraction (integer mapping)
    - Compression is nice too - check out RSmaz (or plain or Zlib) or msgpack
  - Partial updates
    - Only send what you need across the wire - saves time and electrons
  - Unused fields
    - Timestamps are a good example - do you really need them on every doc/record?
  - "Where" matters - locality of memory vs. disk vs network
    - Use stub objects that look like AR but are backed by memory
    - ToyStore has a memory store, which is cool
    - Make the most of your queries
  - A little Ruby can go a long way
    - Don't be afraid to have fun in your code
    
## Code
  - "No code is faster than no code"
  - Do less stuff
  - Object mappers take a lot of time to reconstitute objects, for example
    - Use them where you need fast CRUD, don't when you want to go further off the reservation
  - But, don't be afraid to write code!  DRY is not the be-all end-all.
    - Writing code is how you learn
    - Use `bundle open` or `gemedit` or Github's T shortcut
  - Don't be afraid to fail - risks are low, not that hard to redo if you mess up
  - Use feature toggles, dark reads, double writes, etc if you want - they can be super handy
  - Don't roll back, roll forward
  
- "...It takes a touch of genius and a lot of courage to move in the opposite direction" - Einstein