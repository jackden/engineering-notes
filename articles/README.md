# Article directory

Each child directory is one publication unit.

```text
articles/<slug>/
├── README.md       # canonical Markdown publication
├── metadata.yaml   # catalog and publication metadata
├── article.pdf     # optional; only when supplied
└── figures/        # article-owned figures and figure notes
```

Keep article prose in `README.md`. Add or update publication metadata in `metadata.yaml`; do not add a second application-owned copy of the article body. Use relative paths in `figures` and `pdf` so the same tree can be consumed by GitHub, GitHub Pages, GPT Sites, or another static renderer.

Stable series IDs (`A01`, `A02`, …), publication dates, channel status, and series-level summaries are maintained in [`../docs/article_index.yaml`](../docs/article_index.yaml). The neighboring article metadata repeats the ID and series position for local discoverability.
