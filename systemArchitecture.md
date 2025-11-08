# Locked-in: Intended System Architecture

Locked-in is intended to be built with a modular, microservice architecture which ensures scalability and speed.

## Frontend Stack:
* React Native - Ensures a smooth mobile experience across various iOS and Android devices.
* React - Ensures usability on desktop and other pc devices
* Tailwind CSS - Allows for a stylish display across devices

*HTTPS and WebSockets for real-time updates between each aspect*

## Backend Stack:
Core Framework: Node.js + NestJS

Microservices:
* Feed - Combines updates from user feedback
* Podcasts Service - Streaming links, metadata management
* User Service - Authentication, profiles
* Notification - Push notifications via AWS
* Analytics - Collects and stores behavioural and performance data

*RabbitMQ for inter-service communication*

## Database Stack:
* PostgreSQL - Primary database for user data, posts, and goals
* Redis - Caching user sessions, feed data and goal streaks

## How Components Communicate:
* The mobile/web app sends a user request (lock-in, goal setting, etc.)
* The API Gateway routes the request to the appropriate microservice
* Microservice exchanges data asynchronously through RabbitMQ
* PostgreSQL stores structured data while Redis caches current data
* Analytic Services logs interaction to BigQuery for insight

*Below is a basic representation of the System Intercommunication Diagram*
```
                      +---------------------------+
                      |        Mobile App         |
                      |     (React Native + RN)   |
                      +-------------+-------------+
                                    |
                                    | HTTPS / WebSocket
                                    v
                      +---------------------------+
                      |       Web App (React)     |
                      +-------------+-------------+
                                    |
                                    v
                         +-------------------+
                         |   API Gateway     |
                         | (REST + WebSocket)|
                         +---------+---------+
                                   |
         ---------------------------------------------------------
         |           |            |           |          |        |
         v           v            v           v          v        v
 +---------------+ +---------------+ +---------------+ +---------------+ +---------------+
 | User Service  | | Feed Service  | | Podcast Svc  | | Notification  | | Analytics Svc  |
 | Auth, Profile | | Posts, Feeds  | | Streams, Meta| | Push via AWS  | | BigQuery       |
 +-------+-------+ +-------+-------+ +-------+-------+ +-------+-------+ +-------+-------+
         |                 |                |                |                |
         |                 |                |                |                |
         ------------------- RabbitMQ Message Bus ----------------------------
                                   |
                                   v
                       +------------------------+
                       | PostgreSQL (Primary DB)|
                       +------------------------+
                       | Redis (Cache)          |
                       +------------------------+
                                   |
                                   v
                           +------------------+
                           |   AWS S3 Storage |
                           +------------------+
                                   |
                                   v
                           +------------------+
                           | Google BigQuery  |
                           +------------------+


Data Flow Summary
1. User Request: From mobile/web → API Gateway.  
2. Routing: Gateway directs to correct microservice (e.g., Feed, User, Goal Tracking).  
3. Async Messaging: Microservices communicate via **RabbitMQ**.  
4. Persistence: Data saved in **PostgreSQL**, cached in **Redis**.  
5. Media Storage: User uploads go to **AWS S3**.  
6. Analytics Logging: Behaviour data to **Google BigQuery**.  
7. Notifications: Triggered by Notification service → AWS push.  
8. Real-time updates: via WebSockets to client apps.
```

## Why This Approach Is Technically Feasible:
* Each microservice scales independently
* Redis caching and WebSockets communication ensure fast response with minimal latency.
* Cloud-based infrastructure offers auto-scaling, hence it is cheaper in the long run.
* Security stacks like OAuth2 offer strong protection for user data
* Modular architecture allows for multiple updates without downtime and easier feature integration   
