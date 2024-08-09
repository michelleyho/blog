---
title: "System Design: Type Ahead Suggestion"
categories: TIL
author: "Michelle Ho"
meta: "TIL"
date: 2024-08-09
---

### What
- autocomplete feature.
- enable search for known and frequent queries
- provide list of suggestions to complete query.. based on user history, current context of search, trending topics.
- helps enhance user experience.

### Functional Requirements
- suggest top N terms that are frequent and relevant according to user, based on what's being typed.
- Store new search queries that are popular and recent. 
  
#### Nonfunctional Requirements
- Latency - low, show be in realtime as the user types into search box
- fault tolerant - system should be reliable enough to work and provide suggestions.
- scalability - high, be able to support increasing number of users for this service.

  
#### Building Blocks
- Load balancer - disseminate incoming queries amongst active servers for search.
- Database - to keep related to query's prefixes
- Cache - keep top N suggestions for quick retrieval.

#### Flow


#### Detail design
#### Components


#### Services


#### Overall Steps


#### Things to watch out for:
