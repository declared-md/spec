# declared-md spec

The normative specification for the declared-md family of open standards.

This repository contains no code. It is the citable, copy-pastable source of truth for the three file formats. The validator, indexer, and directory site all derive from this.

---

## the three standards

| Standard | Subject | Spec | Schema | Example |
| --- | --- | --- | --- | --- |
| `whoami.md` | Individuals | [whoami.md](./whoami.md) | [whoami.schema.json](./schemas/whoami.schema.json) | [whoami.example.md](./examples/whoami.example.md) |
| `whois.md` | Organizations | [whois.md](./whois.md) | [whois.schema.json](./schemas/whois.schema.json) | [whois.example.md](./examples/whois.example.md) |
| `whatis.md` | Projects | [whatis.md](./whatis.md) | [whatis.schema.json](./schemas/whatis.schema.json) | [whatis.example.md](./examples/whatis.example.md) |

Rules that apply to all three standards are in [shared.md](./shared.md).

---

## quick start

Pick the standard that matches your subject, copy the corresponding example, and fill in your own data.

To validate your file before publishing:

```bash
npx declared-md validate ./whoami.md
```

The CLI is in the `declared-md/validator` repository.

---

## current version

**v1.0.0**, released 2026-04-26

See [CHANGELOG.md](./CHANGELOG.md) for version history.

---

## design principles

**the file is the source of truth.** The directory is a derived view. Subjects own their data. The indexer reads; it does not write to the subject's repo.

**three standards, not one.** People, organizations, and projects have different fields, different audiences, and different identity narratives. A unified schema would be bad for all three. The shared rules are in `shared.md`; everything else is specific to each standard.

**normative language follows RFC 2119.** MUST means required. SHOULD means strongly recommended. MAY means optional. When in doubt, the spec text is authoritative over any example.

**examples are not authoritative.** The examples in each spec document illustrate common usage. The normative rules are in the schema tables and validation sections. If an example and a schema rule conflict, the schema rule wins.

---

## repository structure

```
spec/
├── README.md          this file
├── shared.md          rules shared across all three standards
├── whoami.md          normative spec for individual identity profiles
├── whois.md           normative spec for organization identity profiles
├── whatis.md          normative spec for project identity profiles
├── CHANGELOG.md       version history
├── schemas/
│   ├── whoami.schema.json
│   ├── whois.schema.json
│   └── whatis.schema.json
└── examples/
    ├── whoami.example.md
    ├── whois.example.md
    └── whatis.example.md
```

---

## contributing

The spec is intended to be stable. Proposed changes should be submitted as issues first, not pull requests, unless they are clearly editorial (typos, wording clarifications that do not change meaning).

Breaking changes to required fields or validation rules require a major version bump. New optional fields require a minor version bump.

---

## license

The specification text is CC0. Use it, copy it, implement it, extend it, without restriction.
