---
title: About Node.js
date: 2025-12-22
summary: 📝 Quick notes on Node.js...
tags:
  - backend
  - javascript
---
## Definition
- Node.js is a JavaScript runtime for the backend:
	- Originally:
		- JavaScript could only run in the browser.
	- With Node.js:
		- JavaScript can run on **servers**, local machines, and CLI tools.
- So Node.js lets you write backend code using JavaScript (or TypeScript).
- Node.js is mainly backend, not frontend, but it supports frontend development.
## Responsibility
- Backend APIs:
	- REST APIs
	- GraphQL APIs
	- Authentication (JWT, OAuth)
	- Business logic
	- Connect to DB / Redis / Message Queue
```
Client → HTTP → Node.js → Database
```
- Real-time Systems
	- Event-driven
	- Non-blocking (async I/O)
	- Single-threaded but highly concurrent
- BFF: Backend for Frontend
	- Aggregates data from multiple services (microservice architectures)
	- Shapes data specifically for frontend needs
- Frontend Tooling (NOT frontend itself)
	- Vite
	- Webpack
	- ESLint
	- NPM / YARN
- Full-Stack / SSR (Next.js, Nuxt)
	- Next.js (React)
	- Nuxt (Vue)
## Benefits
- Same language everywhere (JS / TS)
	- Faster onboarding
	- Shared types (DTOs, schemas)
	- Easier full-stack development
- Excellent for I/O-heavy workloads
	- High concurrency
	- Many fast requests
	- Real-time connections