# Instrucciones para Agentes de IA — FluxBot Studio Contracts

## ⚠️ CRITERIOS UNIFICADOS (2026-05-20)

**Lee primero:** `../UNIFIED_AGENT_CRITERIA.md`

Contiene:
- ✅ Instrucciones obligatorias de referencia
- ✅ Reglas críticas para testing, GraphQL, security
- ✅ Flujo mínimo por cambio
- ✅ Checklist pre-PR
- ✅ Red flags para IA

**Este archivo define reglas específicas para el repo de contratos.**

---

## 🔁 Regla Git automática obligatoria

Este repo debe tener hooks activos (`npm run githooks:install`) con este flujo:
1. `pre-commit` ejecuta `qa:gate`.
2. `post-commit` sincroniza (`fetch` + `pull --rebase --autostash` si aplica).
3. `post-commit` re-ejecuta `qa:gate`.
4. `post-commit` hace `push`.
5. Si hay conflicto, resolver y repetir `qa:gate` antes de empujar.

---

## 🎯 Propósito del Repo

Este es el **repositorio de contratos compartidos** de la familia FluxBot.

**Fuente única de verdad** de la comunicación entre:
- `fluxbot-studio-ia-shopify` (Admin Shopify)
- `fluxbot-studio-back-ia` (Backend IA central)
- `fluxbot-external-widget` (Widget embebible externo)

---

## 📋 Regla Crítica: Este Repo Define el Contrato

- ✅ **Versión principal** de todos los OpenAPI YAML
- ✅ **Validación centralizada** con Spectral
- ✅ **Ejemplos válidos** con requests/responses reales
- ✅ **Breaking changes documentadas**
- ✅ **Changelog actualizado**

## 🧩 Regla de widgets locales y rutas

Requisito raíz: `REQ-ROOT-012`.

Los contratos deben mantener la separación entre:
- Shopify storefront widget: theme app extension que usa app proxy `/apps/fluxbot/*`.
- SDK externo: `chat-widget.js`, CDN o local `http://localhost:3004`, con endpoint explícito en desarrollo (`data-endpoint="http://localhost:3001"`).

Cualquier cambio contractual que afecte chat/config del widget, ejemplos de instalación, rutas `/apps/fluxbot/*`, endpoints del SDK externo o defaults de entorno debe actualizar OpenSpec/SpecKit relacionados (`REQ-ROOT-012`, `REQ-IA-SHOPIFY-009`, `REQ-WIDGET-011`, `specs/006-local-widget-endpoint-routing/`).

---

## 📁 Estructura Obligatoria

```
fluxbot-studio-contracts/
├── openapi/
│   ├── fluxbot-admin-api.v1.yaml       # Admin Shopify ↔ Backend IA
│   ├── fluxbot-widget-api.v1.yaml      # Widget ↔ Backend IA
│   ├── fluxbot-ia-api.v1.yaml          # (Alias de admin-api para backwards compat)
│   └── fluxbot-shared-schemas.v1.yaml  # Esquemas compartidos
├── examples/
│   ├── chat.request.json
│   ├── chat.response.json
│   ├── embeddings-search.request.json
│   ├── embeddings-search.response.json
│   ├── widget-chat.request.json
│   ├── widget-chat.response.json
│   ├── widget-connection-test.request.json
│   └── widget-connection-test.response.json
├── docs/
│   ├── boundaries.md
│   ├── channels.md
│   ├── versioning.md
│   ├── breaking-changes.md
│   ├── widget-installation.md
│   └── backend-ownership.md
├── package.json
├── .spectral.yaml
├── .github/workflows/validate-contracts.yml
├── CHANGELOG.md
└── README.md
```

---

## ✅ Reglas de OpenAPI

### Formato

- ✅ OpenAPI 3.1.0+ (no 3.0.x)
- ✅ YAML bien formado (validado con Spectral)
- ✅ `operationId` único para cada endpoint
- ✅ Esquemas con `$ref` a `#/components/schemas/`
- ✅ Ejemplos JSON válidos en `examples/`

### Naming

- 📝 Rutas: `/api/v1/resource/action` (kebab-case)
- 📝 Métodos: POST para escritura, GET para lectura
- 📝 Schemas: `PascalCase` (ej: `ChatRequest`, `ChatResponse`)
- 📝 Propiedades: `camelCase` (ej: `sessionId`, `messageId`)

### Security

- ✅ Definir `securitySchemes` (ej: Bearer token, API key)
- ✅ Aplicar `security` a endpoints que lo requieren
- ✅ Documentar requirements (ej: "Widget token required")
- ✅ Nunca poner secrets en comentarios

---

## 🔄 Flujo de Cambios

### Cambio A: Nuevo Campo en Respuesta del Chat

**Paso 1: Este repo**
```bash
# Editar openapi/fluxbot-admin-api.v1.yaml
# o openapi/fluxbot-widget-api.v1.yaml
# Añadir campo en respuesta

# Actualizar ejemplo correspondiente en examples/
# Validar con Spectral
npm run lint

# Actualizar CHANGELOG.md con breaking change o feature
# Versionar en package.json
```

**Paso 2: Backend (`fluxbot-studio-back-ia`)**
```bash
npm run contracts:pull
# Implementar el nuevo campo
```

**Paso 3: Cliente (`fluxbot-studio-ia-shopify` o `fluxbot-external-widget`)**
```bash
npm run contracts:pull
npm run api:generate  # Regenerar tipos
# Usar el campo nuevo
```

### Cambio B: Nuevo Endpoint

**Paso 1: Este repo**
```bash
# Definir path completo en OpenAPI
# Definir request/response schemas
# Agregar en examples/
# Validar validación Spectral
npm run lint
```

**Paso 2-3:** Igual al cambio A

---

## 🚫 Prohibido en Este Repo

- ❌ Inventar esquemas sin coordinar con clientes
- ❌ Cambiar tipos de datos de campo ya publicado (breaking)
- ❌ Remover campos sin deprecation
- ❌ No documentar breaking changes
- ❌ No actualizar CHANGELOG
- ❌ No ejemplificar request/response
- ❌ Hardcodear datos reales (tokens, emails, dominios)
- ❌ Mezclar especificaciones de diferentes versiones

---

## ✅ Permitido en Este Repo

- ✅ Agregar campos opcionales (nuevas propiedades con `required: []`)
- ✅ Deprecar campos (documentar en schema y CHANGELOG)
- ✅ Crear nuevos endpoints
- ✅ Mejorar descripción y documentación
- ✅ Validar ejemplos
- ✅ Versionar con semver (package.json + info.version OpenAPI)
- ✅ Lintear con Spectral
- ✅ Documentar límites entre canales

---

## 📋 Canales Soportados

### Canal Admin Shopify

**Endpoints:**
- `POST /api/v1/chat` → Chat con IA
- `POST /api/v1/intent/detect` → Detección de intención
- `POST /api/v1/embeddings/search` → Búsqueda RAG
- `POST /api/v1/triggers/evaluate` → Evaluación de triggers

**Especificación:** `openapi/fluxbot-admin-api.v1.yaml`

### Canal Widget Externo

**Endpoints:**
- `POST /api/v1/widget/chat` → Chat públicamente accesible
- `POST /api/v1/widget/connection-test` → Validar token + dominio

**Especificación:** `openapi/fluxbot-widget-api.v1.yaml`

### Esquemas Compartidos

**Definición:** `openapi/fluxbot-shared-schemas.v1.yaml`

Errores públicos, tipos comunes, etc.

---

## 🔧 Scripts Disponibles

```bash
# Lintear OpenAPI con Spectral
npm run lint

# Validar contratos
npm run validate

# Ver definición de un endpoint
npm run check

# Verificar ejemplos JSON
npm run check:examples
```

---

## 📚 Documentación Obligatoria

### `docs/boundaries.md`

Explica:
- Qué hace cada canal
- Qué NO hace
- Límites de responsabilidad

### `docs/channels.md`

Describe:
- Admin Shopify channel
- Widget channel
- Shared schemas

### `docs/versioning.md`

Define:
- Estrategia de versioning (semver)
- Cómo cambiar sin breaking
- Deprecation policy

### `docs/breaking-changes.md`

Lista:
- Cambios que son breaking
- Cómo comunicarlos
- Cómo migrar

---

## ✅ Checklist Pre-PR (Contratos)

- [ ] OpenAPI válido (Spectral lint pasa)
- [ ] Cambio está documentado en CHANGELOG.md
- [ ] Ejemplos JSON actualizados (request + response)
- [ ] Versionado: `package.json` + `info.version` en OpenAPI
- [ ] Sin datos reales/sensibles en ejemplos
- [ ] Descripción clara en cada endpoint
- [ ] Security schemes definidos
- [ ] Si es breaking: documentado explícitamente
- [ ] Referenciado en `docs/` si aplica
- [ ] `npm run validate` pasa
- [ ] Revisión manual: ¿Está claro para un cliente nuevo?

---

## 🚩 Red Flags (No Hacer Esto)

🚨 **NUNCA:**

- [ ] Cambiar tipo de dato de campo existente
- [ ] Remover campo sin deprecation
- [ ] Cambiar URL de endpoint
- [ ] Cambiar método HTTP (POST → GET)
- [ ] Inventar esquemas sin ser validados por clientes
- [ ] Poner ejemplos con datos reales
- [ ] Aplicar cambio sin actualizar CHANGELOG
- [ ] Cambiar versionado sin validación
- [ ] Mezclar múltiples cambios sin separar por versión
- [ ] No documentar impacto en clientes

---

## 🎯 Éxito Medido Por

- ✅ Spectral lint siempre pasa
- ✅ Clientes pueden regenerar tipos sin conflictos
- ✅ Cambios están documentados y ejemplificados
- ✅ CHANGELOG es claro y completo
- ✅ Versionado es predecible (semver)
- ✅ Nuevos endpoints adoptan convenciones
- ✅ Breaking changes son raros y bien comunicados
- ✅ Una sola verdad de comunicación entre repos

---

## 📚 Referencias

Ejemplos de OpenAPI:
- `examples/`

Documentación de arquitectura:
- `docs/boundaries.md`
- `docs/channels.md`

Reglas globales:
- `../UNIFIED_AGENT_CRITERIA.md`

Guías de referencia (obligatorias):
- https://weaverse.io/blogs/shopify-ai-toolkit-dev-mcp-hydrogen-2026
- https://www.fudge.ai/guides/shopify-ai-toolkit-claude-code-setup/

---

## 📝 Historia

| Fecha | Cambio |
|-------|--------|
| 2026-05-20 | Creación de AGENTS.md unificado para contratos |
