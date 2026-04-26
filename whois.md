---
version: "1.0.0"
status: stable
date: 2026-04-26
---

# whois.md: specification v1.0

The specification for organization identity profiles in the declared-md family.

See [shared.md](./shared.md) for encoding, file location, filename, and common validation rules that apply to all three standards.

---

## purpose

`whois.md` is a self-published identity file for organizations: companies, nonprofits, open-source collectives, communities, and other institutional subjects that maintain a public GitHub presence.

It answers one question: what is this organization, in a format that is portable, machine-readable, and self-owned.

Unlike Crunchbase or a LinkedIn company page, a `whois.md` file lives in the subject's own repository. The organization controls the data. Any directory that indexes it is a derived view, not the source.

---

## file location rules

A `whois.md` file MUST live in one of the following locations (in priority order):

1. Root of the organization's primary public repository
2. Root of a repository named `declared` under the organization's GitHub account
3. `.github/whois.md` in any public repository owned by the organization

"Primary public repository" is not enforced by the spec. The organization chooses which repo is primary. When multiple valid files exist under the same org, the indexer uses priority order.

The filename MUST be exactly `whois.md`. See [shared.md](./shared.md) for the general filename rules.

---

## frontmatter schema

### required fields

These fields MUST be present and valid. A file missing any of these MUST fail validation.

| Field | Type | Rules |
| --- | --- | --- |
| `declared` | string | MUST be `"1.0"`. |
| `kind` | string | MUST be `"whois"`. |
| `name` | string | Legal or display name of the organization. MUST be at least 1 character. |
| `handle` | string | URL slug. MUST match `^[a-z0-9-]{2,39}$`. |

### recommended fields

These fields SHOULD be present. A file without them is valid but produces an incomplete profile.

| Field | Type | Rules |
| --- | --- | --- |
| `headline` | string | One-line description of the organization. MUST be at most 120 characters. |
| `kind_of_org` | string | MUST be one of: `company`, `nonprofit`, `community`, `collective`, `gov`, `educational`. |
| `headquarters` | string | Free-form location of the organization's headquarters. |
| `links` | object | See links schema below. At least `site` or `github` MUST be present if `links` is included. |

### optional fields

These fields MAY be present. All are optional. Validators MUST accept files that omit any or all of them.

| Field | Type | Rules |
| --- | --- | --- |
| `founded` | string | Year of founding. MUST be a four-digit year string. Example: `"2022"`. |
| `size` | string | Organization headcount range. MUST be one of: `solo`, `2-10`, `11-50`, `51-200`, `201-500`, `501-1000`, `1000+`. |
| `industry` | string[] | Industry tags. Free-form strings. Examples: `fintech`, `developer-tools`, `health`. |
| `stack` | string[] | Publicly known technology stack. Free-form strings. |
| `hiring` | boolean | Whether the organization is currently hiring. MUST be `true` or `false`. |
| `remote_policy` | string | MUST be one of: `remote-first`, `hybrid`, `office-first`, `office-only`. |
| `funding_stage` | string | MUST be one of: `bootstrapped`, `pre-seed`, `seed`, `series-a`, `series-b-plus`, `public`, `acquired`. |
| `tags` | string[] | Additional free-form tags for discoverability. |

### links schema

The `links` field, if present, MUST be an object. All URL values MUST use the `https://` scheme. At least one of `site` or `github` MUST be present when `links` is included.

| Key | Type | Description |
| --- | --- | --- |
| `site` | string (URL) | Organization website. |
| `github` | string (URL) | GitHub organization URL. |
| `twitter` | string (URL) | Twitter or X profile URL. |
| `linkedin` | string (URL) | LinkedIn company page URL. |
| `careers` | string (URL) | Jobs or careers page URL. |

No other keys are allowed in `links`.

---

## body rules

The body is the Markdown content after the frontmatter block.

- The body SHOULD be present. A file with only frontmatter is valid but produces a hollow profile.
- There is no minimum or maximum word count enforced by the validator.
- The body SHOULD describe the organization in prose: what it does, who it serves, what it is building.
- The body MUST be valid Markdown.
- Headings, links, lists, and code blocks are all permitted.
- HTML is permitted but MAY be stripped by directory renderers.

---

## validation rules

In addition to the [common validation rules in shared.md](./shared.md#common-validation-rules), the validator MUST enforce:

- If `links` is present, at least one of `links.site` or `links.github` MUST be present.
- The `kind_of_org` field, if present, MUST be one of the enumerated values listed above.
- The `size` field, if present, MUST be one of the enumerated values listed above.
- The `remote_policy` field, if present, MUST be one of the enumerated values listed above.
- The `funding_stage` field, if present, MUST be one of the enumerated values listed above.
- The `founded` field, if present, MUST be a string matching `^\d{4}$`.
- The `hiring` field, if present, MUST be a boolean.
- The `headline`, if present, MUST be at most 120 characters.

---

## full example

```markdown
---
declared: "1.0"
kind: whois
name: Acme Robotics
handle: acme-robotics
headline: Industrial robotics for small manufacturers in Latin America
kind_of_org: company
headquarters: Curitiba, BR
founded: "2022"
size: 11-50
industry: [robotics, manufacturing, hardware]
stack: [rust, python, ros]
hiring: true
remote_policy: hybrid
funding_stage: seed
tags: [latam, b2b, hardware]
links:
  site: https://acme-robotics.com
  github: https://github.com/acme-robotics
  careers: https://acme-robotics.com/jobs
---

Acme Robotics builds affordable robotic arms for small and mid-size manufacturers in
Latin America. Established industrial robot vendors price their systems out of reach for
companies with fewer than 500 employees. We close that gap with hardware that is easier
to install, program, and maintain.

Our systems run on Rust firmware and Python control software with full ROS 2 compatibility.
All hardware schematics are published under a permissive license. We sell the machines
and support; the knowledge stays open.

We are currently hiring embedded engineers and manufacturing automation specialists.
Remote work is available for software roles; hardware roles are hybrid from Curitiba.
```

---

## future considerations

The following topics are explicitly out of scope for v1.0 and deferred to future versions:

- A `logo` field linking to a public image URL
- Cross-references between `whois.md` and `whoami.md` files (e.g., listing employees)
- A `verified` flag for organizations with confirmed domain ownership
- A `private: true` flag to publish without being indexed
- GitLab and Codeberg support (GitHub-only in v1.0)
- Localized organization names and descriptions
