# EHR Intent Detection

mdBook on how a local LLM + deterministic validator + mapping layer answers questions over the Estonian building register (EHR).

Published at **https://turnerrainer.github.io/ehr-intent-detection/**.

## Local preview

```bash
cargo install mdbook mdbook-mermaid   # one-time
mdbook serve --open
```

## Layout

- `src/` — chapter sources (markdown + mermaid diagrams).
- `book.toml` — mdBook config.
- `.github/workflows/book.yml` — builds on push to `main` and deploys to GitHub Pages.
