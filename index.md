# SalesOS Platform API

**A plataforma completa de automação de vendas orientada a eventos**

---

## 🚀 O que é o SalesOS?

SalesOS é uma plataforma B2B SaaS que combina **automação de workflows**, **gamificação**, **IA (Copilot)** e **gestão multi-tenant** para equipes de vendas modernas.

### Principais Recursos

<table>
  <tr>
    <td><strong>🤖 Copilot IA</strong></td>
    <td>Assistente de vendas com RAG, TTS e STT para sugestões contextualizadas</td>
  </tr>
  <tr>
    <td><strong>⚡ Workflows</strong></td>
    <td>Automações event-driven para leads, notificações e integrações</td>
  </tr>
  <tr>
    <td><strong>🎮 Gamificação</strong></td>
    <td>Missões, quizzes e rankings para engajamento da equipe</td>
  </tr>
  <tr>
    <td><strong>🏢 Multi-tenancy</strong></td>
    <td>RBAC completo com planos, capabilities e segmentação</td>
  </tr>
  <tr>
    <td><strong>📊 EventService</strong></td>
    <td>Sistema centralizado de eventos que alimenta toda a plataforma</td>
  </tr>
  <tr>
    <td><strong>🔗 Webhooks</strong></td>
    <td>Integração bidirecional com Meta Ads, Zapier, CRMs e mais</td>
  </tr>
</table>

---

## 📖 Documentação

Esta documentação oferece:

- ✅ **140+ endpoints REST** (CRUD completo de todas as entidades)
- ✅ **14 Edge Functions** (operações serverless)
- ✅ **Guias práticos** para casos de uso comuns
- ✅ **Exemplos de código** funcionais em múltiplas linguagens
- ✅ **API Reference interativa** (teste direto no navegador)

---

## ⚡ Quickstart (5 minutos)

```bash
# 1. Autentique com Auth0
curl -X POST https://salesos.us.auth0.com/oauth/token \
  -H "Content-Type: application/json" \
  -d '{
    "grant_type": "password",
    "username": "seu@email.com",
    "password": "sua-senha",
    "client_id": "seu-client-id",
    "audience": "https://api.play2sell.com"
  }'

# 2. Liste seus tenants
curl https://api.play2sell.com/rest/v1/salesos_user_tenants \
  -H "Authorization: Bearer SEU_TOKEN"

# 3. Emita um evento
curl -X POST https://api.play2sell.com/functions/v1/emit-event \
  -H "Authorization: Bearer SEU_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "event_type": "lead.created",
    "user_id": "user-uuid",
    "tenant_id": "tenant-uuid",
    "payload": {
      "customer_name": "João Silva",
      "customer_email": "joao@example.com"
    }
  }'
```

👉 **[Ver guia completo de Quickstart →](docs/quickstart.md)**

---

## 🏗️ Arquitetura

```
┌─────────────────────────────────────────────────────────────┐
│                      SalesOS Platform                        │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   REST API   │  │ Edge Functions│  │  Webhooks    │      │
│  │  (126 endpoints)│  │  (14 funções) │  │  (Incoming)  │      │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘      │
│         │                  │                  │              │
│         └──────────────────┼──────────────────┘              │
│                            │                                 │
│                   ┌────────▼─────────┐                       │
│                   │   EventService   │                       │
│                   │  (Hub Central)   │                       │
│                   └────────┬─────────┘                       │
│                            │                                 │
│         ┌──────────────────┼──────────────────┐             │
│         │                  │                  │             │
│  ┌──────▼───────┐  ┌──────▼───────┐  ┌──────▼───────┐     │
│  │  Workflows   │  │ Gamificação  │  │  Copilot IA  │     │
│  │  (Automação) │  │  (Missões)   │  │  (RAG/TTS)   │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## 📚 Navegação

<table>
  <tr>
    <td width="50%">
      <h3>🚀 Começar</h3>
      <ul>
        <li><a href="docs/quickstart.md">Quickstart (5min)</a></li>
        <li><a href="docs/auth.md">Autenticação</a></li>
        <li><a href="docs/errors.md">Erros & Troubleshooting</a></li>
      </ul>
    </td>
    <td width="50%">
      <h3>📖 Guias</h3>
      <ul>
        <li><a href="docs/guides/webhooks.md">Webhooks & Eventos</a></li>
        <li><a href="docs/guides/workflows.md">Workflows & Automação</a></li>
        <li><a href="docs/guides/copilot.md">Copilot IA (RAG/TTS/STT)</a></li>
        <li><a href="docs/guides/gamification.md">Gamificação & Missões</a></li>
        <li><a href="docs/guides/multi-tenancy.md">Multi-tenancy & RBAC</a></li>
      </ul>
    </td>
  </tr>
</table>

---

## 🔐 Ambientes

| Ambiente | URL Base | Propósito |
|----------|----------|-----------|
| **Produção** | `https://api.play2sell.com` | Ambiente de produção |
| **Documentação** | `https://docs.play2sell.com` | Portal de documentação |

---

## 📊 Cobertura da API

- **126 endpoints REST** cobrindo 57 tabelas
- **14 Edge Functions** serverless
- **13 categorias** organizadas por domínio
- **100% de cobertura** do banco Supabase

### Categorias Principais

1. **Authentication & Auth Flow** - OAuth 2.0, tokens, tenant switching
2. **Copilot IA** - Sugestões, TTS, STT, RAG, feedback
3. **Leads & Opportunities** - Gestão completa do ciclo de vendas
4. **Workflows & Automation** - Event-driven automations
5. **Gamification & Missions** - Quizzes, missões, pontos
6. **Security & Access Control** - API keys, rate limits, cheat detection
7. **Tenants & Plans** - Módulos, planos, capabilities, entitlements
8. **Users & Context** - Contextos, alocações, comissões
9. **RAG Management** - Documentos, chunks, embeddings
10. **Webhooks & EventService** - Eventos entrantes e saintes

---

## 💡 Casos de Uso Comuns

### 1. Criar um Lead e Disparar Workflow
```javascript
// POST /functions/v1/emit-event
{
  "event_type": "lead.created",
  "payload": {
    "customer_name": "Maria Santos",
    "customer_phone": "11999887766",
    "source": "meta_ads"
  }
}
// → Dispara workflow de atribuição automática
// → Cria missão "Novo Lead" para vendedor
// → Atribui pontos de gamificação
```

### 2. Obter Sugestão do Copilot IA
```javascript
// POST /functions/v1/copilot-suggest
{
  "session_id": "session-uuid",
  "user_message": "Como abordar cliente interessado em seguro auto?",
  "context": {
    "opportunity_id": "opp-uuid",
    "customer_profile": "high_intent"
  }
}
// → Retorna sugestão personalizada via RAG
// → Baseada em documentos do tenant
```

### 3. Configurar Webhook de Meta Ads
```javascript
// POST /webhooks/meta-ads
{
  "entry": [{
    "changes": [{
      "value": {
        "leadgen_id": "12345",
        "field_data": [
          {"name": "full_name", "values": ["João Pedro"]},
          {"name": "phone_number", "values": ["11988776655"]}
        ]
      }
    }]
  }]
}
// → Cria lead automaticamente
// → Dispara workflow de atribuição
```

---

## 🛠️ SDKs e Ferramentas

<table>
  <tr>
    <td><strong>JavaScript/TypeScript</strong></td>
    <td><code>@supabase/supabase-js</code></td>
    <td><a href="https://supabase.com/docs/reference/javascript/introduction">Docs →</a></td>
  </tr>
  <tr>
    <td><strong>Python</strong></td>
    <td><code>supabase-py</code></td>
    <td><a href="https://supabase.com/docs/reference/python/introduction">Docs →</a></td>
  </tr>
  <tr>
    <td><strong>cURL</strong></td>
    <td>Exemplos em todos os endpoints</td>
    <td><a href="#reference/salesos-api/authentication">Ver →</a></td>
  </tr>
  <tr>
    <td><strong>Postman Collection</strong></td>
    <td>Importar collection completa</td>
    <td><a href="/postman/SalesOS-API.postman_collection.json">Download →</a></td>
  </tr>
</table>

---

## 🔄 Rate Limits

| Categoria | Limite | Janela |
|-----------|--------|--------|
| REST API | 1000 req/min | Por IP |
| Edge Functions | 100 req/min | Por API key |
| Webhooks | 100 req/min | Por endpoint |
| EventService | 10000 eventos/dia | Por tenant |
| Copilot IA | 100 req/hora | Por usuário |

---

## 📞 Suporte

<table>
  <tr>
    <td>📧 <strong>Email</strong></td>
    <td><a href="mailto:dev@play2sell.com">dev@play2sell.com</a></td>
  </tr>
  <tr>
    <td>🐛 <strong>Issues</strong></td>
    <td><a href="https://github.com/felipeplay2sellcom/SalesOs-API/issues">GitHub Issues</a></td>
  </tr>
  <tr>
    <td>💬 <strong>Slack</strong></td>
    <td>Canal interno #salesos-api</td>
  </tr>
  <tr>
    <td>📖 <strong>Status</strong></td>
    <td><a href="https://status.play2sell.com">status.play2sell.com</a></td>
  </tr>
</table>

---

## 📝 Changelog

Acompanhe todas as mudanças da API:

### v3.0.0 (Janeiro 2026) - Current
- ✅ **30 novos endpoints** (Security, Gamification, Tenants, Workflows)
- ✅ **14 Edge Functions** documentadas
- ✅ **140+ endpoints** (100% de cobertura)
- ✅ Migração para Redocly
- ✅ Portal de documentação completo

[Ver changelog completo →](docs/changelog.md)

---

<div align="center">
  <p><strong>Feito com ❤️ pela equipe Play2sell</strong></p>
  <p>
    <a href="https://play2sell.com">Website</a> •
    <a href="https://docs.play2sell.com">Documentação</a> •
    <a href="https://github.com/felipeplay2sellcom">GitHub</a>
  </p>
</div>
