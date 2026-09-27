# rohan roots

Personal blog. Notes on AI agents, self-hosting, and infra.

## Develop

```sh
npm install
npm run dev
```

## Build

```sh
npm run build
```

Static output lands in `dist/`. Deployed on Cloudflare Pages from the `main` branch.

## Writing

Posts live in `src/content/blog/` as Markdown files with frontmatter:

```yaml
---
title: "Post title"
description: "One-line summary."
pubDate: 2026-09-21
canonical: "https://rohanroots.substack.com"  # optional
---
```
