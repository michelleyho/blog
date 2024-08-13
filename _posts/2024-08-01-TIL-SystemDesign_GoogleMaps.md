#### Functional Requirements
- view maps
- Identify current location.
- Recommend fastest route
- 
- get directions - find best possible path to destination
- provide time to make trip

#### NonFunctionational Requirements
- high availability - users should be always able to pull up maps and access it.
- low latency: User should be able to get information as quickly as possible, esp if navigating.
- highly scalable - should accomodate large intake of users accessing maps
- consistency - high, the maps informtaion shouldnt change with each view.


#### Building Blocks
load balancers - distribute user requests

key value store - store metadata information

pub-sub - generate/handle important events during navigation, notify corresponidng services

database - NoSQL (graph database) to store road data from various sources in form of a graph

distributed search - search on various locations on map. 

#### Components
- location finder - find user's current location
- route finder - service to find paths between two locations
- navigator - tracks users journey, sends updated directions/notifications to users if deviate path
- GPS/WiFi/cellular - find users ground position
- distributed search - convert place names to longitude/latitude
- area search service - gets location from distbributed search, then use graph processing service to find optimal path
- graph processing service - shortest path algorithm
- database - store road network graph
- pub-sub - listens to events from various services, triggers another service to find new route. collects location data from many users to help determine traffic patters.
- 3rd party road data - used to get information for the graph database
- 
