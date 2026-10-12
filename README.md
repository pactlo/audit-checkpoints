# Pactlo audit checkpoints

Once a day Pactlo publishes the latest block of its audit trail here: its
number, hash and Ed25519 signature. The files contain hashes only, never
personal data or document contents.

Why: Pactlo records every agreement event in hash-chained, signed blocks
(see the specification in [pactlo/verify](https://github.com/pactlo/verify)).
Publishing the chain's head in a public, append-only Git history means Pactlo
cannot show one history to one person and a different history to another.
Any evidence bundle's blocks must be consistent with the checkpoints here.

Layout: `<environment>/<yyyy>/<mm>/<dd>.json`, for example
`production/2027/01/15.json`:

```json
{"block_hash":"…","block_no":"…","created_at_micros":"…","environment":"production","key_id":"…","signature":"…","v":1}
```

- `block_hash`, `block_no`, `created_at_micros`: the block (specification §4).
- `signature`: Ed25519 over `"PACTLO-AUDIT-BLOCK-SIG-v1" || 0x00 || block_hash`,
  checkable with the keys at <https://pactlo.com/.well-known/pactlo-audit-keys.json>.
- Files are written once and never changed. `dev` checkpoints come from the
  development environment and are not production records.
