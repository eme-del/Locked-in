# Locked-in: Intended System Architecture

Locked-in is built with a modular, microservice architecture which ensures scalability and speed.

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

## Why This Approach Is Technically Feasible:
* Each microservice scales independently
* Redis caching and WebSockets communication ensure fast response with minimal latency.
* Cloud-based infrastructure offers auto-scaling, hence it is cheaper in the long run.
* Security stacks like OAuth2 offer strong protection for user data
* Modular architecture allows for multiple updates without downtime and easier feature integration
