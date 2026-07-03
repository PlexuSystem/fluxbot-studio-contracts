# Payment contract strategy

## Decision

- New billing plans should prefer Shopify App Pricing / App Events flows.
- Legacy billing stays in the contract only as compatibility for existing shops.
- Widget `connection-test` may return `paymentInfo` so the embedded widget can adapt messaging and checkout hints.

## Implications

- `billingMode` values are documented in the admin contract with preferred modes first.
- Legacy billing must not be treated as the default for new implementations.
- Widget consumers should treat `paymentInfo` as optional and nullable.
