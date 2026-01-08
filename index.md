---
title: SalesOS API Documentation
---

# SalesOS Platform API

Welcome to the SalesOS API documentation. This API powers the event-driven sales automation platform with gamification.

## Quick Links

- [API Reference](/openapi/salesos-api.yaml) - Complete OpenAPI specification
- [Quick Start](docs/getting-started/quick-start.md) - Get started in 5 minutes
- [Event Types](docs/event-types.md) - All available event types
- [Authentication](docs/auth.md) - How to authenticate

## Features

- **EventService** - Emit and track sales events
- **Workflows** - Automate actions based on events
- **Gamification** - Points, leaderboards, and achievements
- **Webhooks** - Integrate with external systems

## Getting Started

```bash
curl -X POST 'https://api.play2sell.com/rest/v1/rpc/salesos_emit_event' \
  -H 'Authorization: Bearer YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
    "p_user_id": "YOUR_USER_ID",
    "p_tenant_id": "YOUR_TENANT_ID",
    "p_type": "lead.created",
    "p_domain": "leads",
    "p_payload": {
      "lead_name": "John Doe"
    }
  }'
```

## Support

- Email: dev@play2sell.com
- GitHub: [Play2sellSA/SalesOs-API](https://github.com/Play2sellSA/SalesOs-API)
