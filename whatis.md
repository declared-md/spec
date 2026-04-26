---
version: "1.0.0"
status: stable
date: 2026-04-26
---

# whatis.md: specification v1.0

The specification for project identity profiles in the declared-md family.

See [shared.md](./shared.md) for encoding, file location, filename, and common validation rules that apply to all three standards.

---

## purpose

`whatis.md` is a self-published identity file for open-source projects, protocols, libraries, applications, CLIs, and other software projects that maintain a public GitHub repository.

It answers one question: what is this project, in a format that is portable, machine-readable, and self-owned.

Unlike an `awesome-list` entry or a README tag, a `whatis.md` file is structured, versioned, and validated. Any directory that indexes it reads the same fields in the same format, regardless of which project it comes from.

---

## file location rules

A `whatis.md` file MUST live in one of the following locations (in priority order):

1. Root of the project's primary public repository
2. Root of a repository named `declared` that the project's GitHub owner controls
3. `.github/whatis.md` in any public repository the project's owner controls

The filename MUST be exactly `whatis.md`. See [shared.md](./shared.md) for the general filename rules.

---

## frontmatter schema

### required fields

These fields MUST be present and valid. A file missing any of these MUST fail validation.

| Field | Type | Rules |
| --- | --- | --- |
| `declared` | string | MUST be `"1.0"`. |
| `kind` | string | MUST be `"whatis"`. |
| `name` | string | Project name. MUST be at least 1 character. |
| `handle` | string | URL slug. MUST match `^[a-z0-9-]{2,39}$`. |

### recommended fields

These fields SHOULD be present. A file without them is valid but produces an incomplete profile.

| Field | Type | Rules |
| --- | --- | --- |
| `headline` | string | One-line project description. MUST be at most 120 characters. |
| `kind_of_project` | string | MUST be one of: `library`, `framework`, `application`, `cli`, `service`, `protocol`, `dataset`, `documentation`. |
| `links` | object | See links schema below. `links.repo` MUST be present if `links` is included. |

### optional fields

These fields MAY be present. All are optional. Validators MUST accept files that omit any or all of them.

| Field | Type | Rules |
| --- | --- | --- |
| `status` | string | MUST be one of: `experimental`, `alpha`, `beta`, `stable`, `mature`, `archived`. |
| `license` | string | SPDX license identifier. Examples: `MIT`, `Apache-2.0`, `AGPL-3.0`. |
| `language` | string[] | Primary programming languages. Free-form strings. |
| `stack` | string[] | Frameworks and major dependencies. Free-form strings. |
| `maintained_by` | string[] | GitHub handles of active maintainers. Free-form strings. |
| `sponsored_by` | string[] | GitHub handles or org names of project sponsors. Free-form strings. |
| `tags` | string[] | Additional free-form tags for discoverability. |

### links schema

The `links` field, if present, MUST be an object. All URL values MUST use the `https://` scheme. `links.repo` MUST be present when `links` is included.

| Key | Type | Description |
| --- | --- | --- |
| `repo` | string (URL) | Canonical repository URL. MUST point to a public repository. |
| `docs` | string (URL) | Documentation site URL. |
| `demo` | string (URL) | Live demo or playground URL. |
| `site` | string (URL) | Project website URL. |

No other keys are allowed in `links`.

---

## body rules

The body is the Markdown content after the frontmatter block.

- The body SHOULD be present. A file with only frontmatter is valid but produces a hollow profile.
- There is no minimum or maximum word count enforced by the validator.
- The body SHOULD describe the project in prose: what it does, who it is for, what problem it solves.
- The body MUST be valid Markdown.
- Headings, links, lists, and code blocks are all permitted.
- HTML is permitted but MAY be stripped by directory renderers.

---

## validation rules

In addition to the [common validation rules in shared.md](./shared.md#common-validation-rules), the validator MUST enforce:

- If `links` is present, `links.repo` MUST be present.
- `links.repo`, when present, MUST be a valid `https://` URL. The indexer SHOULD verify it resolves to a public GitHub repository.
- The `kind_of_project` field, if present, MUST be one of the enumerated values listed above.
- The `status` field, if present, MUST be one of the enumerated values listed above.
- The `license` field, if present, SHOULD be a valid SPDX identifier. The validator MAY warn on unrecognized values without failing.
- The `headline`, if present, MUST be at most 120 characters.

---

## full example

```markdown
---
declared: "1.0"
kind: whatis
name: Pagefind
handle: pagefind
headline: Static low-bandwidth search at scale
kind_of_project: library
status: stable
license: MIT
language: [rust, javascript]
stack: [wasm, nodejs]
maintained_by: [bglw]
tags: [search, static-sites, wasm, jamstack]
links:
  repo: https://github.com/CloudCannon/pagefind
  docs: https://pagefind.app
  demo: https://pagefind.app/docs/demo
---

Pagefind is a fully static search library that runs in the browser. It indexes your site
at build time and produces a compact binary index that the browser downloads and queries
locally.

There is no search server. There is no API key. There is no external service. The index
lives next to your HTML files and the JavaScript bundle downloads only the parts of the
index it needs for each query.

It works well with Astro, Eleventy, Hugo, and any other static site generator that
produces plain HTML output. The indexer is a single Rust binary distributed via npm.
Typical index sizes are under 100 KB for a 1,000-page site.
```

---

## future considerations

The following topics are explicitly out of scope for v1.0 and deferred to future versions:

- Cross-references between `whatis.md` and `whois.md` files (e.g., linking to the maintainer's org profile)
- A `deprecated_by` field pointing to a successor project
- Support for monorepo projects (one `whatis.md` per package)
- A `private: true` flag to publish without being indexed
- GitLab and Codeberg support (GitHub-only in v1.0)
- Signed or verified authorship
