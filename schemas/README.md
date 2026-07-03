# DeDi Schemas

DeDi is schema-agnostic — a registry can define any record schema that suits its directory. The schemas here are **reference schemas** for the most common trust patterns, ready to use as-is or as a starting point. Each maps to one or more of the [use cases](../docs/use-cases.md).

| Schema | Directory it describes | Use case |
| :-- | :-- | :-- |
| [`public_key.json`](public_key.json) | Active + historical signing keys for an entity, with type/format and rotation history. | [Public key directory](../docs/use-cases.md#1-public-key-directory) |
| [`revoke.json`](revoke.json) | A revocation / negative list — revoked, blacklisted, or sanctioned identifiers with a reason. | [Revocation & negative lists](../docs/use-cases.md#2-revocation--negative-lists) |
| [`membership.json`](membership.json) | Affiliation of a person/entity to a body — club, consortium, licensor, or citizenship — with a validity window. | [Membership & affiliation](../docs/use-cases.md#3-membership--affiliation) |
| [`Beckn_subscriber.json`](Beckn_subscriber.json) | An open-network participant (BAP/BPP/BG/CDS): endpoints, domain, countries, and keys. | Open-network participant discovery |
| [`Beckn_subscriber_reference.json`](Beckn_subscriber_reference.json) | A pointer to subscriber records/registries held elsewhere, enabling federation across operators. | Open-network participant discovery |

## Using a schema

A registry is created with a schema; every record in that registry is validated against it. Records are then resolved uniformly:

```
GET https://<host>/dedi/lookup/{namespace}/{registry_name}/{record_name}
```

See the [API specification](../api/openapi.yaml) for the full Lookup, Query, and Versions endpoints.

## Contributing a schema

New reference schemas are welcome for trust patterns not yet covered (for example, policy/rule registries or AI-agent registries). Keep them minimal and composable — prefer pointing at other DeDi records (e.g. a `publicKey` field that references a key-directory entry) over duplicating data. Open an issue or PR to propose one.
