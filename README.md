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
  U["User Browser"] -->| "HTTP 8080" | N["Nginx (web gateway)"]

  N -->| "serves static UI" | UI["Static UI (HTML + JS)"]

  N -->| "/authapi" | A["auth-service (JWT)"]
  N -->| "/api" | E["events-service (tracking + funnels)"]
  N -->| "/insightsapi" | I["insights-service (recommendations)"]

  A --> DB["Postgres"]
  E --> DB

  I -->| "fetches funnel data" | E

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

---
## How to run locally

### Prerequisites

- Docker
- Docker Compose

### Environment variables

Create a `.env` file in the project root:

```env
POSTGRES_USER=pv
POSTGRES_PASSWORD=pvpass
POSTGRES_DB=pvdb
JWT_SECRET=change-me
JWT_EXPIRES_IN=1h
```

## Start the system
docker compose up --build

## The demo uses two browser pages:

1. **Demo Site (event generator)**  
   http://localhost:8080/index.html  
   Used to generate real user events (page views, clicks, signup completion).

2. **Analytics Dashboard**  
   http://localhost:8080/funnels.html  
   Used to visualize funnels, conversion rates, and insights.

## How to demo the system

1. Open the demo site  
   http://localhost:8080/index.html  
   Interact with the page (click buttons, complete signup).

2. Open the analytics dashboard  
   http://localhost:8080/funnels.html  

3. Login using the auth form

4. Select a site and time range

5. Refresh the dashboard to see:
   - Funnel steps updating in real time
   - Conversion rates
   - Automatically generated insights and recommendations



## Engineering highlights

- Microservices architecture with clear separation of concerns
- JWT authentication propagated through an Nginx gateway
- Funnel aggregation based on unique visitors
- Insights service decoupled from raw analytics logic


