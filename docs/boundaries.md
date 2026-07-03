# Boundaries

## Entra en este repo

- Contrato HTTP entre canales (`fluxbot-studio-ia-shopify`, `fluxbot-external-widget`) y `fluxbot-studio-back-ia`.
- Esquemas de request/response para rutas compartidas.
- Ejemplos canónicos de payload.

## No entra en este repo

- Requisitos de implementación internos (OpenSpec local).
- Lógica de negocio específica de frontend o backend.
- DTOs internos que no cruzan repos.
- Backends paralelos por canal cliente.

## Decisiones de contrato relacionadas con pagos

- La estrategia de pagos vive como contrato/documentación compartida.
- App Pricing / App Events son el valor por defecto para planes nuevos.
- Legacy billing solo permanece por compatibilidad.
- `paymentInfo` en widget connection-test es una salida contractual, no una obligación de implementación interna.
