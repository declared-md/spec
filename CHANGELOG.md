# changelog

All notable changes to the declared-md spec are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/). This spec follows [semantic versioning](https://semver.org/).

---

## [1.0.1] - 2026-04-26

### changed

- `links` object in all three schemas now accepts arbitrary keys. Previously restricted to a fixed set of known keys. Any key whose value is a valid `https://` URL is accepted. The `email` key is the only exception: it accepts a `mailto:` URI instead of an HTTPS URL.
- `links.github` in `whoami.schema.json` now validates that the URL points to `github.com` (pattern `^https://github\.com/.+`), not just any HTTPS URL.
- Removed `format: "uri"` annotations from link URL fields; validation is now done via `pattern: "^https://"` through `additionalProperties`.
- `whois.schema.json`: added `if/then` rule enforcing that at least one of `site` or `github` must be present when `links` is included.

---

## [1.0.0] - 2026-04-26

### added

- `whoami.md` v1.0: normative specification for individual identity profiles
- `whois.md` v1.0: normative specification for organization identity profiles
- `whatis.md` v1.0: normative specification for project identity profiles
- `shared.md`: rules shared across all three standards (encoding, file locations, filename rules, common required fields, common validation rules, versioning policy)
- JSON Schema draft-07 for each standard (`schemas/whoami.schema.json`, `schemas/whois.schema.json`, `schemas/whatis.schema.json`)
- Full example files for each standard (`examples/whoami.example.md`, `examples/whois.example.md`, `examples/whatis.example.md`)
