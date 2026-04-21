# GuardAI Contracts

Source de vérité des contrats partagés entre les services Guard-AI (events, commands, payloads, versioning).

## Ressources

### Events (télémétrie Hub → backend)

- Schéma : `guard-ai-contracts/events/v1/event.schema.json`
- Exemples : `guard-ai-contracts/events/v1/examples/*.json`

### Commands (backend → Hub)

- Schéma : `guard-ai-contracts/commands/v1/command.schema.json`
- Exemples : `guard-ai-contracts/commands/v1/examples/*.json`

## Types d'events canoniques

Le champ `type` reste un `string` libre dans le schéma (pour éviter les bumps de version à chaque nouvel event). La liste ci-dessous est la référence — tout ajout doit passer par une PR sur ce repo.

| Source    | Type                   | Sévérité attendue    | Émetteur    |
|-----------|------------------------|----------------------|-------------|
| `sensor`  | `door_opened`          | `critical`           | IoT sensor  |
| `sensor`  | `motion_detected`      | `info` / `warning`   | IoT sensor  |
| `network` | `new_device_detected`  | `warning`            | Hub réseau  |
| `network` | `device_connected`     | `info`               | Hub réseau  |
| `network` | `device_disconnected`  | `info`               | Hub réseau  |
| `network` | `port_scan_detected`   | `critical`           | Hub réseau  |
| `network` | `suricata_alert`       | `warning` / `critical` | Hub réseau |
| `network` | `firewall_drop_burst`  | `warning`            | Hub réseau  |
| `camera`  | `face_recognized`      | `info`               | IA          |
| `camera`  | `face_unknown`         | `warning`            | IA          |
| `system`  | (divers)               | *                    | Backend     |

## Types de commands canoniques

| Type              | Payload attendu                                    | Effet côté Hub                                 |
|-------------------|----------------------------------------------------|------------------------------------------------|
| `scan_network`    | `{ subnet, mode }`                                 | Lance `nmap` sur le sous-réseau                |
| `block_device`    | `{ mac_address, reason?, ttl_seconds? }`           | Ajoute règle iptables DROP, auto-expire si TTL |
| `kick_device`     | `{ mac_address }`                                  | `hostapd_cli disassociate`                     |
| `unblock_device`  | `{ mac_address }`                                  | Supprime la règle iptables associée            |

## Règles de versioning

- Les champs **optionnels ajoutés** restent v1 (rétro-compatible).
- Un changement **breaking** (rename, suppression, passage optionnel → required) impose une nouvelle version (`v2`) et un plan de migration.
- Chaque exemple JSON doit valider contre son schéma — la CI `schema-validate` (voir `.github/workflows/`) le vérifie sur chaque PR.

## Consommateurs

| Repo                   | Mode de consommation                                     |
|------------------------|----------------------------------------------------------|
| `guard-ai-backend`     | Git submodule à `contracts/` (Rust charge les exemples)  |
| `guard-ai-ia`          | Git submodule à `guard-ai-contracts/` (Python jsonschema) |
| `guard-ai-frontend`    | Types TypeScript dérivés à la main dans `src/lib/types/` |
| `guard-ai-iot` (hub)   | Git submodule à `guard-ai-contracts/` (Python jsonschema) |

Après un bump v1.x côté contracts, chaque consumer fait `git submodule update --remote guard-ai-contracts` pour récupérer les nouveaux champs.
