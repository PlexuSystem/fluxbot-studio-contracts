# Changelog

## Unreleased

- Añadido `POST /api/v1/privacy/requests` al contrato compartido IA para registrar de forma autenticada e idempotente las solicitudes Shopify GDPR `customers/data_request`, `customers/redact` y `shop/redact`.
- El contrato exige `X-Shop-Domain`, evita devolver datos personales y diferencia las solicitudes de cliente de la redacción completa de tienda.

## 0.1.0

- Bootstrap inicial de `fluxbot-studio-contracts`.
- OpenAPI v1 para endpoints cruzados IA/Admin.
- Ejemplos de requests/responses.

## 0.2.0

- Añadido contrato `openapi/fluxbot-widget-api.v1.yaml` para canal `external-widget`.
- Añadidos `openapi/fluxbot-admin-api.v1.yaml` y `openapi/fluxbot-shared-schemas.v1.yaml`.
- Añadidos ejemplos de widget chat y connection-test.
- Añadida documentación de channels, instalación y ownership de backend.

## 0.4.0

- Añadido endpoint `POST /api/v1/catalog/sync` con esquemas `CatalogSyncRequest` y `CatalogSyncEnvelope`.
- Añadido endpoint `GET /api/v1/assistant-config` para leer la configuración del asistente por tienda.
- Añadido endpoint `POST /api/v1/assistant-config` para crear o actualizar `AssistantConfig` (nombre, persona, tono, instrucciones, mensaje de bienvenida, idioma, categorías).
- Añadidos esquemas `AssistantConfigRequest` y `AssistantConfigEnvelope`.
- Mejora IA: RAG conectado al chat, system prompt dinámico por tienda, guardrail anti-repetición y sincronización de catálogo desde Shopify Admin GraphQL.

## 0.3.0

- Actualizado el contrato de billing para reflejar App Pricing / App Events como ruta preferida y legacy billing como compatibilidad.
- Ajustado `paymentInfo` en widget connection-test y añadida documentación de estrategia de pagos.
