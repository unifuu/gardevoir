---
title: Terminology Wiki
date: 2025-12-31
summary: 📰 Some terms...
tags:
  - wiki
  - terminology
---

## Cloud Computing

### Low-Code Platform

- A low-code platform lets users build applications with minimal coding using visual tools.

### PaaS

- PaaS (Platform as a Service)
- A cloud model that provides a ready-to-use platform to build and run applications.
- Examples: Heroku, Google App Engine, Azure App Service.

## DevOps

### DevSecOps

- Integrating security into DevOps from the start.
- It makes security **a shared responsibility** and automates security checks throughout the CI/CD pipeline, so software can be delivered fast and securely.

## LLM

### MCP

- MCP: Model Context Protocol
- Introduced by Anthropic, MCP is a newer standard designed to connect AI Models (like Claude or GPT) to Data Sources and Tools.
  - The Problem: AI models are often "trapped" without access to your local files, databases, or specific APIs unless developers write custom, one-off integrations.
  - The Solution: MCP provides a universal way for AI assistants to securely fetch data or use tools. If a service (like Google Drive or GitHub) creates an MCP server, any AI client can instantly understand how to use it.

## Marketing

### ASP: Affiliate Service Provider

- A company or platform that runs and manages an affiliate marketing system.
- It connects advertisers with affiliates, tracks performance, and handles commission payments.

```
Advertiser (Merchant)
│ pays commission
▼
Affiliate Service Provider (Platform)
│ tracking links / reporting
▼
Affiliate (Publisher)
```

## Monitoring

### Prometheus

- A monitoring system.
- It collects and stores metrics from systems and applications.

## Production

### MVP: Minimum Viable Product

- The smallest version of a product that is usable and delivers real value to users, so you can validate assumptions quickly with minimal cost.
- Build the minimum feature set needed to test whether your idea is worth continuing.

## Sprites

### Live2D & Spine 2D

- Live2D
  - Engine: Live2D Cubism SDK
  - `.moc` or `.moc3`
- Spine 2D
  - `.skel` or `.json`
  - Engine: PixiJS + Spine Runtime

## Storage

### Ceph

- An open-source distributed storage system that combines the disks of many servers into one reliable storage pool.
- It automatically replicates data, handles failures, and scales by adding more machines, providing object, block, and file storage from the same system.

## Tooling

### LSP

- LSP: Language Server Protocol
- Developed by Microsoft, LSP standardizes how Code Editors (like VS Code) communicate with Programming Languages.
  - The Problem: Before LSP, every editor had to write unique code to support "Auto-complete" or "Go to Definition" for every single language.
  - The Solution: LSP acts as a middleware. A "Language Server" handles the heavy lifting of analyzing code, and any LSP-compliant editor can simply "plug in" to get full language support.

```markdown
[ Editor ]
↓ LSP
[ Language Server ]
```

## Visualization

### Grafana

- A visualization tool.
- It visualizes metrics using dashboards and charts.
