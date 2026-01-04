# Erros & Troubleshooting

Guia completo de erros HTTP, códigos de resposta e como resolver problemas comuns.

---

## 📋 Estrutura de Erro Padrão

Todos os endpoints retornam erros no seguinte formato:

```json
{
  "error": "error_code",
  "message": "Descrição legível do erro",
  "details": {
    "field": "campo_com_erro",
    "reason": "Motivo específico"
  },
  "request_id": "req-uuid-123"
}
```

---

## 🔴 Códigos HTTP

### 400 - Bad Request

**Causa**: Request malformado ou parâmetros inválidos

**Exemplos**:

```json
{
  "error": "invalid_request",
  "message": "Campo 'customer_email' é obrigatório",
  "details": {
    "field": "customer_email",
    "reason": "required_field_missing"
  }
}
```

**Solução**: Verifique a documentação do endpoint e envie todos os campos obrigatórios

---

### 401 - Unauthorized

**Causa**: Token ausente, inválido ou expirado

**Exemplos comuns**:

#### 1. Token ausente
```json
{
  "error": "missing_authorization",
  "message": "Header 'Authorization' não encontrado"
}
```

**Solução**:
```bash
curl -H "Authorization: Bearer SEU_TOKEN" ...
```

#### 2. Token expirado
```json
{
  "error": "token_expired",
  "message": "Token JWT expirou em 2026-01-03T10:00:00Z"
}
```

**Solução**: Gere um novo token via `/oauth/token`

#### 3. Token inválido
```json
{
  "error": "invalid_token",
  "message": "Assinatura JWT inválida"
}
```

**Solução**: Verifique se está usando o token correto do Auth0

---

### 403 - Forbidden

**Causa**: Usuário autenticado mas sem permissão

**Exemplos**:

#### 1. Sem acesso ao tenant
```json
{
  "error": "tenant_access_denied",
  "message": "Usuário não tem acesso ao tenant 'tenant-uuid-abc'",
  "details": {
    "user_id": "user-uuid-123",
    "tenant_id": "tenant-uuid-abc"
  }
}
```

**Solução**: Verifique seus tenants via `GET /salesos_user_tenants`

#### 2. Capability faltando
```json
{
  "error": "missing_capability",
  "message": "Tenant não possui a capability 'workflows.create'",
  "details": {
    "required_capability": "workflows.create",
    "tenant_plan": "free"
  }
}
```

**Solução**: Upgrade do plano ou solicite a capability específica

---

### 404 - Not Found

**Causa**: Recurso não existe

```json
{
  "error": "not_found",
  "message": "Oportunidade com ID 'opp-123' não encontrada",
  "details": {
    "resource_type": "opportunity",
    "resource_id": "opp-123"
  }
}
```

**Solução**: Verifique se o ID está correto e se o recurso pertence ao tenant

---

### 422 - Unprocessable Entity

**Causa**: Validação de negócio falhou

**Exemplos**:

#### 1. Email duplicado
```json
{
  "error": "duplicate_email",
  "message": "Email 'joao@example.com' já está em uso",
  "details": {
    "field": "customer_email",
    "value": "joao@example.com"
  }
}
```

#### 2. Valor fora do range
```json
{
  "error": "value_out_of_range",
  "message": "Valor da oportunidade deve ser entre R$ 100 e R$ 1.000.000",
  "details": {
    "field": "value",
    "value": 50,
    "min": 100,
    "max": 1000000
  }
}
```

---

### 429 - Too Many Requests

**Causa**: Rate limit excedido

```json
{
  "error": "rate_limit_exceeded",
  "message": "Limite de 1000 requisições/minuto excedido",
  "details": {
    "limit": 1000,
    "window": "1 minute",
    "retry_after": 45
  }
}
```

**Solução**: Aguarde `retry_after` segundos antes de tentar novamente

**Rate Limits por categoria**:
- REST API: 1000 req/min por IP
- Edge Functions: 100 req/min por API key
- Copilot IA: 100 req/hora por usuário
- EventService: 10000 eventos/dia por tenant

---

### 500 - Internal Server Error

**Causa**: Erro no servidor (não é culpa sua!)

```json
{
  "error": "internal_server_error",
  "message": "Erro inesperado ao processar requisição",
  "request_id": "req-uuid-456"
}
```

**Solução**:
1. Tente novamente em alguns segundos
2. Se persistir, contate o suporte com o `request_id`

---

## 🐛 Problemas Comuns

### Problema: "CORS error" no navegador

**Causa**: Request cross-origin sem headers corretos

**Solução**:
```javascript
fetch('https://api.play2sell.com/rest/v1/salesos_events', {
  headers: {
    'Authorization': `Bearer ${token}`,
    'apikey': ANON_KEY,
    'Content-Type': 'application/json'
  }
})
```

---

### Problema: "Row Level Security violation"

**Causa**: Tentando acessar dados de outro tenant

**Solução**: Sempre inclua o `tenant_id` correto no payload

```json
{
  "tenant_id": "seu-tenant-id",
  "user_id": "seu-user-id",
  ...
}
```

---

### Problema: Workflow não dispara após evento

**Causa possível 1**: Workflow não está ativo

**Verificar**:
```bash
GET /rest/v1/salesos_workflows?select=*&status=eq.active
```

**Causa possível 2**: Evento não corresponde ao trigger

**Verificar**:
```bash
GET /rest/v1/salesos_workflow_triggers?select=*&event_type=eq.lead.created
```

---

### Problema: Copilot retorna sugestões genéricas

**Causa**: RAG sem documentos do tenant

**Solução**: Faça upload de documentos:
```bash
POST /rest/v1/salesos_copilot_documents
{
  "tenant_id": "...",
  "title": "Manual de Vendas",
  "content": "..."
}
```

Depois gere embeddings:
```bash
POST /functions/v1/generate-embeddings
{
  "document_id": "doc-uuid"
}
```

---

## 📊 Monitoramento de Erros

### Ver últimos erros (logs)

```bash
# Logs de API
GET https://api.play2sell.com/logs/api?limit=100

# Logs de workflows
GET /rest/v1/salesos_workflow_runs?select=*&status=eq.failed
```

---

## 💡 Dicas para Debugging

### 1. Sempre capture o request_id

```bash
curl -i https://api.play2sell.com/... \
  | grep -i request-id
```

### 2. Use query param `select` para ver campos específicos

```bash
GET /rest/v1/salesos_events?select=id,event_type,created_at
```

### 3. Filtre por timestamp para debugging

```bash
GET /rest/v1/salesos_events?created_at=gte.2026-01-04T00:00:00Z
```

### 4. Ordene por created_at desc para ver últimos registros

```bash
GET /rest/v1/salesos_events?order=created_at.desc&limit=10
```

---

## 📞 Precisa de Ajuda?

Se você:
- ✅ Verificou este guia
- ✅ Tentou as soluções sugeridas
- ❌ Ainda está com o problema

Entre em contato:

- 📧 **Email**: dev@play2sell.com (inclua o `request_id`)
- 🐛 **GitHub Issues**: Reporte bugs com reprodução
- 💬 **Slack**: #salesos-api (suporte interno)

---

<div align="center">
  <p>
    <a href="../index.md">← Início</a> •
    <a href="quickstart.md">Quickstart</a> •
    <a href="#reference/salesos-api">API Reference →</a>
  </p>
</div>
