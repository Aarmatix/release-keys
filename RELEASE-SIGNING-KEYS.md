# Aarmos release-signing keys

This file lets you independently confirm the public key that signs Aarmos CLI release
archives, and the checksum of the standalone release verifier, through a channel
separate from the download page.

Do **not** trust a public key that arrives alongside a downloaded archive. Corroborate
the values below against the verifier you obtained from `https://aarmos.io` before
relying on a verification result.

## Publisher signer (active)

- **Algorithm:** Ed25519
- **Fingerprint:** `f563cec5798943892b5fc9da`
- **Public key (raw 32-byte, base64url):** `6lpu2IzVHv4YgVC_6h42CsDXqMvEXe02naK9txvXC34`
- **Fingerprint derivation:** first 24 hex characters of SHA-256 over the raw key bytes.

## Release verifier

- **File:** `aarmos-release-verify.mjs` (from `https://aarmos.io` → CLI download page)
- **SHA-256:** `d34c91640bfa4bdd427a2407855567c2c087b2ebe1fb9308cbd818a32db93d0b`
- **Size:** 8,373 bytes
- **Requires:** Node.js 22 LTS or later (Windows, macOS, Linux).

## How to verify a downloaded CLI archive

1. Download the release archive and the verifier from the Aarmos CLI download page.
2. Check the verifier's own checksum (above) before running it.
3. Run: `node aarmos-release-verify.mjs <archive-file>`

The verifier checks the archive's SHA-256, verifies the detached signature against the
pinned signer key, and validates the signed artifact identity and version. It embeds
its trusted keys; it never accepts key material supplied with the download. Exit code
`0` means VERIFIED; `1` means NOT VERIFIED with a reason; `2` means a usage error.

## What the signature covers — and what it does not

The signature covers the archive bytes, package name, version, file name and checksum.
The source revision is **not** part of the signed material: the verifier records the
approved source revision as a separately pinned association, not a cryptographic
signature over the source.

## Scope

This covers **software release verification only**. It is not related to AVAR
governance-receipt verification, which is a separate mechanism.
