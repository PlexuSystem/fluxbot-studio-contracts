# Widget API Contract Requirements

## Objective

Define the contract repository responsibilities for the external widget API. This repo owns OpenAPI operations, shared public error schemas, canonical examples, and v1 compatibility policy.

## OpenSpec Requirements

### REQ-CONTRACT-001 — Widget API OpenAPI contract

The contracts repo defines the versioned OpenAPI contract for external widget APIs.

Acceptance criteria:
- OpenAPI includes widget chat and connection-test operations.
- Contract defines authentication, request, response, and error shapes.
- Spectral validation passes.

### REQ-CONTRACT-002 — Shared public error schema

The contracts repo defines a shared public error schema for widget endpoints.

Acceptance criteria:
- Error schema includes stable code, message, requestId, and retryable fields.
- `401`, `403`, `429`, and `500` examples reference the shared schema.
- Backend and widget requirements depend on the schema.

### REQ-CONTRACT-003 — Widget chat request/response examples

The contracts repo provides canonical widget chat request and response examples.

Acceptance criteria:
- Examples cover first message and follow-up conversation cases.
- Examples include metadata and usage fields.
- Examples are referenced by backend and widget docs.

### REQ-CONTRACT-004 — Backward compatibility policy for widget v1

The contracts repo documents backward compatibility rules for widget API v1.

Acceptance criteria:
- Breaking and non-breaking changes are defined for v1.
- Versioning expectations are documented for CDN/widget consumers.
- Compatibility policy is referenced by widget release requirements.

### REQ-CONTRACT-006 — Contrato de error público para chat de widget Shopify

Los errores públicos del widget deben mantener forma estable para que Shopify Admin, backend IA y storefront puedan serializar y renderizar fallos sin HTTP 500 no controlado ni objetos crudos.

Acceptance criteria:
- Error schema includes `error.code`, `error.message`, `requestId`, and `timestamp`.
- `500` examples use sanitized textual messages and never nested raw objects.
- Widget consumers can render backend failures without `[object Object]`.
- Root-cause investigations document endpoint mismatches between configured URLs and the real Shopify app proxy path.

## Dependencies

- `fluxbot-studio-back-ia` implements the runtime API.
- `fluxbot-external-widget` consumes the API from browser environments.
