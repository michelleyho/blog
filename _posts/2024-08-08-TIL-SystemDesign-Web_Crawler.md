---
title: "System Design: Web Crawler"
categories: TIL
author: "Michelle Ho"
meta: "TIL"
date: 2024-08-08
---

### What
- Internet bot that systematically scours/traverses the web, starting from some "SEED" urls. 
- Crawling - Process of acquring content from the web. 
- Storing - efficiently saving data while crawling for post processing. Stored data is used for indexing and ranking. 
- 
### Functional Requirements
- Crawl - Given a list of seed URLS, score the web from those starting points.
- Store - Extract and store content of URL in a blob store
- Make stored content processable by search engines for indexing and ranking
- Schedule - regular update blob stores based on scheduled crawls.


#### Nonfunctional Requirements
- Availbility - not that important?
- Scalability - high, able to account for more sites to crawl.  Be distributed and multithreaded to crawl millions of pages.
- consistency - should be consisyernt data amongst differnet crawlers
- performance - limit crawling within a domain to keep optimal crawling. Self-throttling.
- extensibility - extensible to other network communication protocols, and addiitonal store formats
  
#### Building Blocks
- scheduler - schedule crawling events
- DNS - get IP address resultion
- Cache - store fetched documents for quick access by all processing modules
- blob store - store crawled content

#### Flow

#### Detail design
#### Components
- scheduler:
  - priority queue - queue that hosts URL ready to be crawled.  URLS can be given priority, and update frequency
  - database - stores all the URLs.  Database populated by new requests from users added URLs from seed and runtime added ones, and crawlers extracted URLS (internal links witin a page)
- DNS resolver - map hostnames to IP addresses. Customized ones to cache frequent ones.
- service host
- html fetcher - initiates communication wirh server hosting the URL. downloads file content based on underlying communication protocol (HTTP, or others)
- 

#### Services
- Service host
  - hosts a bunch of workers who do the crawling. Brains of the overal system.
  - multiworker system where each worker instance communicates with URL priority queue to dequeue next avaialbel URL to be crawled.
  - Each worker responsible for DNS resolution
  - Eacher worker is gateway between scheduler and html fetcher.  Sends resolved DNS info to HTML fetcher for processing.
- 

#### Overall Steps
1. Worker instance assigned a URL task. Achieved by getting a URL from the URL priority queue.
2. Worker does DNS resolution... send to dns resolver or look through cache
3. start communication with the HTML (or other protocol) fetcher. Worker instance forwards URL and ip address to HTML fetcher.
4. worker extracts URLS and the HTML document from page. Puts document in cache for other components to process.
5. Dedupe testing - worker sends extracted URLS, and documenet to deduper to eliminate duplicates.  Duplicate eliminator calculates hash of the extracted URLS, and the document page. Computer hash sum is checked if already in storage.  If match (duplicate), discard. If no math, place new checsums into respective data stores (one for url, one for docuiment page). Green lights extractor to store the content.
6. extractor sends newly discovered URLs (to be crawled later) into the URL prioirty queue. Updates new element with with url, priority, and frequency of how oftren to crawl.
7. extractor writes portions of newly discovered documents from the document input stream into the database.
8. Entire recrawl process starts over with the next element in the priority queue. 

#### Things to watch out for:
- web crawler traps - pages that will infinitely drain the crawling resource.. (cyclic pages, etc)
- 
