#### 7th
- Homework
  ##### Designing a Memcached Cluster for Caching Delivery Timelines
  
  
  ###### Context
  Service T processes requests to retrieve delivery timelines for goods. These requests occur in search results, catalog pages, and delivery cost calculations. Since computing delivery timelines takes significant time, the service cannot retrieve them in real time (on-hit) and always relies on caching.
  
  
  ###### Initial Data
  Input parameters: User location + Warehouse IDs (ranging from 50 to 2,000 per request).
  Total locations and warehouses: 30,000 locations and 150,000 warehouses
  Load:
    - Daytime: 85–100k RPS
    - Peak (sales events): 180–210k RPS
  Request distribution by location:
    - Moscow – 40%
    - Saint Petersburg – 20%
    - Other cities – 40%
  Per request:
    - Minimum 5 warehouses (out of 20).
    - Remaining warehouses belong to sellers.
  Performance requirements:
    - P90 RT ≤ 20ms
    - P99 RT ≤ 50ms
  
  
  ###### Task
  
  Design a caching strategy and load distribution for Memcached to handle peak loads of 210k RPS. Describe your architecture, explaining:
  
  - Which Memcached cluster will you choose? (size, number of instances, CPU/RAM)
  - How will data be distributed across nodes? (consistent hashing, replication, key distribution)
  - How will TTL be managed? (cache refresh strategy, auto-refresh, or eviction)
  - What strategy will handle hot keys? (considering popular locations — Moscow & Saint Petersburg)
  - What fault-tolerance mechanism will be used? (recovery strategy in case of failures)
  - How will request balancing between clients and Memcached be organized? (local cache, intermediate layers)
