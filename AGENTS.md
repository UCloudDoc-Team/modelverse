# Repository Guidelines

## Project Structure & Module Organization
This repository is documentation-first. Content is organized by topic and rendered through Markdown navigation.

- `README.md`: landing page and top-level index.
- `_sidebar.md`: navigation tree; update this when adding or renaming docs.
- `api_doc/`: API references (`text_api/`, `image_api/`, `video_api/`, `audio_api/`, `common/`).
- `guide/`: platform usage guides.
- `best_practice/`: integration playbooks and scenario-based walkthroughs.
- `introduction/`, `price.md`, `private.md`: product overview, billing, and policy docs.
- `images/`: screenshots and static assets, grouped by feature.

## Build, Test, and Development Commands
There is no compiled build step in this repo; contributions are Markdown and assets.

- `rg --files`: quickly inspect tracked documentation files.
- `rg -n "pattern" api_doc guide best_practice`: verify terminology consistency.
- `git diff --name-only`: review changed files before commit.
- `markdownlint "**/*.md"` (if installed): optional style/lint pass.

## Coding Style & Naming Conventions
- Write clear, task-oriented Markdown with short sections and ordered steps.
- Preserve existing heading depth and list style in each document.
- Use fenced code blocks with language tags (for example, ```bash).
- Keep file names descriptive and consistent with current patterns:
  - model docs: lowercase with hyphens (for example, `gemini-media-analysis.md`)
  - vendor/model pages may keep canonical capitalization (for example, `OpenAI-Sora2-T2V.md`).
- Store new images under the closest feature folder in `images/` and use relative links.

## Testing Guidelines
This repo has no automated test suite. Validate changes manually:

- Ensure every new/renamed page is linked from `_sidebar.md`.
- Open modified Markdown and verify headings, tables, and code blocks render correctly.
- Check image paths and internal links are valid.
- For API examples, confirm endpoint paths and parameter names stay consistent across related docs.

## Commit & Pull Request Guidelines
Git history favors short, focused commits (Chinese or English), often imperative, such as `修复一个标头`, `fix docs`, `增加deepseek思考说明`.

- Keep each commit scoped to one documentation change.
- Recommended format: `<area>: <action>` (for example, `api_doc: update Gemini media analysis`).
- PRs should include: change summary, affected paths, screenshots for UI/doc rendering changes, and linked issue/ticket when applicable.
- Call out any navigation updates (`_sidebar.md`) in the PR description.
