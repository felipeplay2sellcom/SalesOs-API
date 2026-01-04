# Security and Privacy Audit Report
## SalesOS Platform API v3.0.0 - OpenAPI Specification

**Audit Date:** 2026-01-04
**File Audited:** `/Users/play2sell/SalesOS-API/openapi/salesos-api-v3.0.yaml`
**Total Lines:** 3227

---

## Executive Summary

This audit identified **7 security issues** across multiple severity levels in the OpenAPI specification. The issues range from exposure of internal development URLs to use of placeholder values that could leak sensitive patterns. No critical security breaches (real credentials, production tokens, or PII) were found, but several improvements are recommended to harden the API documentation.

**Severity Breakdown:**
- CRITICAL: 0
- HIGH: 2
- MEDIUM: 3
- LOW: 2

---

## 1. Sensitive Data in Examples

### 1.1 Generic Placeholder Credentials
**Severity:** LOW
**Lines:** 180-184

**Issue:**
The Auth0 login example uses generic placeholder values that could reveal authentication patterns:

```yaml
example:
  grant_type: password
  client_id: your_client_id
  client_secret: your_client_secret
  audience: https://api.play2sell.com
  username: user@example.com
  password: secure_password
```

**Risk:**
- Reveals the production audience URL (`https://api.play2sell.com`)
- Generic placeholders may not sufficiently obscure actual credential formats

**Recommendation:**
Use more abstract placeholders that don't match real patterns:

```yaml
example:
  grant_type: password
  client_id: "AUTH0_CLIENT_ID_HERE"
  client_secret: "AUTH0_CLIENT_SECRET_HERE"
  audience: "https://api.example.com"
  username: "user@example.com"
  password: "********"
```

---

### 1.2 JWT Token Example Format
**Severity:** LOW
**Lines:** 240

**Issue:**
Partial JWT token shown in example:

```yaml
example:
  auth0_token: eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Risk:**
- Reveals JWT structure (algorithm RS256)
- Could provide hints about token format to attackers

**Recommendation:**
Use a completely generic token placeholder:

```yaml
example:
  auth0_token: "AUTH0_ACCESS_TOKEN_FROM_LOGIN_RESPONSE"
```

---

### 1.3 UUID Examples Using Consistent Pattern
**Severity:** MEDIUM
**Lines:** Multiple (280, 349, 360, 372, 521-533, 570, 703, 786, 813, 841, 992, 1382, etc.)

**Issue:**
The specification uses a predictable UUID pattern throughout examples:
- `123e4567-e89b-12d3-a456-426614174000` (tenant_id)
- `123e4567-e89b-12d3-a456-426614174001` (another tenant)
- `123e4567-e89b-12d3-a456-426614174002` (user_id)

**Risk:**
- Predictable patterns could expose internal ID structure
- Sequential IDs suggest enumeration vulnerability (though UUIDs should be random)
- Easier for attackers to test API endpoints with realistic-looking IDs

**Recommendation:**
Use obviously fake UUIDs with descriptive prefixes:

```yaml
# For tenant IDs
tenant_id: "00000000-0000-0000-0000-000000000001"

# For user IDs
user_id: "ffffffff-ffff-ffff-ffff-ffffffffffff"

# Or use descriptive placeholders
tenant_id: "TENANT_UUID_HERE"
user_id: "USER_UUID_HERE"
```

---

### 1.4 Real Email Domain in Examples
**Severity:** LOW
**Lines:** 184, 2144

**Issue:**
Examples use `user@example.com` and `joao@exemplo.com` which are acceptable, but also reference the real business email domain:

```yaml
email: joao@exemplo.com  # Line 2144
```

**Risk:**
- Minimal risk as `example.com` is reserved for examples
- Portuguese example (`exemplo.com`) is appropriate

**Recommendation:**
Continue using `example.com`, `example.org`, or `exemplo.com` (Portuguese). Current usage is acceptable.

---

## 2. Internal URLs and Endpoints

### 2.1 Development Environment Exposed
**Severity:** HIGH
**Lines:** 66, 82-83

**Issue:**
The OpenAPI specification publicly documents the development/localhost server:

```yaml
## Environments

- **Production**: https://api.play2sell.com
- **Staging**: https://staging-api.play2sell.com
- **Development**: http://localhost:5173

servers:
  - url: https://api.play2sell.com
    description: Production Environment
  - url: https://staging-api.play2sell.com
    description: Staging Environment
  - url: http://localhost:5173
    description: Local Development
```

**Risk:**
- Exposes staging environment URL which should be access-controlled
- Reveals development port (5173) used internally
- Localhost entry is unnecessary in public documentation
- Attackers could target staging environment for vulnerabilities

**Recommendation:**
Remove internal environments from public API documentation:

```yaml
## Environments

- **Production**: https://api.play2sell.com

servers:
  - url: https://api.play2sell.com
    description: Production Environment
```

If staging/development environments are needed for internal documentation, use separate OpenAPI files or parameter-based configuration.

---

## 3. Security Issues

### 3.1 Missing Security on Some Endpoints
**Severity:** HIGH
**Lines:** 2034-2076, 2077-2104

**Issue:**
Webhook endpoints use custom security schemes but don't enforce standard bearer authentication:

**Meta Webhook (Line 2034-2076):**
```yaml
/api/webhooks/meta-leads:
  post:
    security:
      - metaSignature: []
```

**Zapier Webhook (Line 2077-2104):**
```yaml
/api/webhooks/zapier-leads:
  post:
    security:
      - zapierSecret: []
```

**Risk:**
- Custom authentication schemes may be less secure than standard OAuth/JWT
- No documentation of how signature validation actually works
- Potential for replay attacks if timestamp validation is missing
- Webhook secrets could be compromised

**Recommendation:**
1. Add detailed security documentation for webhook signature validation
2. Document timestamp/replay attack protection
3. Recommend secret rotation policies
4. Consider adding IP whitelisting requirements

Enhanced documentation example:

```yaml
/api/webhooks/meta-leads:
  post:
    summary: Meta Ads Webhook - New Lead
    description: |
      Receive lead capture notifications from Meta Ads.

      **Security Requirements:**
      - HMAC SHA256 signature validation using shared secret
      - Timestamp verification (reject requests older than 5 minutes)
      - IP whitelist: Meta's published webhook IP ranges
      - Rate limiting: 100 requests/minute per endpoint

      **Signature Validation:**
      1. Extract X-Meta-Signature header
      2. Compute HMAC-SHA256(shared_secret, request_body)
      3. Compare computed hash with header value
      4. Reject if mismatch or missing
```

---

### 3.2 Overly Detailed Error Messages
**Severity:** MEDIUM
**Lines:** 2259-2302

**Issue:**
Error response examples expose internal implementation details:

```yaml
Forbidden:
  description: Insufficient permissions (RLS policy violation)
  content:
    application/json:
      example:
        message: new row violates row-level security policy
        code: 42501
```

**Risk:**
- Reveals database technology (Row-Level Security is PostgreSQL/Supabase specific)
- Exposes PostgreSQL error code (42501)
- Could help attackers understand internal architecture
- May leak table/column names in actual errors

**Recommendation:**
Use generic error messages in documentation:

```yaml
Forbidden:
  description: Insufficient permissions to access this resource
  content:
    application/json:
      example:
        message: "Insufficient permissions"
        code: "FORBIDDEN"
        details: "You do not have access to this resource"
```

In actual implementation, ensure detailed error messages are only shown in development mode, never in production.

---

### 3.3 No Rate Limiting Documentation per Endpoint
**Severity:** MEDIUM
**Lines:** 55-60

**Issue:**
Global rate limits are documented but not enforced per endpoint:

```yaml
## Rate Limiting

- **API Calls**: 1000 requests/minute per IP
- **Events**: 10000 events/day per tenant
- **Webhooks**: 100 requests/minute per endpoint
- **Copilot Suggestions**: 100 requests/hour per user
```

**Risk:**
- Endpoints with different risk profiles share same rate limit
- Authentication endpoints should have stricter limits
- No documented rate limit headers (X-RateLimit-Remaining, etc.)
- No guidance on handling 429 responses

**Recommendation:**
Add rate limit headers to all responses and document per-endpoint limits:

```yaml
- name: X-RateLimit-Limit
  in: header
  description: Maximum requests allowed in the time window
  schema:
    type: integer
    example: 1000
- name: X-RateLimit-Remaining
  in: header
  description: Remaining requests in current window
  schema:
    type: integer
    example: 995
- name: X-RateLimit-Reset
  in: header
  description: Unix timestamp when the rate limit resets
  schema:
    type: integer
    example: 1641024000
```

Implement stricter limits for sensitive endpoints:
- `/oauth/token`: 5 requests/minute per IP
- `/functions/v1/token_exchange_edge_function`: 10 requests/minute per user
- `/rest/v1/rpc/salesos_emit_event`: 100 requests/minute per tenant

---

## 4. Data Privacy (LGPD/GDPR)

### 4.1 PII Fields Not Marked as Sensitive
**Severity:** MEDIUM
**Lines:** 2519-2524, 2556-2560, 2893-2899

**Issue:**
Schema definitions contain PII fields without privacy annotations:

```yaml
Opportunity:
  properties:
    customer_name:
      type: string
    customer_phone:
      type: string
    customer_email:
      type: string
      format: email

User:
  properties:
    email:
      type: string
      format: email
    phone:
      type: string
    document:
      type: string  # CPF/CNPJ - highly sensitive
```

**Risk:**
- No indication these fields require special handling
- Missing data classification (public, internal, confidential, restricted)
- No encryption requirements documented
- LGPD/GDPR compliance unclear

**Recommendation:**
Add privacy annotations using OpenAPI extensions:

```yaml
Opportunity:
  properties:
    customer_name:
      type: string
      description: "Customer full name"
      x-privacy-level: "confidential"
      x-pii: true
      x-lgpd-category: "personal_data"
    customer_phone:
      type: string
      description: "Customer phone number"
      x-privacy-level: "confidential"
      x-pii: true
      x-lgpd-category: "personal_data"
    customer_email:
      type: string
      format: email
      description: "Customer email address"
      x-privacy-level: "confidential"
      x-pii: true
      x-lgpd-category: "personal_data"

User:
  properties:
    document:
      type: string
      description: "Government-issued document (CPF/CNPJ)"
      x-privacy-level: "restricted"
      x-pii: true
      x-lgpd-category: "sensitive_personal_data"
      x-encryption: "required"
```

Add documentation section:

```markdown
## Data Privacy & LGPD Compliance

This API handles personal data subject to LGPD (Lei Geral de Proteção de Dados)
and GDPR regulations.

**Data Classification:**
- **Public**: Can be freely shared
- **Internal**: For authorized users only
- **Confidential**: Encrypted in transit, restricted access
- **Restricted**: Encrypted at rest and in transit, audit logging required

**PII Fields:**
Fields marked with `x-pii: true` contain personally identifiable information and
must be handled according to LGPD Article 5, VII.

**Data Subject Rights:**
- Right to access: GET /rest/v1/salesos_users?email=eq.{email}
- Right to deletion: DELETE /rest/v1/salesos_users?id=eq.{id}
- Right to portability: Contact privacy@play2sell.com
```

---

## 5. Additional Recommendations

### 5.1 Add Security Best Practices Section

Add a security section to the API documentation:

```markdown
## Security Best Practices

### Authentication
1. **Never share API tokens** - Treat JWT tokens as passwords
2. **Token rotation** - Tokens expire after 24 hours (86400 seconds)
3. **Secure storage** - Store tokens in secure, encrypted storage (never localStorage)
4. **HTTPS only** - All API calls must use HTTPS in production

### Authorization
1. **Principle of least privilege** - Request minimum required capabilities
2. **Multi-tenant isolation** - Always include tenant_id in queries
3. **RLS enforcement** - Row-level security prevents cross-tenant data access

### API Keys & Secrets
1. **Environment variables** - Never hardcode credentials
2. **Secret rotation** - Rotate webhook secrets every 90 days
3. **Audit logging** - Monitor all authentication failures

### Data Protection
1. **Input validation** - Validate all user inputs client-side and server-side
2. **Output encoding** - Prevent XSS in API responses
3. **SQL injection** - Use parameterized queries (handled by Supabase)
```

### 5.2 Add CORS Documentation

Document CORS policies:

```markdown
## Cross-Origin Resource Sharing (CORS)

**Allowed Origins:**
- Production: https://app.play2sell.com
- Staging: https://staging.play2sell.com

**Allowed Methods:** GET, POST, PATCH, DELETE, OPTIONS

**Allowed Headers:**
- Authorization
- Content-Type
- Prefer
- X-Client-Info

**Exposed Headers:**
- Content-Range
- X-RateLimit-*

**Credentials:** true (cookies and authorization headers allowed)
```

### 5.3 Add Webhook Security Section

Add comprehensive webhook security documentation:

```markdown
## Webhook Security

### Signature Verification

All incoming webhooks must verify request signatures:

**Meta Webhooks:**
```python
import hmac
import hashlib

def verify_meta_signature(payload: bytes, signature: str, secret: str) -> bool:
    expected = hmac.new(
        secret.encode('utf-8'),
        payload,
        hashlib.sha256
    ).hexdigest()
    return hmac.compare_digest(f"sha256={expected}", signature)
```

**Zapier Webhooks:**
```python
def verify_zapier_secret(header_secret: str, configured_secret: str) -> bool:
    return hmac.compare_digest(header_secret, configured_secret)
```

### Replay Attack Prevention

1. Include timestamp in webhook payload
2. Reject requests older than 5 minutes
3. Store processed webhook IDs to prevent duplicates
4. Use idempotency keys for critical operations
```

### 5.4 Implement Content Security Policy

Add CSP headers documentation:

```markdown
## Content Security Policy

The API returns the following CSP headers to prevent XSS:

```
Content-Security-Policy:
  default-src 'none';
  script-src 'none';
  style-src 'none';
  img-src 'none';
  connect-src 'self';
  font-src 'none';
  object-src 'none';
  media-src 'none';
  frame-src 'none';
```
```

---

## 6. Summary of Fixes

### Immediate Actions (HIGH Priority)

1. **Remove staging and localhost URLs** from public documentation (Lines 66, 82-83)
2. **Add detailed webhook security validation** documentation (Lines 2034-2104)
3. **Implement stricter rate limits** for authentication endpoints

### Short-term Actions (MEDIUM Priority)

4. **Replace sequential UUID examples** with generic placeholders (Multiple lines)
5. **Add privacy annotations** to all PII fields (Lines 2519+)
6. **Sanitize error messages** to avoid exposing internal architecture (Lines 2259-2302)
7. **Document rate limit headers** and per-endpoint limits

### Long-term Actions (LOW Priority)

8. **Replace JWT token example** with generic placeholder (Line 240)
9. **Add security best practices section** to documentation
10. **Document CORS policies** explicitly
11. **Add LGPD/GDPR compliance section**

---

## 7. Safe Example Values

Use these safe example values throughout the specification:

```yaml
# UUIDs
tenant_id: "00000000-0000-0000-0000-000000000001"
user_id: "11111111-1111-1111-1111-111111111111"
opportunity_id: "22222222-2222-2222-2222-222222222222"
suggestion_id: "33333333-3333-3333-3333-333333333333"

# Emails
user_email: "user@example.com"
customer_email: "customer@example.org"

# Phone Numbers (using invalid format)
phone: "+55 11 00000-0000"

# Names
customer_name: "Jane Doe"
user_name: "John Smith"

# Tokens
auth0_token: "AUTH0_TOKEN_FROM_LOGIN_RESPONSE"
access_token: "SUPABASE_ACCESS_TOKEN_FROM_EXCHANGE"

# Credentials
client_id: "your-auth0-client-id"
client_secret: "your-auth0-client-secret"
api_key: "your-api-key-here"

# URLs
production_url: "https://api.example.com"
callback_url: "https://app.example.com/callback"
```

---

## 8. Compliance Checklist

- [ ] Remove internal URLs (staging, localhost) from public docs
- [ ] Add LGPD/GDPR compliance section
- [ ] Document data retention policies
- [ ] Add data subject rights endpoints documentation
- [ ] Implement audit logging for PII access
- [ ] Document encryption requirements (TLS 1.3+)
- [ ] Add privacy policy link
- [ ] Document data processing agreements for third parties
- [ ] Implement consent management for marketing webhooks
- [ ] Add cookie policy (if applicable)

---

## Conclusion

The OpenAPI specification is generally well-structured and follows industry best practices. However, the exposure of internal infrastructure details (staging URLs, localhost) and the use of predictable example IDs represent the most significant security concerns.

**Overall Security Rating: B+**

With the recommended changes implemented, the specification would achieve an **A** rating.

**Estimated Effort:**
- High priority fixes: 2-4 hours
- Medium priority fixes: 4-8 hours
- Low priority improvements: 4-6 hours
- **Total: 10-18 hours**

---

**Auditor Note:** No actual credentials, production tokens, or real PII were found in the specification. All issues identified are preventative in nature to harden the API documentation against potential security risks.

**Last Updated:** 2026-01-04
**Next Audit Recommended:** After implementing high priority fixes, or quarterly
