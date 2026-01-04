# SalesOS Platform API v3.0.0 - Complete Specification

**Generated:** January 4, 2026
**Version:** 3.0.0
**Total Endpoints:** 110+
**OpenAPI Version:** 3.1.0

---

## Overview

This release represents the **complete, production-ready OpenAPI 3.1.0 specification** for the SalesOS Platform API, documenting all 110+ endpoints across 8 main domains extracted from the official Postman collections.

## Files

- **`salesos-api-v3.0.yaml`** - Complete specification with all 110+ endpoints (86KB)
- **`salesos-api.yaml`** - Main spec file (updated to v3.0.0) (6.7KB)
- **`POSTMAN_ENDPOINTS_INVENTORY.md`** - Source endpoint inventory

## What's New in v3.0.0

### Major Additions

- **92+ new endpoints** for complete API coverage
- Full **Copilot IA** documentation (14 endpoints)
- Complete **RBAC** and multi-tenant management (16 endpoints)
- **RAG Management** for AI knowledge base (13 endpoints)
- Comprehensive **Workflows & Automation** (15 endpoints)
- **Webhooks & EventService** integration (23 endpoints)
- Full **User & Auth** management (10 endpoints)
- Complete **Leads & Opportunities** lifecycle (14 endpoints)

### Documentation Improvements

- Professional descriptions for all endpoints
- Detailed request/response schemas
- Multiple examples per endpoint
- Security scheme documentation
- Error response standards
- Rate limiting information
- Environment configurations

---

## API Domains & Endpoints

### 1. Authentication & Auth Flow (6 endpoints)

Complete OAuth 2.0 flow with Auth0 integration and multi-tenant support.

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/oauth/token` | Auth0 login with password grant |
| POST | `/functions/v1/token_exchange_edge_function` | Exchange Auth0 token for Supabase JWT |
| POST | `/functions/v1/switch-tenant` | Switch active tenant context |
| GET | `/rest/v1/salesos_user_tenants` | List user's tenant associations |

**Key Features:**
- OAuth 2.0 Password Grant
- JWT token exchange
- Multi-tenant context switching
- Role-based access control

---

### 2. Copilot IA (AI Features) (14 endpoints)

AI-powered sales assistant with RAG, TTS, STT, and feedback mechanisms.

#### Suggestion Generation
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/functions/v1/copilot-suggest` | Generate AI suggestions using RAG |

#### Voice Features
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/functions/v1/copilot-tts` | Text-to-Speech conversion |
| POST | `/functions/v1/copilot-stt` | Speech-to-Text transcription |

#### Feedback & Analytics
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/functions/v1/copilot-feedback` | Submit user feedback |
| GET | `/rest/v1/salesos_copilot_sessions` | List AI sessions |
| GET | `/rest/v1/salesos_copilot_suggestions` | List generated suggestions |
| GET | `/rest/v1/salesos_copilot_feedback` | List feedback records |
| GET | `/rest/v1/salesos_copilot_cache` | List cached suggestions |
| GET | `/rest/v1/salesos_copilot_static_suggestions` | List fallback suggestions |

**Key Features:**
- RAG (Retrieval-Augmented Generation)
- Context-aware suggestions for:
  - Qualification scripts
  - Objection handling
  - Closing techniques
  - Follow-up messages
- Multi-language TTS/STT support
- Feedback loop for continuous improvement

---

### 3. Leads & Opportunities Management (14 endpoints)

Complete lead lifecycle from capture to closure.

#### Core CRUD
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_opportunities` | List/filter opportunities |
| POST | `/rest/v1/salesos_opportunities` | Create new lead |
| PATCH | `/rest/v1/salesos_opportunities` | Update lead |
| DELETE | `/rest/v1/salesos_opportunities` | Delete lead |

#### Interactions
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_lead_interactions` | List lead interactions |
| POST | `/rest/v1/salesos_lead_interactions` | Log new interaction |

#### Pipeline Management
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_context_stages` | List pipeline stages |

**Interaction Types:**
- call, whatsapp, email, meeting, visit, note, audio, photo, status_change

**Key Features:**
- Multi-stage pipeline
- Rich filtering (stage, owner, priority, date)
- Interaction history tracking
- Priority management
- Origin tracking (WhatsApp, Instagram, Facebook, etc.)

---

### 4. RAG Management (Documents & Embeddings) (13 endpoints)

Knowledge base management for Copilot AI.

#### Documents
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_copilot_documents` | List knowledge base documents |
| POST | `/rest/v1/salesos_copilot_documents` | Create document |
| PATCH | `/rest/v1/salesos_copilot_documents` | Update document |
| DELETE | `/rest/v1/salesos_copilot_documents` | Delete document |

#### Chunks
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_copilot_chunks` | List text chunks |
| POST | `/rest/v1/salesos_copilot_chunks` | Create chunk(s) |
| PATCH | `/rest/v1/salesos_copilot_chunks` | Update chunk |
| DELETE | `/rest/v1/salesos_copilot_chunks` | Delete chunk |

#### Embeddings
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/functions/v1/generate-embeddings` | Generate embeddings for chunks |
| GET | `/rest/v1/salesos_copilot_chunks?embedding=is.null` | List chunks without embeddings |

**Document Types:**
- script, faq, objection_handler, product_info, process, training

**Key Features:**
- Semantic search via embeddings
- Hierarchical document structure
- Batch embedding generation
- Metadata tagging for context filtering

---

### 5. Tenants & Roles (Multi-tenant RBAC) (16 endpoints)

Complete multi-tenant organization management with RBAC.

#### Tenants
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_tenants` | List organizations |
| POST | `/rest/v1/salesos_tenants` | Create tenant |
| PATCH | `/rest/v1/salesos_tenants` | Update tenant |

#### Roles
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_tenant_roles` | List roles |
| POST | `/rest/v1/salesos_tenant_roles` | Create custom role |

#### Capabilities
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_capabilities` | List all capabilities |
| GET | `/rest/v1/salesos_role_capabilities` | List role capabilities |
| POST | `/rest/v1/salesos_role_capabilities` | Add capability to role |
| DELETE | `/rest/v1/salesos_role_capabilities` | Remove capability from role |

#### User Associations
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_user_tenants` | List user-tenant associations |
| POST | `/rest/v1/salesos_user_tenants` | Associate user to tenant |
| PATCH | `/rest/v1/salesos_user_tenants` | Update user role |

**Capability Modules:**
- leads, copilot, workflows, tenants, users, reports, settings

**Key Features:**
- Multi-tenant data isolation
- Custom role creation
- Granular capability-based permissions
- User-tenant-role associations
- System vs custom roles

---

### 6. Users & Profiles (10 endpoints)

User management with authentication, identities, and gamification.

#### User Management
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_users` | List users |
| PATCH | `/rest/v1/salesos_users` | Update user profile |

#### Authentication & Identities
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/functions/v1/reconcile-user-context` | Reconcile user data |
| GET | `/rest/v1/salesos_user_identities` | List user identities |

#### Social Connections
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_user_social_connections` | List social connections |
| POST | `/functions/v1/social-auth-callback` | OAuth callback handler |

#### Gamification
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_user_scores` | List user scores |
| GET | `/rest/v1/salesos_checkins` | List check-ins |

**Identity Providers:**
- Auth0, Google, Facebook, LinkedIn

**Social Platforms:**
- Instagram, Facebook, LinkedIn, Twitter

**Key Features:**
- Multiple identity provider support
- Social media OAuth integration
- Gamification scoring and ranking
- Check-in/attendance tracking

---

### 7. Workflows & Automation (15 endpoints)

Event-driven workflow engine with comprehensive automation.

#### Workflows
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_workflows` | List workflows |
| POST | `/rest/v1/salesos_workflows` | Create workflow |
| PATCH | `/rest/v1/salesos_workflows` | Update workflow |

#### Triggers
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_workflow_triggers` | List triggers |
| POST | `/rest/v1/salesos_workflow_triggers` | Create trigger |

#### Execution Queue
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_workflow_queue` | List queue items |
| POST | `/functions/v1/workflow-worker` | Process queue |

#### Execution History
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_workflow_runs` | List workflow executions |

#### Events & Actions
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/rest/v1/salesos_events` | List events |
| GET | `/rest/v1/salesos_actions` | List actions |
| GET | `/rest/v1/salesos_event_schemas` | List event schemas |

**Trigger Types:**
- event, schedule, manual

**Queue Statuses:**
- pending, processing, completed, failed

**Key Features:**
- Event-driven automation
- Scheduled workflows
- Manual execution
- Queue management
- Step-by-step execution tracking
- Error handling and retry logic

---

### 8. Webhooks & EventService (23 endpoints)

Incoming webhooks and centralized event emission.

#### Incoming Webhooks
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/webhooks/meta-leads` | Meta Ads lead capture |
| POST | `/api/webhooks/zapier-leads` | Zapier automation |

#### Event Emission
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/rest/v1/rpc/salesos_emit_event` | Emit system event via RPC |

**Event Types:**

**Leads Domain:**
- lead.created, call.completed, email.sent, whatsapp.sent
- visit.scheduled, visit.completed, meeting.completed, proposal.sent

**Gamification Domain:**
- quiz.completed, mission.completed

**Event Domains:**
- leads, go (gamification), workflows

**Key Features:**
- HMAC signature validation (Meta)
- Secret token authentication (Zapier)
- Automatic workflow triggering
- Event persistence and querying
- Points attribution for gamification
- Integration with external systems

---

## Authentication & Security

### Authentication Flow

1. **Auth0 Login**
   ```
   POST /oauth/token
   Content-Type: application/json

   {
     "grant_type": "password",
     "client_id": "your_client_id",
     "client_secret": "your_client_secret",
     "audience": "https://api.play2sell.com",
     "username": "user@example.com",
     "password": "password",
     "scope": "openid profile email"
   }
   ```

2. **Token Exchange**
   ```
   POST /functions/v1/token_exchange_edge_function
   Authorization: Bearer <auth0_token>
   Content-Type: application/json

   {
     "auth0_token": "<auth0_access_token>"
   }
   ```

3. **Use Supabase Token**
   ```
   GET /rest/v1/salesos_opportunities
   Authorization: Bearer <supabase_token>
   apikey: <anon_key>
   ```

### Security Schemes

| Scheme | Type | Description |
|--------|------|-------------|
| bearerAuth | HTTP Bearer | Supabase JWT token |
| auth0Bearer | HTTP Bearer | Auth0 access token |
| metaSignature | API Key | HMAC SHA256 for Meta webhooks |
| zapierSecret | API Key | Secret token for Zapier webhooks |

---

## Query Parameters (PostgREST)

### Filtering

```
?tenant_id=eq.123e4567-e89b-12d3-a456-426614174000
?stage_id=eq.stage-123
?created_at=gte.2026-01-01
?is_active=eq.true
```

### Operators

- `eq` - Equals
- `neq` - Not equals
- `gt` - Greater than
- `gte` - Greater than or equal
- `lt` - Less than
- `lte` - Less than or equal
- `is.null` - Is null
- `not.is.null` - Is not null

### Selection

```
?select=*
?select=id,name,email
?select=*,salesos_users(name,email)
```

### Ordering

```
?order=created_at.desc
?order=name.asc
?order=priority.desc,created_at.desc
```

### Pagination

```
?limit=50
?limit=100&offset=50
```

---

## Rate Limiting

| Resource | Limit |
|----------|-------|
| API Calls | 1000 requests/minute per IP |
| Events | 10000 events/day per tenant |
| Webhooks | 100 requests/minute per endpoint |
| Copilot Suggestions | 100 requests/hour per user |

---

## Response Formats

### Success Response

```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "name": "João Silva",
  "email": "joao@example.com",
  "created_at": "2026-01-04T10:30:00Z"
}
```

### Error Response

```json
{
  "message": "Invalid request body",
  "code": "400",
  "details": "Missing required field 'tenant_id'"
}
```

### Standard HTTP Status Codes

| Code | Description |
|------|-------------|
| 200 | OK - Request successful |
| 201 | Created - Resource created |
| 204 | No Content - Deletion successful |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid credentials |
| 403 | Forbidden - Insufficient permissions |
| 404 | Not Found - Resource not found |
| 500 | Internal Server Error |

---

## Prefer Header

Used with POST/PATCH requests to control response:

```
Prefer: return=representation  (default - return created/updated object)
Prefer: return=minimal          (return only status code)
```

---

## Migration from v2.1.0

### Breaking Changes

None - v3.0.0 is fully backward compatible.

### New Endpoints to Integrate

1. **Copilot IA**: Start using AI suggestions
2. **RAG Management**: Build knowledge base
3. **RBAC**: Implement role-based access control
4. **Workflows**: Automate business processes

### Recommended Integration Order

1. Update authentication to support tenant switching
2. Integrate Copilot IA for sales assistance
3. Set up RAG knowledge base
4. Configure RBAC for multi-tenant access
5. Build workflow automations
6. Connect external webhooks

---

## Testing

### Using Postman

Import the collections from `/postman/`:
- `SalesOS_Auth_Flow_Collection.json`
- `SalesOS_Copilot_IA_Collection.json`
- `SalesOS_Leads_Collection.json`
- `SalesOS_RAG_Collection.json`
- `SalesOS_Tenants_Roles_Collection.json`
- `SalesOS_Users_Auth_Collection.json`
- `SalesOS_Workflows_Collection.json`
- `SalesOS-Webhooks-EventService.postman_collection.json`

### Environment Variables

```json
{
  "base_url": "https://api.play2sell.com",
  "anon_key": "your_anon_key",
  "auth0_domain": "your_tenant.us.auth0.com",
  "auth0_client_id": "your_client_id",
  "auth0_client_secret": "your_client_secret",
  "supabase_token": "",
  "tenant_id": "",
  "user_id": ""
}
```

---

## Use Cases

### 1. Lead Capture from Meta Ads

```
Meta Ads → /api/webhooks/meta-leads → Create Lead → Emit Event → Trigger Workflow → Assign to User
```

### 2. AI-Assisted Sales

```
User Opens Lead → /functions/v1/copilot-suggest → Get AI Suggestions → Apply Suggestion → Submit Feedback
```

### 3. Multi-Tenant SaaS

```
User Login → Token Exchange → List Tenants → Switch Tenant → Access Tenant Data
```

### 4. Workflow Automation

```
Event Emitted → Match Triggers → Add to Queue → Worker Processes → Execute Actions → Record Results
```

---

## Support & Documentation

- **Full API Spec**: `/openapi/salesos-api-v3.0.yaml`
- **Endpoint Inventory**: `/POSTMAN_ENDPOINTS_INVENTORY.md`
- **Postman Collections**: `/postman/` directory
- **Contact**: dev@play2sell.com
- **Docs**: https://docs.salesos.com

---

## Changelog

### v3.0.0 (January 4, 2026)

**Added:**
- 92+ new endpoints for complete API coverage
- Authentication & Auth Flow (6 endpoints)
- Copilot IA (14 endpoints)
- Leads & Opportunities (14 endpoints)
- RAG Management (13 endpoints)
- Tenants & Roles (16 endpoints)
- Users & Profiles (10 endpoints)
- Workflows & Automation (15 endpoints)
- Webhooks & EventService (23 endpoints)

**Improved:**
- Professional endpoint descriptions
- Comprehensive schema definitions
- Multiple examples per endpoint
- Security documentation
- Error handling standards

**Technical:**
- OpenAPI 3.1.0 specification
- PostgREST query parameter documentation
- Rate limiting information
- Multi-environment support

---

## Production Readiness Checklist

- [x] All 110+ endpoints documented
- [x] Request/response schemas defined
- [x] Security schemes configured
- [x] Error responses standardized
- [x] Rate limiting documented
- [x] Examples provided
- [x] Authentication flow documented
- [x] Multi-tenant support
- [x] RBAC implementation
- [x] Webhook validation
- [x] Event emission
- [x] Workflow automation

---

**Status**: Production Ready ✓
**Last Updated**: January 4, 2026
**Maintainer**: SalesOS Development Team
