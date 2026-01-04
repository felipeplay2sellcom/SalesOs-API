# ✅ Checklist Pré-Lançamento - API Pública

**Data:** 2026-01-04
**Status:** 🔒 Em Revisão

---

## 🔐 1. Segurança & Privacidade (CRÍTICO)

### **Remover Informações Sensíveis**

- [ ] **API Keys/Tokens**
  - ❌ Nenhuma API key real nos exemplos
  - ✅ Usar placeholders: `YOUR_API_KEY`, `YOUR_TOKEN`
  - ❌ Nenhum token Auth0 real
  - ❌ Nenhuma Supabase anon key real exposta

- [ ] **Dados de Clientes/Tenants**
  - ❌ Nenhum `tenant_id` real nos exemplos
  - ❌ Nenhum `user_id` real nos exemplos
  - ❌ Nenhum email/telefone real de clientes
  - ❌ Nenhum dado de lead real
  - ✅ Usar dados fictícios: `tenant-123`, `user-456`

- [ ] **URLs e IPs Internos**
  - ❌ Nenhuma URL interna (staging, dev, IPs privados)
  - ✅ Apenas URLs públicas documentadas
  - ❌ Nenhum endpoint de debug/admin exposto

- [ ] **Credenciais em Exemplos**
  - ❌ Nenhuma senha real
  - ❌ Nenhum certificado ou chave privada
  - ❌ Nenhuma connection string de banco

### **Verificar Exemplos de Request/Response**

- [ ] Todos os exemplos usam dados fictícios
- [ ] Nenhum payload com dados de produção
- [ ] Nenhum ID sequencial que revele volume de dados

---

## 📋 2. Documentação & Qualidade

### **Completude**

- [ ] Todos os endpoints têm descrição clara
- [ ] Todos os parâmetros estão documentados
- [ ] Exemplos de request para cada endpoint
- [ ] Exemplos de response (success e error)
- [ ] Códigos de erro documentados (400, 401, 403, 404, 500)
- [ ] Rate limits documentados
- [ ] Authentication flow completo

### **Precisão**

- [ ] Base URLs corretas (production, não staging/dev)
- [ ] Versões de API corretas
- [ ] Schemas de request/response refletem a realidade
- [ ] Exemplos testados e funcionais

### **Profissionalismo**

- [ ] Descrições em português ou inglês consistente
- [ ] Sem typos ou erros gramaticais
- [ ] Tom profissional (não informal demais)
- [ ] Sem comentários internos/TODOs visíveis

---

## 🚫 3. Endpoints & Permissões

### **Endpoints Privados/Admin**

- [ ] Verificar se algum endpoint deveria ser privado
- [ ] Endpoints de admin claramente marcados
- [ ] Debug endpoints removidos ou documentados como privados

**Revisar:**
```yaml
# Endpoints que podem ser sensíveis:
/rest/v1/salesos_users           # Dados de usuários
/rest/v1/salesos_tenants         # Dados de tenants
/rest/v1/salesos_user_roles      # RBAC
/rest/v1/identities              # Identidades Auth0
```

### **Autenticação & Autorização**

- [ ] Todos os endpoints têm `security` definida
- [ ] Bearer token obrigatório onde necessário
- [ ] Webhooks com validação de assinatura documentada
- [ ] Scopes/permissões documentados

---

## 🏢 4. Legal & Termos

### **Documentos Legais**

- [ ] **Terms of Service**
  - Criar: `docs/legal/terms-of-service.md`
  - Incluir: uso permitido, restrições, limitações

- [ ] **Privacy Policy**
  - Criar: `docs/legal/privacy-policy.md`
  - LGPD compliance (Brasil)
  - Explicar coleta/uso de dados

- [ ] **API Usage Policy**
  - Rate limits e fair use
  - Proibições (scraping, abuse)
  - Consequências de violação

### **Licenciamento**

- [ ] Verificar LICENSE
  - ✅ Atualmente: MIT (permite uso comercial)
  - ⚠️ Considerar mudar para proprietária se necessário
  - Definir se SDK gerado é open-source ou não

---

## 💼 5. Informações de Contato & Suporte

### **Adicionar ao README**

- [ ] Email de suporte: `api-support@play2sell.com`
- [ ] Email comercial: `sales@play2sell.com`
- [ ] Link para criar issues: GitHub Issues
- [ ] Status page: https://status.salesos.com
- [ ] Documentação: https://docs.salesos.com

### **SLA e Suporte**

- [ ] Definir horário de suporte
- [ ] Tempo de resposta esperado
- [ ] Uptime SLA (ex: 99.9%)
- [ ] Janelas de manutenção

---

## 📊 6. Rate Limiting & Quotas

### **Documentar Claramente**

- [ ] Requests por minuto (atual: 1,000/min)
- [ ] Events por dia por tenant (atual: 10,000/dia)
- [ ] Webhooks por minuto (atual: 100/min)
- [ ] Como solicitar aumento de limites

### **Implementação Técnica**

- [ ] Rate limiting está implementado no backend?
- [ ] Headers de rate limit expostos?
  ```
  X-RateLimit-Limit: 1000
  X-RateLimit-Remaining: 999
  X-RateLimit-Reset: 1234567890
  ```

---

## 🔧 7. Versionamento & Deprecation

### **Estratégia de Versões**

- [ ] Política de versionamento clara
- [ ] Quanto tempo v2.0 será suportada?
- [ ] Breaking changes comunicados com antecedência
- [ ] Changelog mantido atualizado

### **Deprecation Policy**

```markdown
- Avisar com 90 dias de antecedência
- Endpoint deprecated marcado claramente
- Alternativa/migração documentada
- Sunset headers em endpoints deprecated
```

---

## 🌐 8. Ambientes & URLs

### **Verificar Base URLs**

- [ ] **Production:** `https://api.play2sell.com` ✅
- [ ] **Staging:** NÃO expor na doc pública (ou documentar acesso)
- [ ] **Development:** NÃO expor na doc pública

### **Domínios Corretos**

- [ ] Certificado SSL válido
- [ ] DNS configurado corretamente
- [ ] CDN/Cloudflare se aplicável

---

## 🧪 9. Testes & Validação

### **Testar Endpoints Documentados**

- [ ] Todos os exemplos de curl funcionam
- [ ] Postman collections funcionam
- [ ] Response schemas corretos
- [ ] Error responses retornam conforme documentado

### **Ferramentas de Teste**

```bash
# Validar OpenAPI spec
swagger-cli validate openapi/salesos-api-v3.0.yaml

# Testar todos os endpoints
newman run postman/*.json
```

---

## 💰 10. Monetização & Acesso

### **Modelo de Acesso**

- [ ] API é totalmente gratuita?
- [ ] Requer cadastro/conta?
- [ ] Planos (Free, Pro, Enterprise)?
- [ ] Trial period?

### **Onboarding**

- [ ] Como obter API key está claro?
- [ ] Fluxo de signup documentado
- [ ] Quick start leva <5 minutos

---

## 🚀 11. Infraestrutura & Performance

### **Monitoramento**

- [ ] Logging de todas as requests
- [ ] Alertas configurados (uptime, errors)
- [ ] APM (Application Performance Monitoring)
- [ ] Error tracking (Sentry, etc.)

### **Capacidade**

- [ ] Load testing realizado
- [ ] Escalabilidade horizontal configurada
- [ ] Database otimizada para carga esperada
- [ ] CDN para assets estáticos

---

## 📢 12. Marketing & Comunicação

### **Materiais de Lançamento**

- [ ] Blog post anunciando a API
- [ ] Email para beta testers
- [ ] Social media posts
- [ ] Developer newsletter

### **Developer Experience**

- [ ] Tutoriais/guides além da referência
- [ ] Vídeos demonstrativos
- [ ] Use cases documentados
- [ ] Community/Discord/Forum

---

## ✅ CHECKLIST RÁPIDO - Antes de Publicar

**CRÍTICO (bloqueia publicação):**
```
[ ] Nenhuma API key/token real exposta
[ ] Nenhum dado de cliente real nos exemplos
[ ] Todos os endpoints requerem autenticação adequada
[ ] Base URLs apontam para produção
[ ] Terms of Service e Privacy Policy publicados
```

**IMPORTANTE (publicar com aviso):**
```
[ ] Rate limiting implementado e documentado
[ ] Todos os exemplos testados
[ ] Documentação 100% em inglês OU português (consistente)
[ ] Contatos de suporte definidos
[ ] Política de versionamento clara
```

**RECOMENDADO (pode publicar, melhorar depois):**
```
[ ] SDK oficial disponível
[ ] Portal de developer profissional
[ ] Status page público
[ ] Tutoriais em vídeo
[ ] Community forum
```

---

## 🎯 Próximos Passos

1. ✅ Revisar este checklist item por item
2. ✅ Criar issues para itens faltantes
3. ✅ Fazer security audit dos exemplos
4. ✅ Adicionar Terms of Service
5. ✅ Testar todos os endpoints documentados
6. ✅ Soft launch com beta testers
7. 🚀 **Public launch!**

---

**Última atualização:** 2026-01-04
**Responsável:** Play2Sell Tech Team
