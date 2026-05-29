# Backend ownership

`fluxbot-studio-back-ia` es el dueño único de:

- tenants
- tokens de widget
- dominios permitidos
- rate limiting
- conversaciones
- orquestación IA / RAG
- persistencia y observabilidad

`fluxbot-external-widget` solo actúa como cliente de canal y debe consumir exclusivamente:

- `openapi/fluxbot-widget-api.v1.yaml`

## Regla para agentes

Si la funcionalidad requiere persistencia, autenticación, tenant, dominio, límites o conversación, el cambio va en backend/contrato, no en el widget cliente.
