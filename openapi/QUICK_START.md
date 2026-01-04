# SalesOS Platform API v3.0.0 - Quick Start Guide

**Version:** 3.0.0 | **Date:** January 4, 2026

---

## 1. Get Started in 5 Minutes

### Step 1: Authenticate

```bash
# Login with Auth0
curl -X POST https://api.play2sell.com/oauth/token \
  -H "Content-Type: application/json" \
  -d '{
    "grant_type": "password",
    "client_id": "YOUR_CLIENT_ID",
    "client_secret": "YOUR_CLIENT_SECRET",
    "audience": "https://api.play2sell.com",
    "username": "user@example.com",
    "password": "your_password",
    "scope": "openid profile email"
  }'
```

### Step 2: Exchange Token

```bash
# Get Supabase token
curl -X POST https://api.play2sell.com/functions/v1/token_exchange_edge_function \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_AUTH0_TOKEN" \
  -d '{
    "auth0_token": "YOUR_AUTH0_TOKEN"
  }'
```

### Step 3: Make Your First Request

```bash
# List your leads
curl -X GET "https://api.play2sell.com/rest/v1/salesos_opportunities?select=*&limit=10" \
  -H "Authorization: Bearer YOUR_SUPABASE_TOKEN" \
  -H "apikey: YOUR_ANON_KEY"
```

---

## 2. Key Endpoints by Use Case

### Create a Lead

```bash
POST /rest/v1/salesos_opportunities
Authorization: Bearer YOUR_TOKEN
Content-Type: application/json
Prefer: return=representation

{
  "tenant_id": "YOUR_TENANT_ID",
  "customer_name": "João Silva",
  "customer_phone": "11999999999",
  "customer_email": "joao@example.com",
  "origin": "whatsapp",
  "priority": "medium"
}
```

### Get AI Suggestions

```bash
POST /functions/v1/copilot-suggest
Content-Type: application/json

{
  "tenant_id": "YOUR_TENANT_ID",
  "lead_context": {
    "stage": "qualification",
    "notes": "Cliente interessado em apartamento 2 quartos",
    "product": "Apartamento MCMV",
    "buying_moment": "warm"
  }
}
```

### Emit Event

```bash
POST /rest/v1/rpc/salesos_emit_event
Authorization: Bearer YOUR_TOKEN
Content-Type: application/json

{
  "p_user_id": "USER_ID",
  "p_tenant_id": "TENANT_ID",
  "p_type": "lead.created",
  "p_domain": "leads",
  "p_payload": {
    "lead_id": "lead-123",
    "lead_name": "João Silva"
  },
  "p_source": "application"
}
```

---

## 3. Common Queries

### Filter Leads by Stage

```
GET /rest/v1/salesos_opportunities?stage_id=eq.STAGE_ID&order=created_at.desc
```

### Get Recent Events

```
GET /rest/v1/salesos_events?select=*&order=created_at.desc&limit=50
```

### List Active Workflows

```
GET /rest/v1/salesos_workflows?status=eq.active
```

### Get User Scores

```
GET /rest/v1/salesos_user_scores?user_id=eq.USER_ID
```

---

## 4. Environment Setup

### Production
```
BASE_URL=https://api.play2sell.com
```

### Staging
```
BASE_URL=https://staging-api.play2sell.com
```

### Local Development
```
BASE_URL=http://localhost:5173
```

---

## 5. Rate Limits

| Resource | Limit |
|----------|-------|
| API Calls | 1000/min |
| Events | 10000/day |
| Webhooks | 100/min |
| Copilot | 100/hour |

---

## 6. Response Codes

| Code | Meaning |
|------|---------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Server Error |

---

## 7. PostgREST Operators

```
?field=eq.value          Equal
?field=neq.value         Not equal
?field=gt.value          Greater than
?field=gte.value         Greater or equal
?field=lt.value          Less than
?field=lte.value         Less or equal
?field=is.null           Is null
?field=not.is.null       Not null
```

---

## 8. Testing with Postman

1. Import collections from `/postman/` directory
2. Set environment variables:
   - `base_url`
   - `anon_key`
   - `auth0_domain`
   - `auth0_client_id`
   - `auth0_client_secret`
3. Run "Auth Flow" collection first
4. Use saved tokens in other collections

---

## 9. Documentation Files

- **Full API Spec**: `salesos-api-v3.0.yaml` (86KB)
- **README**: `README-v3.0.md` (17KB)
- **This Guide**: `QUICK_START.md`
- **Summary**: `COMPLETION_SUMMARY.md` (11KB)

---

## 10. Support

- **Email**: dev@play2sell.com
- **Docs**: https://docs.play2sell.com
- **Spec File**: `/openapi/salesos-api-v3.0.yaml`

---

**Ready to integrate?** Start with the Authentication flow, then explore the 110+ endpoints! 🚀
