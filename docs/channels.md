# Channels

Familia FluxBot y ownership de canal:

- `fluxbot-studio-ia-shopify`: canal Admin Shopify.
- `fluxbot-external-widget`: canal web embebible externo (sin backend propio).
- `fluxbot-studio-back-ia`: backend central IA para todos los canales.
- `fluxbot-studio-contracts`: fuente única de verdad de contratos.

## Regla

Cada canal cliente consume el backend central mediante contrato versionado.  
Ningún canal cliente define backend paralelo.
