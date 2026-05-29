# Breaking changes policy

Un cambio se considera breaking cuando:

- Se elimina un endpoint.
- Se elimina o renombra un campo existente.
- Se vuelve obligatorio un campo antes opcional.
- Se cambia el tipo de un campo.

Flujo recomendado:

1. Introducir versión nueva del contrato.
2. Adaptar backend.
3. Adaptar frontend.
4. Retirar versión anterior cuando ambos repos estén migrados.
