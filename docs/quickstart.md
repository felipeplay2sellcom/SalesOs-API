# Quickstart - SalesOS API em 5 Minutos

Este guia vai te fazer rodar sua primeira chamada à API SalesOS em **menos de 5 minutos**.

---

## 📋 Pré-requisitos

- ✅ Conta Auth0 configurada
- ✅ Client ID e Client Secret
- ✅ Usuário com acesso a pelo menos 1 tenant
- ✅ cURL ou Postman instalado

---

## 🚀 Passo 1: Obter Token de Autenticação

### Via Auth0 (Password Grant)

```bash
curl -X POST https://salesos.us.auth0.com/oauth/token \
  -H "Content-Type: application/json" \
  -d '{
    "grant_type": "password",
    "username": "seu@email.com",
    "password": "sua-senha-segura",
    "client_id": "SEU_CLIENT_ID",
    "client_secret": "SEU_CLIENT_SECRET",
    "audience": "https://api.play2sell.com",
    "scope": "openid profile email"
  }'
```

**Resposta esperada:**

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "Bearer",
  "expires_in": 86400
}
```

💡 **Dica**: Salve o `access_token` em uma variável de ambiente:

```bash
export SALESOS_TOKEN="eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9..."
```

---

## 🏢 Passo 2: Listar Seus Tenants

Agora que você tem um token, vamos ver quais tenants você tem acesso:

```bash
curl https://api.play2sell.com/rest/v1/salesos_user_tenants \
  -H "Authorization: Bearer $SALESOS_TOKEN" \
  -H "apikey: SEU_ANON_KEY"
```

**Resposta esperada:**

```json
[
  {
    "id": "uuid-tenant-1",
    "tenant_id": "tenant-uuid-abc",
    "user_id": "user-uuid-123",
    "role": "admin",
    "is_active": true,
    "tenant": {
      "id": "tenant-uuid-abc",
      "name": "Acme Corp",
      "slug": "acme-corp"
    }
  }
]
```

💡 **Salve o tenant_id** para usar nos próximos passos:

```bash
export TENANT_ID="tenant-uuid-abc"
export USER_ID="user-uuid-123"
```

---

## 📊 Passo 3: Emitir Seu Primeiro Evento

Vamos emitir um evento de "lead criado" que irá:
- ✅ Criar um registro no EventService
- ✅ Disparar workflows configurados
- ✅ Atribuir pontos de gamificação (se configurado)

```bash
curl -X POST https://api.play2sell.com/functions/v1/emit-event \
  -H "Authorization: Bearer $SALESOS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "event_type": "lead.created",
    "user_id": "'"$USER_ID"'",
    "tenant_id": "'"$TENANT_ID"'",
    "payload": {
      "customer_name": "João Silva",
      "customer_email": "joao.silva@example.com",
      "customer_phone": "11999887766",
      "source": "meta_ads",
      "campaign_id": "camp_123"
    },
    "source": "api_quickstart",
    "points": 10
  }'
```

**Resposta esperada:**

```json
"f47ac10b-58cc-4372-a567-0e02b2c3d479"
```

✅ **Sucesso!** O UUID retornado é o `event_id`. Esse evento foi:
- Persistido no banco (`salesos_events`)
- Processado por workflows ativos
- Pode ter gerado ações (notifications, webhooks, etc)

---

## 🔍 Passo 4: Verificar o Evento Criado

Vamos confirmar que o evento foi criado:

```bash
curl "https://api.play2sell.com/rest/v1/salesos_events?select=*&order=created_at.desc&limit=1" \
  -H "Authorization: Bearer $SALESOS_TOKEN" \
  -H "apikey: SEU_ANON_KEY"
```

**Resposta esperada:**

```json
[
  {
    "id": "f47ac10b-58cc-4372-a567-0e02b2c3d479",
    "event_type": "lead.created",
    "tenant_id": "tenant-uuid-abc",
    "user_id": "user-uuid-123",
    "payload": {
      "customer_name": "João Silva",
      "customer_email": "joao.silva@example.com",
      "customer_phone": "11999887766",
      "source": "meta_ads",
      "campaign_id": "camp_123"
    },
    "source": "api_quickstart",
    "points": 10,
    "created_at": "2026-01-04T08:30:00Z"
  }
]
```

---

## 📈 Passo 5: Criar uma Oportunidade

Agora vamos criar uma oportunidade (lead) no sistema:

```bash
curl -X POST https://api.play2sell.com/rest/v1/salesos_opportunities \
  -H "Authorization: Bearer $SALESOS_TOKEN" \
  -H "Content-Type: application/json" \
  -H "Prefer: return=representation" \
  -d '{
    "user_id": "'"$USER_ID"'",
    "tenant_id": "'"$TENANT_ID"'",
    "customer_name": "Maria Santos",
    "customer_email": "maria@example.com",
    "customer_phone": "11988776655",
    "value": 5000.00,
    "source": "api",
    "priority": "high",
    "status": "new"
  }'
```

**Resposta esperada:**

```json
{
  "id": "opp-uuid-456",
  "user_id": "user-uuid-123",
  "tenant_id": "tenant-uuid-abc",
  "customer_name": "Maria Santos",
  "customer_email": "maria@example.com",
  "customer_phone": "11988776655",
  "value": 5000.00,
  "source": "api",
  "priority": "high",
  "status": "new",
  "created_at": "2026-01-04T08:35:00Z"
}
```

---

## 🎯 Passo 6 (Bônus): Obter Sugestão do Copilot IA

Se você tem o módulo Copilot ativo, pode obter sugestões de IA:

```bash
curl -X POST https://api.play2sell.com/functions/v1/copilot-suggest \
  -H "Authorization: Bearer $SALESOS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "session_id": "session-quickstart-123",
    "user_message": "Como abordar um cliente que pediu orçamento de seguro auto?",
    "tenant_id": "'"$TENANT_ID"'",
    "user_id": "'"$USER_ID"'",
    "context": {
      "opportunity_id": "opp-uuid-456",
      "customer_profile": "high_intent"
    }
  }'
```

**Resposta esperada:**

```json
{
  "suggestion": "Para um cliente que solicitou orçamento de seguro auto, recomendo:\n\n1. **Confirmar dados do veículo** (marca, modelo, ano)\n2. **Perguntar sobre histórico** (sinistros anteriores)\n3. **Oferecer 3 opções** (básica, intermediária, premium)\n4. **Destacar benefícios** específicos do seguro auto...",
  "confidence": 0.92,
  "sources": [
    "Manual de Vendas - Seguro Auto",
    "FAQ - Coberturas Veiculares"
  ],
  "session_id": "session-quickstart-123"
}
```

---

## ✅ Próximos Passos

Parabéns! 🎉 Você completou o quickstart. Agora você sabe como:

- ✅ Autenticar com Auth0
- ✅ Listar tenants do usuário
- ✅ Emitir eventos via EventService
- ✅ Criar oportunidades (leads)
- ✅ (Bônus) Usar o Copilot IA

### O que aprender agora?

<table>
  <tr>
    <td><strong>🔐 Autenticação Avançada</strong></td>
    <td>Troca de tenants, refresh tokens, SSO</td>
    <td><a href="auth.md">Ver guia →</a></td>
  </tr>
  <tr>
    <td><strong>🔗 Webhooks</strong></td>
    <td>Receber eventos de Meta Ads, Zapier, CRMs</td>
    <td><a href="guides/webhooks.md">Ver guia →</a></td>
  </tr>
  <tr>
    <td><strong>⚡ Workflows</strong></td>
    <td>Criar automações event-driven</td>
    <td><a href="guides/workflows.md">Ver guia →</a></td>
  </tr>
  <tr>
    <td><strong>🤖 Copilot IA</strong></td>
    <td>RAG, TTS, STT, sugestões personalizadas</td>
    <td><a href="guides/copilot.md">Ver guia →</a></td>
  </tr>
  <tr>
    <td><strong>🎮 Gamificação</strong></td>
    <td>Missões, quizzes, pontos</td>
    <td><a href="guides/gamification.md">Ver guia →</a></td>
  </tr>
</table>

---

## 📖 Referência Completa da API

Explore todos os **140+ endpoints** na referência interativa:

👉 **[Ver API Reference →](#reference/salesos-api)**

---

## 🐛 Problemas Comuns

### Erro 401: Unauthorized

**Causa**: Token expirado ou inválido

**Solução**: Gere um novo token no Passo 1

### Erro 403: Forbidden

**Causa**: Usuário não tem permissão no tenant

**Solução**: Verifique se o `user_id` tem acesso ao `tenant_id` via `/salesos_user_tenants`

### Erro 422: Unprocessable Entity

**Causa**: Payload com campos obrigatórios faltando ou formato inválido

**Solução**: Verifique a documentação do endpoint para ver campos obrigatórios

👉 **[Ver catálogo completo de erros →](errors.md)**

---

## 💬 Precisa de Ajuda?

- 📧 Email: [dev@play2sell.com](mailto:dev@play2sell.com)
- 🐛 Issues: [GitHub](https://github.com/felipeplay2sellcom/SalesOs-API/issues)
- 💬 Slack: Canal #salesos-api (interno)

---

<div align="center">
  <p><strong>🎉 Você completou o Quickstart!</strong></p>
  <p>
    <a href="../index.md">← Voltar ao início</a> •
    <a href="auth.md">Autenticação →</a>
  </p>
</div>
