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

Pagefind is a fully static search library that runs in the browser. It indexes your site at build time and produces a compact binary index that the browser downloads and queries locally.

There is no search server. There is no API key. There is no external service. The index lives next to your HTML files and the JavaScript bundle downloads only the parts of the index it needs for each query.

It works well with Astro, Eleventy, Hugo, and any other static site generator that produces plain HTML output. The indexer is a single Rust binary distributed via npm. Typical index sizes are under 100 KB for a 1,000-page site.
