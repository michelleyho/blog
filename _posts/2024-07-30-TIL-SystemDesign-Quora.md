---
title: "System Design: Quora"
categories: TIL
author: "Michelle Ho"
meta: "TIL"
date: 2024-07-30
---
### What is it?
- Social Platform
- Question and Answer Forum

### Functional Requirements
- Ask questions
- Give answers
- Posts can contain text, image, videos
- Upvote/Downvote/Comment on posts
- Search posts
- Recommendation on interest topics
- Rank posts

### Non Functional Requirements
1. Availability:
  - Highly availble to read/write posts
2. Scalability:
  - Can have large number of users, posts, etc
3. Durability/Consistency
  - Eventual consistency - different users should see same post contents. Ok if not see immediately updated post.
4. Performance
  - Low latency - shouldnt be huge delay in seeing posts/video/images

#### System Components 
- Authors/Subscribers
- Posts
  - Write/Comment
  - Upvote, downvote

#### Information Flow
1. User -> Quora
     - write, read, comment, upvote
2. Quora -> user
    - return search results
    - recommend topics
    - rank topics
    - 
### Building Blocks
1. Load Balancers - balance traffic for service hosts
1. Database
  - Posts/Comments
  - User data/relationships
2. Blob storage - video/image content
3. Cache - for quick retrieval of posts/answers

### Process Flow
1. User -> Load Balancer
2. Load Balancer -> web server
3. Web server -> application server
4. Application server
   - -> Blob Storage for media posts
   - -> Database for posts
5. Database can be attached to ML engine for recommendation/ranking processing

### Database Choice
1. Relational database - MySQL
  - for critical data - poss, answers, comment, votes. - Higher consitency, ACID property. Structured data
2. Non relational database - HBase
   - for stats  (page views, page, rank, extracted data for ML recommendation system)
   - Stats require large amt of data/info for data processing.  So want high read/write throughput so can obtain large quantity data at once.

### Cache options
1. CDNs - for media posts
2. Memcache - Store frequencly accessed data for lower latency
3. Redis - store counter info because allows in - store increment updates.

### Compute Servers
- computations needed for recommendation/ranking services.

### APIs


