# Widget installation contract notes

Snippet recomendado:

```html
<script
  src="https://cdn.fluxbot.ai/chat-widget.v1.js"
  data-fluxbot-widget
  data-token="fbw_live_xxxxxxxxx"
  data-endpoint="https://api.fluxbot.ai/api/v1/widget/chat"
  data-api-version="v1"
  data-position="bottom-right"
  data-primary-color="#2563eb"
  data-greeting="Hola 👋 ¿En qué puedo ayudarte?"
  data-locale="es"
  data-tenant-mode="external"
  async>
</script>
```

## Alcance del snippet

El snippet solo configura el cliente embebible.  
No define ni sustituye comportamiento de backend.
