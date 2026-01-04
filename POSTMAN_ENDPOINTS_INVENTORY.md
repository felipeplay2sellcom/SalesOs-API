# SalesOS API - Postman Collections Inventory

**Generated:** January 4, 2026

---

## Summary Overview

| Collection Name | Endpoint Count | HTTP Methods | Primary Domain |
|---|---|---|---|
| SalesOS Auth Flow | 5 | POST, GET | Authentication |
| SalesOS Copilot IA | 14 | POST, GET | AI/Copilot |
| SalesOS Leads & Opportunities | 14 | GET, POST, PATCH, DELETE | Leads Management |
| SalesOS RAG Management | 13 | GET, POST, PATCH, DELETE | RAG/Copilot Content |
| SalesOS Tenants & Roles | 16 | GET, POST, PATCH, DELETE | Tenants/RBAC |
| SalesOS Users & Auth | 10 | GET, POST, PATCH | Users/Identities |
| SalesOS Workflows & Automation | 15 | GET, POST, PATCH | Workflows |
| SalesOS - Webhooks & EventService | 23 | POST, GET | Webhooks/Events |
| **TOTAL** | **110** | **7** | **Multi-domain** |

---

## Collection 1: SalesOS Auth Flow

**Description:** Fluxo de autenticação: Auth0 → Supabase Token. Execute na ordem: 1) Login Auth0, 2) Token Exchange, 3) Use o token nas outras collections.

**Base URL:** `https://api.play2sell.com`

**Endpoints:** 5

| # | Method | Endpoint | Description |
|---|---|---|---|
| 1 | POST | `/oauth/token` | Auth0 - Login (Password Grant) - Login com email/senha. O token Auth0 será salvo automaticamente na variável 'auth0_token'. |
| 2 | POST | `/functions/v1/token_exchange_edge_function` | Token Exchange (Auth0 → Supabase) - Troca o token Auth0 pelo token Supabase. Salva automaticamente: supabase_token, user_id, tenant_id. |
| 3 | GET | `/rest/v1/salesos_user_tenants` | Testar Token - Listar Meus Tenants - Testa se o token está funcionando listando seus tenants. |
| 4 | GET | `/rest/v1/salesos_users` | Testar Token - Meu Perfil - Busca seus dados de perfil. |
| 5 | GET | `/rest/v1/salesos_opportunities` | Testar Token - Meus Leads - Lista seus leads mais recentes. |
| 6 | POST | `/functions/v1/switch-tenant` | Trocar de Tenant - Troca para outro tenant. Atualiza o supabase_token automaticamente. |

---

## Collection 2: SalesOS Copilot IA

**Description:** APIs de IA: Sugestões, Text-to-Speech, Speech-to-Text, Feedback

**Base URL:** `https://api.play2sell.com`

**Endpoints:** 14

### Sugestões (RAG + LLM)
| # | Method | Endpoint | Description |
|---|---|---|---|
| 1 | POST | `/functions/v1/copilot-suggest` | Gerar Sugestões - Qualificação |
| 2 | POST | `/functions/v1/copilot-suggest` | Gerar Sugestões - Objeção de Preço |
| 3 | POST | `/functions/v1/copilot-suggest` | Gerar Sugestões - Fechamento |

### Text-to-Speech
| # | Method | Endpoint | Description |
|---|---|---|---|
| 4 | POST | `/functions/v1/copilot-tts` | Gerar Áudio (TTS) - Retorna audio/mpeg. Salve a resposta como arquivo .mp3 |

### Speech-to-Text
| # | Method | Endpoint | Description |
|---|---|---|---|
| 5 | POST | `/functions/v1/copilot-stt` | Transcrever Áudio (STT) - Envie o áudio em base64 (formato webm/opus). Mínimo 2 segundos de áudio. |

### Feedback
| # | Method | Endpoint | Description |
|---|---|---|---|
| 6 | POST | `/functions/v1/copilot-feedback` | Enviar Feedback Positivo |
| 7 | POST | `/functions/v1/copilot-feedback` | Enviar Feedback Negativo |

### Analytics & Histórico
| # | Method | Endpoint | Description |
|---|---|---|---|
| 8 | GET | `/rest/v1/salesos_copilot_sessions` | Sessões do Copilot |
| 9 | GET | `/rest/v1/salesos_copilot_suggestions` | Sugestões Geradas |
| 10 | GET | `/rest/v1/salesos_copilot_feedback` | Feedbacks Recebidos |
| 11 | GET | `/rest/v1/salesos_copilot_cache` | Cache de Sugestões |
| 12 | GET | `/rest/v1/salesos_copilot_static_suggestions` | Sugestões Estáticas (Fallback) |

---

## Collection 3: SalesOS Leads & Opportunities

**Description:** Gestão de Leads, Oportunidades e Interações

**Base URL:** `https://api.play2sell.com`

**Endpoints:** 14

### Opportunities (Leads)
| # | Method | Endpoint | Description |
|---|---|---|---|
| 1 | GET | `/rest/v1/salesos_opportunities` | Listar Leads do Tenant |
| 2 | GET | `/rest/v1/salesos_opportunities` | Buscar Lead por ID |
| 3 | POST | `/rest/v1/salesos_opportunities` | Criar Lead |
| 4 | PATCH | `/rest/v1/salesos_opportunities` | Atualizar Lead |
| 5 | PATCH | `/rest/v1/salesos_opportunities` | Atualizar Stage do Lead |
| 6 | DELETE | `/rest/v1/salesos_opportunities` | Deletar Lead |
| 7 | GET | `/rest/v1/salesos_opportunities` | Filtrar por Stage |

### Interações
| # | Method | Endpoint | Description |
|---|---|---|---|
| 8 | GET | `/rest/v1/salesos_lead_interactions` | Listar Interações do Lead |
| 9 | POST | `/rest/v1/salesos_lead_interactions` | Criar Interação |
| 10 | GET | `/rest/v1/` | Tipos de Interação Disponíveis |

### Stages (Etapas)
| # | Method | Endpoint | Description |
|---|---|---|---|
| 11 | GET | `/rest/v1/salesos_context_stages` | Listar Stages do Contexto |
| 12 | GET | `/rest/v1/salesos_context_stages` | Stages por Tipo de Contexto |

### Analytics
| # | Method | Endpoint | Description |
|---|---|---|---|
| 13 | GET | `/rest/v1/salesos_opportunities` | Contagem por Stage |
| 14 | GET | `/rest/v1/salesos_opportunities` | Leads Criados Hoje |

---

## Collection 4: SalesOS RAG Management

**Description:** Collection para gerenciar conteúdo do RAG (Copilot IA) do SalesOS

**Base URL:** `https://api.play2sell.com`

**Endpoints:** 13

### Documentos
| # | Method | Endpoint | Description |
|---|---|---|---|
| 1 | GET | `/rest/v1/salesos_copilot_documents` | Listar Documentos |
| 2 | POST | `/rest/v1/salesos_copilot_documents` | Criar Documento |
| 3 | PATCH | `/rest/v1/salesos_copilot_documents` | Atualizar Documento |
| 4 | DELETE | `/rest/v1/salesos_copilot_documents` | Deletar Documento |

### Chunks
| # | Method | Endpoint | Description |
|---|---|---|---|
| 5 | GET | `/rest/v1/salesos_copilot_chunks` | Listar Chunks |
| 6 | GET | `/rest/v1/salesos_copilot_chunks` | Listar Chunks por Documento |
| 7 | POST | `/rest/v1/salesos_copilot_chunks` | Criar Chunk |
| 8 | POST | `/rest/v1/salesos_copilot_chunks` | Criar Multiplos Chunks |
| 9 | PATCH | `/rest/v1/salesos_copilot_chunks` | Atualizar Chunk (regenera embedding) |
| 10 | DELETE | `/rest/v1/salesos_copilot_chunks` | Deletar Chunk |

### Embeddings
| # | Method | Endpoint | Description |
|---|---|---|---|
| 11 | POST | `/functions/v1/generate-embeddings` | Gerar Embeddings Pendentes |
| 12 | GET | `/rest/v1/salesos_copilot_chunks` | Verificar Chunks sem Embedding |

### Testar Copilot
| # | Method | Endpoint | Description |
|---|---|---|---|
| 13 | POST | `/functions/v1/copilot-suggest` | Testar Sugestao (copilot-suggest) |

### Analytics
| # | Method | Endpoint | Description |
|---|---|---|---|
| 14 | GET | `/rest/v1/salesos_copilot_documents` | Contagem de Chunks por Documento |
| 15 | GET | `/rest/v1/salesos_copilot_sessions` | Sessoes do Copilot (ultimas) |
| 16 | GET | `/rest/v1/salesos_copilot_suggestions` | Sugestoes Geradas (ultimas) |

---

## Collection 5: SalesOS Tenants & Roles

**Description:** Gestão de Tenants, Roles, Capabilities e Permissões

**Base URL:** `https://api.play2sell.com`

**Endpoints:** 16

### Tenants
| # | Method | Endpoint | Description |
|---|---|---|---|
| 1 | GET | `/rest/v1/salesos_tenants` | Listar Tenants |
| 2 | GET | `/rest/v1/salesos_tenants` | Buscar Tenant por ID |
| 3 | POST | `/rest/v1/salesos_tenants` | Criar Tenant |
| 4 | PATCH | `/rest/v1/salesos_tenants` | Atualizar Tenant |

### Roles
| # | Method | Endpoint | Description |
|---|---|---|---|
| 5 | GET | `/rest/v1/salesos_tenant_roles` | Listar Roles do Tenant |
| 6 | GET | `/rest/v1/salesos_tenant_roles` | Listar Todos os Roles |
| 7 | POST | `/rest/v1/salesos_tenant_roles` | Criar Role Custom |

### Capabilities
| # | Method | Endpoint | Description |
|---|---|---|---|
| 8 | GET | `/rest/v1/salesos_capabilities` | Listar Todas Capabilities |
| 9 | GET | `/rest/v1/salesos_capabilities` | Capabilities por Módulo |
| 10 | GET | `/rest/v1/salesos_role_capabilities` | Capabilities do Role |
| 11 | POST | `/rest/v1/salesos_role_capabilities` | Adicionar Capability ao Role |
| 12 | DELETE | `/rest/v1/salesos_role_capabilities` | Remover Capability do Role |

### User Tenants (Associações)
| # | Method | Endpoint | Description |
|---|---|---|---|
| 13 | GET | `/rest/v1/salesos_user_tenants` | Listar Usuários do Tenant |
| 14 | GET | `/rest/v1/salesos_user_tenants` | Tenants do Usuário |
| 15 | POST | `/rest/v1/salesos_user_tenants` | Associar Usuário ao Tenant |
| 16 | PATCH | `/rest/v1/salesos_user_tenants` | Alterar Role do Usuário |

---

## Collection 6: SalesOS Users & Auth

**Description:** Gestão de Usuários, Autenticação e Identidades

**Base URL:** `https://api.play2sell.com`

**Endpoints:** 10

### Autenticação
| # | Method | Endpoint | Description |
|---|---|---|---|
| 1 | POST | `/functions/v1/token_exchange_edge_function` | Token Exchange (Auth0 -> Supabase) |
| 2 | POST | `/functions/v1/reconcile-user-context` | Reconciliar Usuário |
| 3 | POST | `/functions/v1/switch-tenant` | Trocar de Tenant |

### Usuários
| # | Method | Endpoint | Description |
|---|---|---|---|
| 4 | GET | `/rest/v1/salesos_users` | Listar Usuários |
| 5 | GET | `/rest/v1/salesos_users` | Buscar Usuário por Email |
| 6 | PATCH | `/rest/v1/salesos_users` | Atualizar Usuário |

### Identidades
| # | Method | Endpoint | Description |
|---|---|---|---|
| 7 | GET | `/rest/v1/salesos_user_identities` | Listar Identidades do Usuário |
| 8 | GET | `/rest/v1/salesos_user_identities` | Buscar por Provider ID |

### Social Connections
| # | Method | Endpoint | Description |
|---|---|---|---|
| 9 | GET | `/rest/v1/salesos_user_social_connections` | Listar Conexões Sociais |
| 10 | POST | `/functions/v1/social-auth-callback` | Social Auth Callback |

### Scores & Gamification
| # | Method | Endpoint | Description |
|---|---|---|---|
| 11 | GET | `/rest/v1/salesos_user_scores` | Listar Scores do Usuário |
| 12 | GET | `/rest/v1/salesos_checkins` | Check-ins do Usuário |

---

## Collection 7: SalesOS Workflows & Automation

**Description:** Gestão de Workflows, Filas, Triggers e Execuções

**Base URL:** `https://api.play2sell.com`

**Endpoints:** 15

### Workflows
| # | Method | Endpoint | Description |
|---|---|---|---|
| 1 | GET | `/rest/v1/salesos_workflows` | Listar Workflows |
| 2 | GET | `/rest/v1/salesos_workflows` | Buscar Workflow por ID |
| 3 | POST | `/rest/v1/salesos_workflows` | Criar Workflow |
| 4 | PATCH | `/rest/v1/salesos_workflows` | Ativar/Desativar Workflow |

### Triggers
| # | Method | Endpoint | Description |
|---|---|---|---|
| 5 | GET | `/rest/v1/salesos_workflow_triggers` | Listar Triggers do Workflow |
| 6 | POST | `/rest/v1/salesos_workflow_triggers` | Criar Trigger |

### Fila de Execução
| # | Method | Endpoint | Description |
|---|---|---|---|
| 7 | GET | `/rest/v1/salesos_workflow_queue` | Ver Fila Pendente |
| 8 | GET | `/rest/v1/salesos_workflow_queue` | Ver Fila Processando |
| 9 | GET | `/rest/v1/salesos_workflow_queue` | Ver Itens com Erro |
| 10 | POST | `/functions/v1/workflow-worker` | Executar Worker (processar fila) |

### Execuções (Runs)
| # | Method | Endpoint | Description |
|---|---|---|---|
| 11 | GET | `/rest/v1/salesos_workflow_runs` | Listar Execuções |
| 12 | GET | `/rest/v1/salesos_workflow_runs` | Detalhes da Execução (com steps) |
| 13 | GET | `/rest/v1/salesos_workflow_runs` | Execuções com Erro |

### Events & Actions
| # | Method | Endpoint | Description |
|---|---|---|---|
| 14 | GET | `/rest/v1/salesos_events` | Listar Eventos |
| 15 | GET | `/rest/v1/salesos_actions` | Listar Actions |
| 16 | GET | `/rest/v1/salesos_event_schemas` | Schemas de Eventos |

---

## Collection 8: SalesOS - Webhooks & EventService

**Description:** Collection completa para testar webhooks externos e EventService do SalesOS (v2 - Jan 2026)

**Base URL:** `http://localhost:5173` (local dev) / `https://api.play2sell.com` (production)

**Endpoints:** 23

### 1. Webhooks Recebidos (Incoming)
| # | Method | Endpoint | Description |
|---|---|---|---|
| 1 | POST | `/api/webhooks/meta-leads` | Meta Ads - Novo Lead - Webhook do Meta Ads quando um novo lead é capturado |
| 2 | POST | `/api/webhooks/zapier-leads` | Zapier - Novo Lead |

### 2. EventService - Eventos de Leads
| # | Method | Endpoint | Description |
|---|---|---|---|
| 3 | POST | `/rest/v1/rpc/salesos_emit_event` | Lead Criado - Testa emissão de evento lead.created via RPC |
| 4 | POST | `/rest/v1/rpc/salesos_emit_event` | Ligação Completada |
| 5 | POST | `/rest/v1/rpc/salesos_emit_event` | Email Enviado |
| 6 | POST | `/rest/v1/rpc/salesos_emit_event` | WhatsApp Enviado |
| 7 | POST | `/rest/v1/rpc/salesos_emit_event` | Visita Agendada |
| 8 | POST | `/rest/v1/rpc/salesos_emit_event` | Visita Completada |
| 9 | POST | `/rest/v1/rpc/salesos_emit_event` | Reunião Completada |
| 10 | POST | `/rest/v1/rpc/salesos_emit_event` | Proposta Enviada |

### 3. EventService - Eventos GO (Gamificação)
| # | Method | Endpoint | Description |
|---|---|---|---|
| 11 | POST | `/rest/v1/rpc/salesos_emit_event` | Quiz Completado |
| 12 | POST | `/rest/v1/rpc/salesos_emit_event` | Missão Completada |

### 4. Queries - Verificação
| # | Method | Endpoint | Description |
|---|---|---|---|
| 13 | GET | `/rest/v1/salesos_events` | Listar Eventos Recentes |
| 14 | GET | `/rest/v1/salesos_workflow_runs` | Workflows Disparados |
| 15 | GET | `/rest/v1/salesos_events` | Eventos por Tipo |
| 16 | GET | `/rest/v1/salesos_events` | Pontos do Usuário |

### 5. Webhooks Enviados (Outgoing)
| # | Method | Endpoint | Description |
|---|---|---|---|
| 17 | POST | `https://webhook.site/{{webhook_site_id}}` | Webhook.site - Teste de Notificação - Endpoint de teste (webhook.site) para simular sistema externo recebendo webhook do SalesOS |

---

## HTTP Methods Distribution

| Method | Count | Collections Using |
|---|---|---|
| GET | 48 | All collections |
| POST | 44 | All collections |
| PATCH | 11 | Leads, RAG, Tenants, Users, Workflows |
| DELETE | 7 | Leads, RAG, Tenants, Webhooks |

---

## API Domains/Tables Referenced

### Data Tables (REST API)
- `salesos_opportunities` - Leads/Opportunities management
- `salesos_users` - User profiles
- `salesos_tenants` - Tenant management
- `salesos_tenant_roles` - Role definitions
- `salesos_role_capabilities` - Role-Capability associations
- `salesos_capabilities` - System capabilities
- `salesos_user_tenants` - User-Tenant associations
- `salesos_user_identities` - User identity providers
- `salesos_user_social_connections` - Social media connections
- `salesos_user_scores` - User gamification scores
- `salesos_checkins` - User check-ins
- `salesos_lead_interactions` - Lead interaction history
- `salesos_context_stages` - Sales pipeline stages
- `salesos_workflows` - Workflow definitions
- `salesos_workflow_versions` - Workflow versions
- `salesos_workflow_triggers` - Workflow triggers
- `salesos_workflow_queue` - Workflow execution queue
- `salesos_workflow_runs` - Workflow execution history
- `salesos_workflow_run_steps` - Workflow step results
- `salesos_events` - Event log
- `salesos_actions` - Action log
- `salesos_event_schemas` - Event schema definitions
- `salesos_copilot_sessions` - Copilot AI sessions
- `salesos_copilot_suggestions` - AI-generated suggestions
- `salesos_copilot_feedback` - User feedback on suggestions
- `salesos_copilot_cache` - Suggestion cache
- `salesos_copilot_static_suggestions` - Fallback suggestions
- `salesos_copilot_documents` - RAG documents
- `salesos_copilot_chunks` - RAG text chunks
- `salesos_copilot_feedback` - Copilot feedback

### Edge Functions (Custom Logic)
- `/functions/v1/token_exchange_edge_function` - Auth0 to Supabase token exchange
- `/functions/v1/switch-tenant` - Tenant switching
- `/functions/v1/copilot-suggest` - AI suggestion generation
- `/functions/v1/copilot-tts` - Text-to-Speech conversion
- `/functions/v1/copilot-stt` - Speech-to-Text conversion
- `/functions/v1/copilot-feedback` - Feedback submission
- `/functions/v1/generate-embeddings` - RAG embedding generation
- `/functions/v1/reconcile-user-context` - User context reconciliation
- `/functions/v1/social-auth-callback` - Social media OAuth callback
- `/functions/v1/workflow-worker` - Workflow queue processor

### Webhook Endpoints
- `/api/webhooks/meta-leads` - Meta Ads lead capture
- `/api/webhooks/zapier-leads` - Zapier automation

### RPC Functions
- `/rest/v1/rpc/salesos_emit_event` - Event emission

---

## Authentication Methods

| Method | Description | Collections Using |
|---|---|---|
| **Bearer Token (Supabase)** | JWT token from Auth0 exchange | All collections |
| **API Key** | Supabase anonymous key | Most collections |
| **Auth0 Token** | Initial authentication token | Auth Flow, Users Auth |
| **Webhook Signature** | HMAC signature validation | Webhooks |

---

## Key Features by Collection

### Authentication & Access Control
- **SalesOS Auth Flow** - Auth0 password grant, token exchange, tenant switching
- **SalesOS Users & Auth** - User profiles, identities, social connections, gamification

### Lead Management
- **SalesOS Leads & Opportunities** - CRUD operations, stage management, interaction tracking, analytics

### AI/Copilot Features
- **SalesOS Copilot IA** - Suggestion generation, text-to-speech, speech-to-text, feedback tracking
- **SalesOS RAG Management** - Document and chunk management, embedding generation, RAG testing

### Organization Management
- **SalesOS Tenants & Roles** - Multi-tenant support, RBAC, capability management, user associations

### Automation
- **SalesOS Workflows & Automation** - Workflow creation, trigger definition, queue management, execution history
- **SalesOS - Webhooks & EventService** - Incoming webhooks, event emission, event querying, outgoing webhooks

---

## Query Parameters Usage

### Common Filter Parameters
- `select` - Select specific columns
- `order` - Sort results (ascending/descending)
- `limit` - Limit result count
- `offset` - Pagination offset

### Filter Operators
- `eq` - Equals
- `is.null` - Is null
- `not.is.null` - Is not null
- `gte` - Greater than or equal

### Typical Query Examples
```
?select=*&tenant_id=eq.{{tenant_id}}&order=created_at.desc&limit=50
?select=id,name,email&order=created_at.desc
?select=id,title,salesos_copilot_chunks(count)
```

---

## Notes

1. **Base URL:** Most endpoints use `https://api.play2sell.com` (production)
2. **Authentication:** All requests require either:
   - Bearer token (Supabase JWT)
   - API key (anonymous Supabase key)
3. **Headers:** Standard headers include:
   - `Content-Type: application/json`
   - `apikey: {{anon_key}}`
   - `Authorization: Bearer {{supabase_token}}`
4. **Pagination:** Results are paginated using `limit` parameter (default 50 items)
5. **Relationships:** Supabase PostgREST API supports nested selects for related data
6. **RPC Functions:** Custom business logic via PostgreSQL RPC functions

---

## File References

All collections are located in: `/Users/play2sell/SalesOS-API/postman/`

- `SalesOS_Auth_Flow_Collection.json`
- `SalesOS_Copilot_IA_Collection.json`
- `SalesOS_Leads_Collection.json`
- `SalesOS_RAG_Collection.json`
- `SalesOS_Tenants_Roles_Collection.json`
- `SalesOS_Users_Auth_Collection.json`
- `SalesOS_Workflows_Collection.json`
- `SalesOS-Webhooks-EventService.postman_collection.json`
