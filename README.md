# PulseView Analytics

A small end-to-end web analytics platform built for learning and technical interviews.

The system tracks user events, builds conversion funnels, and generates actionable insights
using a microservices architecture.

---

## What does the system do?

- Collects frontend events (page views, clicks, custom events)
- Aggregates funnel metrics (conversion & drop-off)
- Generates product insights and recommendations
- Displays everything in a simple web dashboard

---

## Architecture

The system is composed of several independent services running in Docker containers,
with Nginx acting as a gateway and static UI server.

```mermaid
flowchart LR
  U[User Browser] -->|HTTP :8080| N[Nginx (web)]
  N -->|serves static UI| UI[HTML + JS]

  N -->|/authapi| A[auth-service]
  N -->|/api| E[events-service]
  N -->|/insightsapi| I[insights-service]

  A --> P[(Postgres)]
  E --> P

  I --> E
```
---

## Services overview

### auth-service
Handles user registration and login.  
Issues JWT tokens used to authenticate API requests.

### events-service
Receives tracking events and stores them in PostgreSQL.  
Calculates funnel statistics per site and time range.

### insights-service
Consumes funnel data and generates human-readable insights,  
including severity, summary and recommendations.

### web (nginx)
Serves the frontend dashboard and proxies API requests  
to internal services.


## Services overview

### auth-service
Handles user registration and login.  
Issues JWT tokens used to authenticate API requests.

### events-service
Receives tracking events and stores them in PostgreSQL.  
Calculates funnel statistics per site and time range.

### insights-service
Consumes funnel data and generates human-readable insights,  
including severity, summary and recommendations.

### web (nginx)
Serves the frontend dashboard and proxies API requests  
to internal services.

## Start the system
docker compose up --build



---

```md
## How to demo the system

1. Open the funnel dashboard in the browser  
   http://localhost:8080/funnels.html
2. Login using the auth form
3. Select a site and time range
4. Send events (page_view, click, signup_completed)
5. Refresh the dashboard to see:
   - Funnel steps
   - Conversion rates
   - Insights and recommendations


## Engineering highlights

- Microservices architecture with clear separation of concerns
- JWT authentication propagated through an Nginx gateway
- Funnel aggregation based on unique visitors
- Insights service decoupled from raw analytics logic


