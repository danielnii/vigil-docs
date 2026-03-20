# High-Level System Design

## Architecture Overview

Vigil follows a flat MVC pattern with no service layer or DTOs. This was a deliberate decision for speed and direct debugging.

### Backend (Node.js + Express)
- Routes → Controllers → PostgreSQL (direct queries)
- No ORM (raw SQL for complex analytics)
- JWT authentication with role-based access
- Winston logging with daily rotation

### Frontend (React + React Query)
- React Query for server state (replaced Redux)
- Context API for auth state
- Tailwind CSS for styling
- Recharts for analytics

### Infrastructure
- Docker containers (API + PostgreSQL)
- Cloudflare tunnel (no open ports)
- Oracle Cloud VPS (Ubuntu 24.04)
- Vercel for frontend hosting
