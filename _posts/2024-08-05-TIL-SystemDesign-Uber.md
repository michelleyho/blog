### What
- Ride hailing service to users.
- Ability to register and book a vehicle to travel from one place to another
- driver and rider communication through app
- 
### Functional Requirements
- update driver location
- find nearby drivers
- request ride
- manage payments
- show driver ETA
- confirm pickup
- show trip update
- end trip

#### Nonfunctional Requirements
- Availability - high - needs to able to locate driver/riders, see trip progress,
- Scalability - account for spike in drivers/riders
- Reliability - service should be fast and error free. Ride requests and location update should happen smoothly.
- Consistency - driver and rider have consistent view of system
- Fraud detection - payment frauds should be detected.


#### Building Blocks
- Load balaancer - handle influx of ride requests
- Cache - to get info  fast as possible
- Database - hold metadata info of riders/drivers
- CDN - get local information asap


#### Detail design
#### Components/Services
- load balancer
- CDN
- Quadtree map service - divide map into segments, help find drivers.
- location manager: rider and driver connect to this to know where they are. Gets up to date information of where driver & riders are.
- request vehicle service
- find driver service

#### Databases
- Horizontally scalable DB - acount for new customers/drivers. Be able to add storage no problem.
- High read/write requests - driver location updated constantly.
- DB should never be down.
- Cassandra - high availablilty, high scalability,and fault tolerance.  data increases enormously.
- mysql - for individual trip data - highly relational data to be kept across multiple tables.

#### Tables needed
