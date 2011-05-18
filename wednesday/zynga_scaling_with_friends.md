# Geof Dagley - Scaling with Friends (Zynga)
- Products are Chess With Friends and Words With Friends, Free/Paid, iPhone/iPad/Android
- Lots of celebrity endorsements - can be interesting when they bring massive bursts in traffic
- Launched in June/July 2009
- Been #1 on the app store a few times
- Started by two brothers in the McKinney, TX public library
## Why Rails?
- Rails is sexy
- Founders were just learning iOS and server development
## Service Constraints
- Backwards compatibility
  - limit the forced upgrade experience
  - Can force the issue from the server side but try not to because it annoys users
- iPhone client built around XML (AR#to_xml)
  - Prior to ObjectiveResource
  - Lots of custom code
- Supporting multiple game types from the same service
  - Chess, Words, etc.
  - May not always be the same
## Organic Growth
- Simplest thing possible
- Stick to YAGNI principle
- Especially important in servers, schemas, and architecture
- Team size has grown - backend mostly contractors (10), 3-4 iOS, 1-2 Android
- Try to cross-train teams so folks can jump in and help

## Hosting
- Started at Slicehost
  - Single DB server, single app server
  - Added more app servers but virtualization killed
- Moved to RailsMachine
  - Combo of virtualized app tier and bare-metal DB tier
- Moved to SoftLayer
  - 100% bare-metal to support growth
  - USe puppet for configuration and deployment
- Moved to Zynga
  - "Like strapping on a jetpack"
  - Own hosting platform, need servers and they're just there

## MySQL growing pains
- MyISAM vs. InnoDB
  - MyISAM table locking on inserts and updates
  - InnoDB row locking on inserts and updates
  - Huge perf gain
- Movepocalypse
  - Overflowing integer ID for moves - both DB and iPhone needed changes
  - 6/21/2010
  - Required a forced update
  - Ended up partitioning multiple tables based on IDs
    - Not enough time for sharding
    - Crazy table names 
  
- Write bound
  - 50/50 on read/write traffic
  - Cached columns save queries but multiply writes (4 per move)
  - Multiple by 1000 moves per second and it gets messy
  - Ended up moving to memcache
  
- Sharding
  - Spread load across multiple DBs
  - Most tables don't ned it, but can rebalance a bit
  - Horizontally scalable, which is nice
  - Had to move to a unique ID generator service (in Redis)
  
- Caching
  - Early on: no caching
  - Then: cache to relieve bottlenecks
  - Now: cache as much as humanly possible
  - Using the cache_money plugin (Rails 2.3)
  - Be careful with expiration times - letting it roll may be better
  - Watch out when growing the cache pool - may get stale data
  - Resque for background work
    - APN notifications, FB queries, etc.

- Cautiously roll out new features
  - Problems reveal themselves at scale
  - Slowly roll out features (rollout gem)
  - Activate for small percentages at a time
  - Go back and remove feature flags after full rollouts
  
- Instrument the little things
   - 30-60ms response times
   - 5ms response time savings may be significant
   - How fast are the to_xml methods?  Compare to string concats?
    - Use NewRelic method tracers to get better definition
    
- Deploy with confidence
  - No tests == no confidence
  - Multiple clients, backwards compatibility
  - Not 100% coverage
  - All new and refactored code gets tests
  - Use cucumber and RSpec
  - As a dev, can't necessarily think of wide-reaching implications for small changes
  
- Fail early
  - Client vs. server release schedules
    - Nice to have the agility of deploying anytime
  - Deploy and monitor
  - Be ready to rollback if needed
  - Use git tags to roll back and forward
  - Don't deploy before you want to go home!
  - Use NewRelic for app monitoring
  - Use Scout for server monitoring
  - Complementary products
  - Write a lot of custom reports

- During this talk:
  - Thousands of users started playing
  - Tens of thousands of new games were started
  - Millions of moves were made