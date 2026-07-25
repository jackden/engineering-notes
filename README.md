# Engineering Notes

This repository is the public publication layer for engineering notes about AI-assisted engineering, workflow governance, and reliability.

The repository keeps the publication unit readable without the website:

- `articles/<slug>/README.md` is the canonical article body.
- `articles/<slug>/metadata.yaml` is the article catalog record.
- `articles/<slug>/figures/` contains figures owned by that article.
- `articles/<slug>/article.pdf` is included only when a publication PDF has been supplied.
- `about/` describes the repository and its boundaries.
- `series/` contains series-level narrative structure.
- `references/` contains shared research references.
- `docs/article_index.yaml` is the source of truth for stable article IDs, series order, publication dates, and channel status.
- `assets/` is reserved for genuinely shared images, figures, and PDFs.

## Article loading contract

A site or static renderer should enumerate the immediate child directories of `articles/`, keep directories that contain both `README.md` and `metadata.yaml`, parse the metadata, and render `README.md` as the article body. The `figures` and `pdf` fields in `metadata.yaml` are relative to that article directory. A missing or null `pdf` means that no download link should be rendered.

The repository also includes a small, repository-native Jekyll layer for GitHub Pages. It provides navigation and article routes while keeping each `README.md` as the canonical article body.

Series maintenance documents live under `docs/`: the article index, series overview, publishing log, writing guidelines, and roadmap.

## Series articles

1. [Prompt Is Not a Workflow Engine](articles/prompt-is-not-a-workflow-engine/)
2. [Producing Work Is Not Completing Workflow](articles/producing-work-is-not-completing-workflow/)
3. [AI Can Produce Work, But What Counts as Evidence?](articles/ai-can-produce-work-but-what-counts-as-evidence/)
4. [The most overlooked problem in AI-assisted engineering](articles/the-most-overlooked-problem-in-ai-assisted-engineering/)
5. [Completion Needs Evidence, Not Confidence](articles/completion-needs-evidence-not-confidence/)
6. [A Healthy Workflow Sometimes Says No](articles/a-healthy-workflow-sometimes-says-no/)
7. [AI-Assisted Engineering Needs More Than Long Context](articles/ai-assisted-engineering-needs-more-than-long-context/) — Ruei Ming Deng, July 23, 2026

The series map is in [`series/ai-assisted-engineering-reliability.md`](series/ai-assisted-engineering-reliability.md).

## Public content boundary

The top-level `articles/` tree is the public publication model. Each article directory is self-contained: its README is the canonical body, its metadata is the catalog record, and its figures are article-owned assets.

Unknown publication dates remain null rather than being inferred. Platform provenance is represented separately from the public author identity where it is available.
