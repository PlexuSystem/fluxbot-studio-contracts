# FluxBot Contracts Constitution

## Core Principles

### I. Contract Source of Truth
This repository owns shared API contracts, examples, validation rules, and compatibility policy for cross-repo communication.

### II. No Runtime Business Logic
This repository must not implement backend runtime behavior, UI behavior, tenant storage, authentication execution, rate limiting, RAG, or LLM orchestration. It defines contracts consumed by other repos.

### III. OpenAPI First
Endpoint paths, request schemas, response schemas, public error schemas, authentication schemes, and examples must be represented in versioned OpenAPI before implementation in clients or backend.

### IV. Backward Compatibility
Widget v1 compatibility rules must be explicit. Breaking changes require a versioning decision before backend or widget implementation.

### V. Validation as Gate
Contract changes must pass Spectral/OpenAPI validation and include examples where they affect public API consumers.

## Governance Workflow

1. Register contract-owned behavior in `.openspec.json`.
2. Update `openapi/`, `examples/`, and `docs/` before runtime repos implement behavior.
3. Keep `specs/features/*/requirements.md` aligned with OpenSpec requirements.
4. Run `npm run qa:gate` before completion.

**Version**: 1.0.0 | **Ratified**: 2026-06-01 | **Last Amended**: 2026-06-01
