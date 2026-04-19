# Security ASVS Matrix (Language-Agnostic)

This document defines a practical OWASP ASVS-aligned security baseline for AI agents rebuilding this booking system in any programming language.

## How To Use This Matrix

- `Requirement`: Security objective that must be implemented.
- `Control`: Concrete control expected in the solution.
- `Where`: Typical placement in architecture/runtime.
- `Verification`: Evidence/check proving compliance.
- `Status`: `Not Started`, `In Progress`, `Done`.

> This matrix is intentionally implementation-language neutral.

## ASVS Baseline Matrix

| ID | Requirement | Control | Where | Verification | Status |
|---|---|---|---|---|---|
| V1-1 | Secure design process exists | Threat modeling and abuse-case review required before coding | `docs/` + planning stage | Threat model file + reviewed checklist | Not Started |
| V1-2 | Security requirements are explicit | Security requirements documented with MUST/SHOULD wording | `docs/` | Review of requirements docs | In Progress |
| V2-1 | Authentication is strong | Password policy + optional OAuth + account status gates | Auth module | Automated auth tests | Not Started |
| V2-2 | Session/token lifecycle is controlled | Access token TTL, rotation strategy, logout revocation | Auth + token storage | Token lifecycle tests | Not Started |
| V3-1 | Authorization is enforced centrally | RBAC and permission checks in middleware/domain policy | Middleware + services | Endpoint permission matrix tests | Not Started |
| V3-2 | Deny-by-default access | Protected routes require explicit permission | Routing + middleware | Negative access tests (403) | Not Started |
| V4-1 | Input validation and sanitization | Schema validation at API boundary for body/query/params | API adapters | Validation tests + fuzz tests | Not Started |
| V4-2 | Output is safe | Output encoding and safe rendering rules for frontend | Frontend renderer | XSS-focused test cases | Not Started |
| V5-1 | Crypto usage is safe | Approved algorithms and key length policy documented | Security policy + code | Config review + static checks | Not Started |
| V6-1 | Secrets are never hardcoded | All secrets come from ENV/secret manager | Runtime config layer | Secret scanning + config tests | Not Started |
| V6-2 | Secret rotation is defined | Rotation cadence and emergency rotation runbook | Ops/security docs | Rotation drill evidence | Not Started |
| V7-1 | Error handling avoids leaks | Standardized error responses with no stack traces in prod | API error middleware | Security test for error leakage | Not Started |
| V8-1 | Data protection is defined | PII classification + minimization + retention/deletion rules | Data architecture docs | Data lifecycle tests/review | Not Started |
| V9-1 | Communication security is enforced | TLS-only in production, secure cookies/headers as needed | Ingress/runtime config | TLS and header scanning | Not Started |
| V10-1 | Logging and monitoring are secure | Security events logged without exposing secrets | Logging subsystem | Log review + redaction tests | Not Started |
| V11-1 | API and business logic abuse prevention | Rate limiting, anti-automation, brute-force protection | API gateway/middleware | Load + abuse scenario tests | Not Started |
| V12-1 | File and URL handling is validated | Strict MIME/type/URL validation and allowlists | Upload/integration modules | Unit/integration tests | Not Started |
| V13-1 | Configuration is hardened | Safe defaults, explicit production hardening profile | Runtime config | Config conformance checks | Not Started |
| V14-1 | Build/dependency security is validated | SCA/SAST in CI pipeline | CI/CD | CI security reports | Not Started |

## Required Security Artifacts

Every implementation generated from this repository must produce:

1. Security requirements checklist.
2. Threat model with abuse cases.
3. Endpoint authorization matrix.
4. Secrets inventory and rotation plan.
5. Security test report (authz, validation, leakage, rate limit).

## Verification Cadence

- On every pull request: lint, tests, secret scan, dependency scan.
- Before release: threat model review + manual security checklist.
- Quarterly: rotation drill and incident runbook tabletop.
