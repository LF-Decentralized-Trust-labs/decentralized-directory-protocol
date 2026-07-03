# DeDi Use Cases

DeDi is deliberately unopinionated about *what* a directory contains — it standardizes only *how* any public directory is discovered, looked up, queried, and verified. That small surface covers a surprisingly large set of real-world trust problems.

This page collects the canonical patterns. Each one is a *source of truth* publishing a list, and one or more *relying parties* looking that list up in the middle of a transaction to answer a simple question: **can I trust this?**

## The common shape

Every use case below is the same three moves, expressed through DeDi's information model:

| Construct | Role | Example |
| :-- | :-- | :-- |
| **Namespace** | The publishing organization — a starting point for trust, anchored to a domain. | `ca.example` (a certificate authority) |
| **Registry** (Directory) | A schema-defined list the namespace maintains. | `signing-keys` |
| **Record** | A single entry in that list. | the key for `issuer-42` |

A relying party resolves a record with a single, uniform call:

```
GET https://<host>/dedi/lookup/{namespace}/{registry_name}/{record_name}
```

…and gets back a tamper-evident, provenance-tracked, versioned answer: a record carries a `digest` (tamper-evidence), a lifecycle `state` (`live`, `suspended`, `revoked`, `expired`), provenance (`created_by` and timestamps), and a `version`. Adding `?version_id=` or `?as_on=` retrieves a historical snapshot. Because the interface is the same for every directory, one integration reaches all of them — no per-registry connectors, no bespoke formats.

Each pattern maps to one of the three **trust pillars** (see the [Appendix in the README](../README.md#appendix-three-types-of-verification)): **Integrity** (unaltered), **Validity** (current, not revoked/expired), and **Authenticity** (genuinely from the claimed source).

## At a glance

| Use case | The question it answers | Publisher | Primary pillar | Schema |
| :-- | :-- | :-- | :-- | :-- |
| [Public key directory](#1-public-key-directory) | Is this the real signing key for this issuer — right now? | CAs, issuers, wallets | Authenticity | [`public_key.json`](../schemas/public_key.json) |
| [Revocation & negative lists](#2-revocation--negative-lists) | Has this credential / entity been revoked, sanctioned, or blacklisted? | Regulators, issuers, boards | Validity | [`revoke.json`](../schemas/revoke.json) |
| [Membership & affiliation](#3-membership--affiliation) | Is this party genuinely a member / accredited / in good standing? | Associations, consortia, licensors | Authenticity + Validity | [`membership.json`](../schemas/membership.json) |
| [Policy & rule registries](#4-policy--rule-registries) | What are the current, machine-readable rules I must apply? | Regulators, standards bodies | Validity | configurable |
| [AI agent registries](#5-ai-agent-registries) | Is this autonomous agent an approved, identifiable actor? | Enterprises, platforms | Authenticity | configurable |

---

## 1. Public key directory

**The problem.** To verify any signed document, credential, or message, a relying party needs the issuer's *current* public key — from an authentic source, not an attacker-in-the-middle. Today this means bespoke `.well-known` endpoints, out-of-band key exchange, or trusting whatever key is bundled with the artifact.

**How DeDi models it.** An issuer or certificate authority publishes a `signing-keys` registry under its namespace. Each record holds the active `publicKey`, its `keyType` and `keyFormat`, the owning `entity`, and — critically — a `previousKeys` history so that verifiers can still validate artifacts signed before a rotation.

**Verification flow.**
1. A verifier reads the issuer identifier from a credential (e.g. a DID or FQDN).
2. It calls the record lookup `/dedi/lookup/{namespace}/{registry_name}/{record_name}` — e.g. `/dedi/lookup/ca.example/signing-keys/issuer-42`.
3. It checks the returned key against the signature — offline-friendly, no round-trip to the issuer's own servers.
4. On rotation, the issuer updates the record; `previousKeys` and DeDi's version history keep old signatures verifiable.

**Why it matters.** Key discovery becomes a uniform lookup instead of N integrations, and rotation stops breaking downstream verification. This is the pattern behind the site's "DNS for trust" framing — a globally resolvable way to find the right key.

---

## 2. Revocation & negative lists

**The problem.** Integrity checks tell you a credential *was* validly issued; they say nothing about whether it has since been revoked, suspended, sanctioned, or blacklisted. Missing a revocation is how fraud and stale-authorization risk creep in.

**How DeDi models it.** The authority maintains a `revocations` (or `sanctions`, `no-fly`, `blacklist`) registry. Each record is a `revoked_id` plus an optional `reason`. Presence in the list is the signal; absence means "not revoked as of this version." DeDi also carries revocation at the record level — a record's lifecycle `state` can be `suspended`, `revoked`, or `expired`, with `valid_till` bounding its window — so a relying party can treat either a negative-list hit *or* a non-`live` state as grounds to refuse.

**Verification flow.**
1. Before honoring a credential or onboarding a counterparty, the relying party looks up the identifier in the relevant negative list.
2. A hit blocks or flags the transaction; the `reason` supports audit.
3. Because records are versioned and provenance-tracked, the relying party can prove *what the list said at the moment it acted* — essential for compliance disputes.

**Examples.** Revoked university credentials, suspended professional licenses, sanctioned entities, no-fly lists, blocked device or key identifiers.

**Note on real-time.** Negative lists are only useful if they are current. DeDi's live-update posture (and push notifications on hosted deployments) means a suspension propagates in near-real-time rather than on the next batch download.

---

## 3. Membership & affiliation

**The problem.** "Is this party actually a member of X?" — a licensed broker, an accredited lab, a bank in a settlement network, an entity in a consortium, a citizen of a jurisdiction. Verifying affiliation usually means trusting a PDF or calling the association by phone.

**How DeDi models it.** The membership body publishes a `members` registry. Each record carries the member's public `detail` (name, url, address, and optionally a `publicKey` pointer into a key directory), optional `evidence`, and a `memberSince` / `memberTill` validity window.

**Verification flow.**
1. A relying party looks up the counterparty in the body's `members` registry.
2. It checks the validity window (`memberSince` ≤ now ≤ `memberTill`), reinforced by the record's protocol-level `state` and `valid_till`.
3. If the record links a `publicKey`, the relying party can *chain* into a public key directory (use case 1) to cryptographically bind the affiliation to a signing identity.

**Why it matters.** Affiliation is where authenticity and validity meet — this is the "trust, but verify" pattern for consortium and network membership, and it composes naturally with key and revocation directories.

---

## 4. Policy & rule registries

**The problem.** Regulations and network rules change, but the systems that must obey them find out late, in prose, and implement them inconsistently. A platform operating across jurisdictions can't reliably auto-apply "the current rule."

**How DeDi models it.** A regulator or standards body publishes rules as records in a purpose-built registry (schema configurable to the rule domain — e.g. data-residency requirements, fee schedules, eligibility criteria). The record is the machine-parseable rule; version history is the changelog.

**Verification flow.**
1. A platform periodically resolves the policy registry (or watches its version history for changes).
2. It applies the current record programmatically rather than hard-coding a stale copy.
3. Version history + `as_on` lookups let it prove *which rule was in force* at the time of any past decision.

**Examples.** Data-residency rules a platform must enforce per country, machine-readable compliance requirements, network operating parameters, standardized code lists (country/currency/language) maintained by an authority.

---

## 5. AI agent registries

**The problem.** As autonomous agents begin to transact, relying parties need to know an agent is an *approved, identifiable* actor bound to an accountable operator — not an impersonator. This is the trust gap the README calls out for the agentic world.

**How DeDi models it.** An enterprise or platform publishes an `agents` registry. Each record identifies an approved agent, its operator, and a public key for authenticating the agent's actions (again, composable with use case 1). Revocation of a compromised agent is a negative-list entry (use case 2).

**Verification flow.**
1. A counterparty (human or machine) receiving an agent's request looks it up in the operator's `agents` registry.
2. It confirms the agent is listed, active, and bound to the expected operator and key.
3. It verifies the request signature against the registered key.

**Why it matters.** DeDi gives agents a discoverable, verifiable, revocable identity anchored to a real organization's domain — a prerequisite for letting agents act with authority while keeping a human/enterprise accountable.

---

## These patterns compose

The power isn't in any single directory — it's that they share one protocol, so they chain. A membership record points at a key directory; an agent authenticates against its registered key; a revocation list gates all of the above. A relying party that speaks DeDi once can traverse the whole graph.

## Running these in practice

DeDi is an open specification: anyone can implement these patterns on their own infrastructure, and existing registries can expose the DeDi API without changing their internal processes.

For organizations that would rather not run infrastructure, [**dedi.global**](https://dedi.global/) — operated by the Networks for Humanity Foundation — offers a freely hosted implementation of the protocol, provided as a public good with no licensing fees or usage caps. It is one way to publish and consume DeDi directories; the protocol itself is the standard, and it is deliberately implementation-neutral.

## Don't see your use case?

Most trust problems reduce to "a source of truth publishes a list; a relying party verifies against it." If yours fits that shape, it likely maps to one of the patterns above — often a small combination of them. Open an issue or discussion to propose a new schema or pattern.
