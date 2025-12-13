# GuardAI Contracts

Source de vérité des contrats partagés (Events, payloads, versioning).

## Events
- v1: `events/v1/event.schema.json`
- exemples: `events/v1/examples/*.json`

## Règle de versioning
- Breaking change => nouvelle version (v2) + migration plan
- Les exemples doivent toujours passer dans les implémentations (backend, hub, ia, iot, frontend)
