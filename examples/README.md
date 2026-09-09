# Examples

Illustrative, internally-consistent examples for [publishing-dedi-files.md](../docs/publishing-dedi-files.md).
Same publisher (`example.org`) and key (`key-1`) throughout.

| File | What it is |
|---|---|
| [`dedi.index.json`](dedi.index.json) | The publisher's `/.well-known/dedi.index.json` manifest — declares the current key, lists two referenced registries, embeds a third (`trust-anchors`) inline, and points to a digests file. **The authority.** |
| [`dedi.digests.json`](dedi.digests.json) | The digests file the manifest points to — current digest per registry, signed by the same key, re-issued on every content change so the manifest itself stays stable. |
| [`dedi.public-keys.json`](dedi.public-keys.json) | A positive directory: presence of a record = a valid key. |
| [`dedi.revocations.json`](dedi.revocations.json) | A negative list: presence of a record = revoked. Same shape; polarity comes from the registry, not a per-record field. |
| [`domains.txt`](domains.txt) | The discovery list — publisher domains, one per line, with an optional `# head:` last-changed marker. Vouches for nobody. |

Verify any DeDi file by: (1) checking its `proof` against its embedded `publisher.key`, then
(2) confirming that key appears in `keys` of `dedi.index.json`.

As hosted, these correspond to the recommended layout:

```
https://example.org/.well-known/dedi.index.json          ← manifest — fixed path (RFC 8615)
https://example.org/dedi/dedi.public-keys.json     ← files — RECOMMENDED convention
https://example.org/dedi/dedi.revocations.json
https://example.org/dedi/dedi.digests.json         ← digests file — optional
```

The `dedi.*.json` name and the `/dedi/` directory are conventions; nothing in verification or
discovery consults them — the manifest's `files[].url` is what locates a file.

The third registry, `trust-anchors`, has no hosted file: it is a complete DeDi file embedded
**inline** in the manifest's `files[]` (see the spec's Inline registries section). Its `source_url`
is the manifest's own well-known URL, and it carries no `digest` — the manifest's signature covers
its bytes directly.

> **Not cryptographically valid.** The `jws` and `digest` values are placeholders (marked
> `ILLUSTRATIVE_...` where applicable) to show *shape*, not real signatures. A real file carries a
> detached JWS over the JCS-canonicalized document, signed with the publisher's private key.

> **Temporary schema URLs.** The `schema` / `$id` URLs point at the repo's `main` branch, which is
> mutable. Before release they must be pinned to a version tag or commit SHA (see the Schemas section of the spec).
