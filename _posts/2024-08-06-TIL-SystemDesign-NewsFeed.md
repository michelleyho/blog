---
title: "System Design: Newsfeed"
categories: TIL
author: "Michelle Ho"
meta: "TIL"
date: 2024-08-06
---

### What
- social media platform where list of posts/entiites are created by users, and consumed by followers.
- Text, image, videos
- comments, likes/dislikes, upvotes/downvotes
- continuous updates of these posts on users' homepage based on their preference (who/what they follow , users, topics, etc)
- 


### Functional Requirements
- generate timeline (get relevant info based on followers, connections, groups, etc)
- rank to see what's most relevant, or based on X criteria, and display on homepage timeline.
- interact with posts (like, dislike, comment, forward, etc)
- May be text, image, video


#### Nonfunctional Requirements
- Availability - high. should be able to see feed at all times.
- Scalavility - be able to handle large influx of new posts
- latency - should see most relevant posts/info asap
- fault tolerant - high, be able to handle large of amounts of data..
- If based on CAP theroem, prioritze on availability
- 

#### Building Blocks
- Load balancers - to distribute millions of user requests for newsfeed
- Database - store metadata of users (followers, following - graph database), track posts (SQL for durability and no loss of post entries)
- cache - lower latecny of frequently accessed data.
- CDN - lower latency for posts locally closer to user
- blob storage - handle large media files (images, videos)
- sharded counters - track interact of posts to aid with top k results, to help with ranking of posts.

#### Flow
- feed generation - created by aggregating ranked friends/followers posts 
- feed publishing - relevant data for the feed is written into cache and database.  THis is then used to populate a user's newsfeed.

#### Detail design
#### Components
- user
- load balancer - directs user to one of the webservers
- webservers - redirects traffice to application servers for corresponding needed service
- various services
- newsfeed cache
- blob storage
- post cache
- post database
#### Services
- notification service
- newsfeed generation service
- newsfeed publishing service
- post service
