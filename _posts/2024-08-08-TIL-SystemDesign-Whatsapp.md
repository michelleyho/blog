---
title: "System Design: Whatsapp"
categories: TIL
author: "Michelle Ho"
meta: "TIL"
date: 2024-08-08
---

### What
- messaging service connecting millions of people worldwide
- 
### Functional Requirements
- messages: 1:1, group messages
- send/share text, media (image/videos)
- Acknowledgement of messages sent/recieved/read
- Chat storage : persistent storage of chat messages, media files, etc
- notifications of messages.


#### Nonfunctional Requirements
- latency - low latency, esp during real time chat
- scalability - account for billions of users
- consistency - data/files should be consistent, esp across different devices.
- Availability - highly avaialbitly but not at the expense of consistency
- security - messages should be secure, end to end encryption.  Only parties involved in messages can see messages.

  
#### Building Blocks
- load balancers
- databases
- blob storage
- CDN
- cache
- message queues


#### Flow
1. user 1 and user 2 communicating
2. user1 sends message to intermediate chat server.
3. Chat server responds to user 1 that it got the message
4. Chat message either sends to user 2 (if online) or save in server/database if user 2 is offline.
5. User 2 (when online) receives message, sends notification acknowledgement to chat server.
6. Chat server notifies user 1 that user 2 got message
7. User 2 reads message, and notifies chat server.
8. Chat server notifies user 1 that user 2 READ message

#### Detail design
#### Components
- load balancers
- websocket servers
- websocket managers
- Mnesia database cluster
- various services (message service, group service, asset service)
- Kafka - for group messages
- CDN/blob storage/MySQL user group DB server/Redis


#### Services
- Websocket manager
- group mmessage handler
- message service
- asset service
- user service
- group service

#### Overall Steps
1. User 1 communicates with its own websocket server.
2. websocket server 1 communicates with websocker manager to determine which websocket server user 2 is connected to. websocket manager tells websocket server1 is websocket server 2 (of user 2) is online
3. Websocket server 1 sends message to message service, where its stored in teh msneia database, FIFO processing. When gets sent to websocket server2, will then be dequeued and deleted from mnesia database cluster.
4. if user 2 was previously offline, and now online, messages from mnesia cluster will sent via push notification to webserver 2.
- If media was involved:
   - media is compressed and encrypred and passed to asset service to store in blob storage.  Asset service issue ID # for media, hashes it to avoid duplicates.
   - For reciever (user 2) to get the media, was given an ID as part of the message, to go retrieve from blob storage/ or CDN/cache.
- If group messaging is involved:
  - message service sends message to Kafka to process publisher/subscribers
  - group service - tracks all metadata on group (user id, group id, description, icon, etc). Info sits on top of a mysql DB cluster, that can be replicated adn distributed globally.
  - group message handler communicates with kafka and group service.  Then passes on message like usual to all the recipient group users.

#### Things to watch out for:
- security
- prioritize consistency over availabiity. Crucial that messages are delivered and in correct order
- Pritorizte security over latency.  Make sure messages are encrypted/decrypted accordingly even if it increases latency a bit.
