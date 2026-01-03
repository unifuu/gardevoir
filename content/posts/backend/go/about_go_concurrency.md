---
title: About Go
date: 2026-01-03
summary: 📝 Quick notes on Go concurrency...
tags:
  - backend
  - go
  - concurrency
---
## Context
### Purpose
- Lifecycle & Cancellation: control the lifecycle of goroutines
	- Cancel
	- Timeout
	- Propagate signals to all child goroutines
- Data Passing: pass request-scoped data
	- Every API request should have a context
	- Pass Request ID / Trace ID / User ID, etc