# ACP-PUBLIC Versioning

**Document ID:** `acp_public_versioning`
**Version:** `v1.0.0-public-draft`
**Status:** `Research Draft`
**Class:** `Release / Publication File`
**Authority:** `Controlled public distribution draft`
**Anchors:** `acp_public_distribution_profile`

---

## Purpose

This document defines how `ACP-PUBLIC` package versions should be interpreted.

---

## Version Layers

`ACP-PUBLIC` has two version notions:

1. **source protocol version**
2. **public package version**

The source protocol version identifies the ACP authority line from which the
package was derived.

The public package version identifies the released consumer-facing package state.

---

## Current Package Version

Current package version:

- `1.0.0-public-draft`

Current source protocol line:

- `ACP 1.0.0`

---

## Interpretation Rule

When the package is named:

- `ACP-PUBLIC v1.0.0`

it means:

- this package is aligned to the ACP 1.0.0 authority line
- it is not a claim that the package itself is the protocol maintenance source
- it is a consumer-facing distribution built from that authority line

---

## Future Versioning Rule

Recommended package progression:

- `1.0.0-public-draft`
- `1.0.0-public-beta`
- `1.0.0-public`

If the public package changes materially while remaining aligned to ACP
1.0.0, the suffix should change before the major protocol line changes.

If the package is rebuilt from a later ACP authority line, the ACP-PUBLIC base
version should advance with that authority line.
