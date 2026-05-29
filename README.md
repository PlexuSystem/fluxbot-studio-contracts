# FluxBot Studio Contracts

Fuente única de verdad para contratos compartidos entre:

- `fluxbot-studio-ia-shopify`
- `fluxbot-studio-back-ia`
- `fluxbot-external-widget`

## Contenido

- `openapi/fluxbot-ia-api.v1.yaml`: contrato cross-repo IA/admin actual.
- `openapi/fluxbot-widget-api.v1.yaml`: contrato del canal embebible externo.
- `openapi/fluxbot-admin-api.v1.yaml`: contrato del canal admin Shopify.
- `openapi/fluxbot-shared-schemas.v1.yaml`: esquemas compartidos entre canales.
- `examples/`: ejemplos canónicos request/response.
- `docs/`: límites de responsabilidad, ownership de backend, canales y versionado.

## Scripts

```bash
npm run lint
npm run validate
npm run check
```
