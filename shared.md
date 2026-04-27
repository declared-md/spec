---
version: "1.0.0"
status: stable
date: 2026-04-26
---

# shared rules

This document defines the rules that apply to all three standards in the declared-md family: `whoami.md`, `whois.md`, and `whatis.md`. Individual spec documents reference this document instead of repeating these rules.

---

## encoding and format

All declared-md files MUST be:

- UTF-8 encoded
- Valid Markdown with valid YAML frontmatter
- At most 50 KB in total file size

---

## file locations

A declared-md file MUST live in one of the following three locations in a public GitHub repository:

| Priority | Location | Description |
| --- | --- | --- |
| 1 | Root of the canonical repo | See per-standard rules for what "canonical" means |
| 2 | Root of a repo named `declared` | Any public repo the subject owns or controls |
| 3 | `.github/<filename>` | Inside the `.github` directory of any public repo |

When a subject publishes the same standard in multiple valid locations, the indexer uses priority order: location 1 wins over location 2, location 2 wins over location 3.

### what "public GitHub repository" means

The repository MUST be:

- Hosted on `github.com`
- Accessible without authentication (public visibility)
- Owned by the same user or organization as the declared subject

---

## filename rules

Each standard has exactly one canonical filename. No aliases, no capitalization variants, no `.yaml` extensions.

| Standard | Canonical filename |
| --- | --- |
| Individual identity | `whoami.md` |
| Organization identity | `whois.md` |
| Project identity | `whatis.md` |

A file named `WHOAMI.md`, `WhoAmI.md`, or `whoami.yaml` MUST NOT be accepted by the validator or indexer.

---

## required frontmatter fields

All three standards share the following required fields:

| Field | Type | Description |
| --- | --- | --- |
| `declared` | string | Spec version. MUST be `"1.0"` for this version. |
| `kind` | string | Identifies which standard this file implements. |
| `name` | string | Display name of the subject. MUST be at least 1 character. |
| `handle` | string | URL slug for the subject. See handle rules below. |

### the `declared` field

The `declared` field declares which version of the spec the file targets. Validators MUST reject files with an unknown or missing `declared` value.

The value MUST be a string: `"1.0"`. Note the quotes. An unquoted `1.0` is a float in YAML and MUST be rejected.

### the `kind` field

The `kind` field MUST exactly match the standard the file is intended to implement:

| File | Required value |
| --- | --- |
| `whoami.md` | `whoami` |
| `whois.md` | `whois` |
| `whatis.md` | `whatis` |

A `whoami.md` file with `kind: whois` in its frontmatter MUST fail validation.

### the `handle` field

The `handle` field is the canonical URL slug for the subject. It MUST:

- Match the regular expression `^[a-z0-9-]{2,39}$`
- Be lowercase only
- Contain only alphanumeric characters and hyphens
- Be at least 2 characters long
- Be at most 39 characters long
- Not start or end with a hyphen

Examples of valid handles: `treblavg`, `declared-md`, `my-project`.

Examples of invalid handles: `T`, `my_project`, `-starts-with-hyphen`, `ends-with-hyphen-`.

---

## links object

All three standards use a `links` object for external URLs. The following rules apply to all links:

- The `links` object accepts any key. The set of keys is not closed.
- All values MUST use the `https://` scheme, except the `email` key which MUST use the `mailto:` scheme (e.g. `mailto:user@example.com`).
- Plain `http://` URLs MUST be rejected.
- Well-known link keys (e.g. `github`, `twitter`, `site`, `repo`) are documented in each standard's spec for discoverability, but any key that satisfies the URL rules is accepted.

---

## common validation rules

The validator MUST enforce the following for every declared-md file regardless of standard:

1. The file is valid UTF-8.
2. The frontmatter parses as valid YAML.
3. All four required fields are present: `declared`, `kind`, `name`, `handle`.
4. The `declared` value is a known spec version string.
5. The `kind` value matches the standard for the filename being validated.
6. The `handle` value matches `^[a-z0-9-]{2,39}$`.
7. The total file size is at most 50 KB.
8. All URLs in `links` use the `https://` scheme.

Failures in any of these rules MUST result in the file being marked invalid. Partial profiles are not accepted.

---

## versioning policy

The declared-md spec follows semantic versioning at the family level, not per standard.

- Patch releases (1.0.x): corrections to wording, no schema changes.
- Minor releases (1.x.0): new optional fields, backward-compatible.
- Major releases (x.0.0): breaking changes to required fields or validation rules.

All three standards share the same version number. A file that is valid against v1.0 of `whoami.md` MUST remain valid in any v1.x release.

---

## what shared.md does not cover

The following topics are intentionally left to the individual spec documents:

- Which specific fields are required, recommended, or optional for each standard
- Per-standard file location rules (e.g., what "canonical repo" means for each standard)
- Per-standard validation rules
- Field-level descriptions, examples, and enumerations
