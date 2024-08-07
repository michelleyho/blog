---
title: "System Design: Twitter"
categories: TIL
author: "Michelle Ho"
meta: "TIL"
date: 2024-08-06
---

### What
- social platform
- share "breaking" news, posts

### Functional Requirements
- View timeline
- Create Posts - CRUD
- Posts are text, images, videos
- Text are limited in terms of # of characters
- Comment/like posts
- Follow users... user/subscriber model
- search posts - keywords, hashtags, users

#### Nonfunctional Requirements
- Availability - high. should be able to see feed at all times.
- scalability - high. large influx of users, followers, likes/retweets, etc.  More read heavy than write heavy.
- reliability - all viewers should see the same posts. posts shouldnt be "lost"
- latency - low, should be able to see posts asap
- consistency - eventual consistency.. response to posts should be immediate for closer regions, and eventually propagate through.
- 
#### Building Blocks
- load balancer
- seqeuencer - for generating Tweet ID
- database - metadata of users and tweets.
- blob storage - images/videos of tweets
- key value store - indexing/counters to update values/counters of user tweets.
- CDN - for image, videos, quick response for closer geographic location
- Blob storage for large media posts.
- pub-sub - real time processing - elimination of redudncate data, organzing data.. distribution of posts to followers?
- sharded counters - count of various features (views, likes, retweets, etc)
- cache - most requestsed and recent data in RAM for quick responses
- CDN - reduce latency, get posts closer based on geolocation
- 
#### Flow
- User contacts DNS - to get redirection IP addressed to corresponding twitter serveice
- User contact load balancer  when want to post tweets, load balancers balances traffic and choosed availbel server to process tweet.  tw
- User contacts CDN to get low latency search/lookup of tweets.
- Application server - provide various services and logic needed to orchestrate different components to meet functional requirements
- 
#### Detail design
#### Components
- blob store - store photos/videos associated with tweets
- SQL DB - strong consistency for tweets
- Kafka - handling deduping of data (like duplicate posts, etc). convert kafka topics to pub sub topics
- graph database - maintain relationship of user/followers. to help with notifications.
- apache lucerne - for search indexing of posts,
- distributed counters - burst write request (increment/decrement) against various interactions on Tweets from accounts with many followers.  The tweets with more interction (likes) becomes top K problem, over a certain timeframe , making them local or global hashtag trends

#### Services
- Tweet - contact tweet service.  Finds tweets attachments and puts images/videos to blob store. Text and other metadata in other databases (ex: SQL databases).  Real time tweet processing - user interaction data, metrics, logs, etc - wtih apache kafka Pub-sub service to dedupe data
- Timeline - to view timeline for latest tweets, first search nearest CDN for static data. If requested data not found, go to differnet databases and stores to find top K tweets. Collect varous interaction of tweets from different sharded counters to find Top K. 
- Search - look into index servers to find all tweets containing requested keywords. Based on factors like time, location, rank the tweet reults, return/display top ones.
- 
