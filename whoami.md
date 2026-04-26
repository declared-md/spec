---
version: "1.0.0"
status: stable
date: 2026-04-26
---

# whoami.md: specification v1.0

The specification for individual identity profiles in the declared-md family.

See [shared.md](./shared.md) for encoding, file location, filename, and common validation rules that apply to all three standards.

---

## purpose

`whoami.md` is a self-published identity file for individuals: developers, designers, writers, and other people who maintain a public GitHub profile.

It answers one question: who are you, in a format that is portable, machine-readable, and self-owned.

Unlike a LinkedIn profile or a GitHub bio, a `whoami.md` file lives in the subject's own repository. The subject controls the data. Any directory that indexes it is a derived view, not the source.

---

## file location rules

A `whoami.md` file MUST live in one of the following locations (in priority order):

1. Root of the GitHub profile repository (`<username>/<username>`)
2. Root of a repository named `declared` owned by the user
3. `.github/whoami.md` in any public repository owned by the user

The filename MUST be exactly `whoami.md`. See [shared.md](./shared.md) for the general filename rules.

---

## frontmatter schema

### required fields

These fields MUST be present and valid. A file missing any of these MUST fail validation.

| Field | Type | Rules |
| --- | --- | --- |
| `declared` | string | MUST be `"1.0"`. |
| `kind` | string | MUST be `"whoami"`. |
| `name` | string | Display name. MUST be at least 1 character. |
| `handle` | string | URL slug. MUST match `^[a-z0-9-]{2,39}$`. |

### recommended fields

These fields SHOULD be present. A file without them is valid but produces an incomplete profile.

| Field | Type | Rules |
| --- | --- | --- |
| `headline` | string | One-line self-description. MUST be at most 120 characters. |
| `location` | string | Free-form location string. ISO country code at the end is recommended. |
| `links` | object | See links schema below. |

### optional fields

These fields MAY be present. All are optional. Validators MUST accept files that omit any or all of them.

| Field | Type | Rules |
| --- | --- | --- |
| `status` | string | MUST be one of: `open-to-work`, `working`, `building`, `learning`, `unavailable`. |
| `roles` | string[] | Professional roles. Free-form strings. Examples: `software-engineer`, `designer`, `pm`, `founder`. |
| `stack` | string[] | Technologies, languages, frameworks. Free-form strings. |
| `focus` | string[] | Topics or domains the person currently works on. Free-form strings. |
| `languages` | string[] | Spoken or written languages. ISO 639-1 codes are recommended. |
| `available_for` | string[] | Types of engagements the person is open to. Examples: `freelance`, `consulting`, `hiring`, `partnership`, `mentorship`. |
| `tags` | string[] | Additional free-form tags for discoverability. |
| `pronouns` | string | Free-form. |
| `timezone` | string | IANA timezone identifier. Example: `America/Sao_Paulo`. |

### links schema

The `links` field, if present, MUST be an object. All URL values MUST use the `https://` scheme.

| Key | Type | Description |
| --- | --- | --- |
| `github` | string (URL) | GitHub profile URL. |
| `twitter` | string (URL) | Twitter or X profile URL. |
| `linkedin` | string (URL) | LinkedIn profile URL. |
| `bluesky` | string (URL) | Bluesky profile URL. |
| `mastodon` | string (URL) | Mastodon profile URL. |
| `site` | string (URL) | Personal website. |
| `email` | string (email) | Contact email address. |

No other keys are allowed in `links`.

---

## body rules

The body is the Markdown content after the frontmatter block.

- The body SHOULD be present. A file with only frontmatter is valid but produces a hollow profile.
- There is no minimum or maximum word count enforced by the validator.
- The body SHOULD describe the person in prose: who they are, what they are building or working on, how to reach them.
- The body MUST be valid Markdown.
- Headings, links, lists, and code blocks are all permitted.
- HTML is permitted but MAY be stripped by directory renderers.

---

## validation rules

In addition to the [common validation rules in shared.md](./shared.md#common-validation-rules), the validator MUST enforce:

- If `links.github` is present, it MUST point to a URL on `github.com`. The validator SHOULD verify it matches the GitHub username of the repository where the file lives.
- The `status` field, if present, MUST be one of the enumerated values listed above.
- The `headline`, if present, MUST be at most 120 characters.

---

## full example

```markdown
---
declared: "1.0"
kind: whoami
name: Ana Ferreira
handle: ana-ferreira
headline: Full-stack developer focused on sustainable supply chain software
location: Recife, BR
status: open-to-work
roles: [software-engineer, technical-writer]
stack: [typescript, react, node, postgresql]
focus: [supply-chain, open-source, developer-tooling]
languages: [pt-BR, en]
available_for: [freelance, consulting, mentorship]
tags: [brazil, northeast, b2b]
pronouns: she/her
timezone: America/Recife
links:
  github: https://github.com/ana-ferreira
  site: https://anaferreira.dev
  email: ana@anaferreira.dev
---

I'm a full-stack developer based in Recife, Brazil, with seven years of experience building
internal tools and APIs for logistics companies.

My current focus is sustainable supply chain software: tools that help small distributors
track emissions, reduce waste, and report to regulators without drowning in spreadsheets.
Most of my open-source work is on the data layer, though I've been contributing more to
frontend tooling lately.

I write technical articles in both Portuguese and English. If you are working on something
at the intersection of climate, logistics, or developer infrastructure, I'd like to hear
about it.
```

---

## future considerations

The following topics are explicitly out of scope for v1.0 and deferred to future versions:

- A `photo` field linking to an avatar URL
- Cross-references to `whois.md` files (e.g., linking to an employer's org profile)
- Support for multiple spoken language bodies (same frontmatter, multiple prose sections in different languages)
- A `signature` field for cryptographic proof of authorship
- GitLab and Codeberg support (GitHub-only in v1.0)
- A `private: true` flag to publish a valid file without being indexed
