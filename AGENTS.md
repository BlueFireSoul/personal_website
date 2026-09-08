# Project working guide

## Context

- This repository is Dongyang He's personal academic website.
- Live site: https://dongyanghe.netlify.app/ (provided by the owner).
- Git remote: https://github.com/BlueFireSoul/personal_website.git; existing branch: `main`.
- Stack: Hugo with the vendored Academic 4.8.0 theme, Markdown content, TOML configuration, and an R/blogdown workflow. This is not a Node application.
- These guidelines record repository conventions discovered during initial exploration; no pre-existing AGENTS.md was found. Keep them current as the project changes.

## Where to make changes

- `content/authors/admin/_index.md`: biography, role, education, and social links. `avatar.jpg` alongside it is the portrait.
- `content/home/`: homepage widgets. Active sections are `about.md`, `experience.md`, `working-papers.md`, `teaching.md`, and `contact.md`, ordered by `weight` (20, 40, 100, 120, 130).
- `content/publication/<slug>/index.md`: individual papers and their metadata, abstracts, and download links.
- `static/files/`: paper PDFs. The owner requested removal of the CV; keep CV links and PDFs off the site. A file here is served under `/files/`, without the `static/` prefix.
- `config.toml`: site title, relative base URL (`/`), theme selection, rendering, and taxonomies.
- `config/_default/params.toml`: appearance, site description, contact information, and theme features.
- `config/_default/menus.toml`: navigation. Homepage anchors must match widget filenames, e.g. `#working-papers`.
- `config/_default/languages.toml`: language configuration; English is enabled.
- `themes/hugo-academic/`: vendored theme code. Prefer site-level layout/asset overrides for customizations when supported; inspect the relevant theme implementation first.

## Editing conventions

- Preserve each file's existing front matter format: homepage widgets generally use TOML between `+++`; author and publication pages use YAML between `---`.
- Preserve the Academic widget schema, author identifier `admin`, and existing page slugs unless the requested change requires otherwise.
- The Working Papers widget selects publications with `publication_types: ["3"]`, sorted by descending date, with a five-item limit. Follow an existing publication page when adding a paper.
- Keep filename capitalization exact in links, especially PDFs; production hosting can distinguish uppercase and lowercase.
- Check all related locations when updating repeated information: profile, global description/contact settings, and menu links.
- Preserve academic claims, dates, and biographical details unless the user provides or requests factual updates. Existing content is not evidence of current personal status.
- Many inactive homepage widgets and example pages remain from the theme starter. A disabled widget does not necessarily disable the corresponding standalone pages. Do not activate or remove samples as a side effect of unrelated work.
- `public/`, `resources/_gen/`, and `.hugo_build.lock` are currently tracked. Treat generated HTML/assets as output, edit their sources, and avoid incidental regeneration in ordinary content changes. Do not change the repository's tracking policy incidentally.
- R Markdown sources are ignored by Hugo via `ignoreFiles`; use blogdown when rendering changes to `.Rmd` content rather than assuming Hugo will process it.

## Build and validation

- The existing `new.r` records `blogdown::serve_site()`, `blogdown::build_site()`, and `blogdown::stop_server()`. Run the appropriate command individually from the repository root rather than sourcing the entire script for a persistent preview.
- RStudio project: `job-market-website.Rproj`; root `index.Rmd` selects the blogdown site generator.
- The vendored theme README specifies Hugo **Extended 0.73–0.74**; its example Netlify configuration uses **0.74.3**. Treat this as historical compatibility evidence, not a verified production version. Do not blindly upgrade the legacy theme or Hugo.
- Once a compatible Hugo Extended binary is available, a basic preview is `hugo server --bind 127.0.0.1`. Use a temporary destination and resource directory for validation to avoid changing tracked generated files; see README.md.
- After content/layout edits, build if tooling is available, inspect affected pages, check navigation and PDF links, and review `git diff --check` plus `git status --short`. For visible design changes, inspect desktop and mobile layouts.
- No project test suite or root package.json was found. The theme's npm test command is only a failing placeholder; it is not a validation workflow.
- Initial environment check (2026-09-07): R 4.4.0 is installed; `blogdown` is not installed; `hugo` is not on PATH. A subsequent validation build succeeded using checksum-verified Hugo Extended 0.74.3 at `/private/tmp/personal-website-hugo/hugo`. It is temporary, not installed on PATH. Set `HUGO_RESOURCEDIR` for temporary resources; this version has no `--resourceDir` flag. Recheck tool availability before relying on this snapshot.

## Hosting and known issues

- Netlify is the existing host. Dashboard settings verified on 2026-09-08: project `dongyanghe`, site ID `b5cf93ad-a2eb-4969-8bb9-6d60cef3aaa4`, GitHub repository `BlueFireSoul/personal_website`, production branch `main`, base directory `/`, build command `hugo`, publish directory `public`, builds active. Root `netlify.toml` now pins Hugo Extended 0.74.3 and runs `hugo --cleanDestinationDir` to remove stale output. This fixes the missing Hugo error on Netlify’s current build image. No repository CI deployment workflow was found. The configuration in `themes/hugo-academic/netlify.toml` belongs to the upstream example site.
- Pushing to `main` triggers the existing Netlify production build. Verify the resulting deployment and live content after publishing. Recheck dashboard settings if this workflow changes.
- Existing `static/admin/config.yml` uses Git Gateway with branch `master`, while the current checkout tracks `main`. CMS operation has not been verified.
- The zoning paper is revise and resubmit at the Journal of Urban Economics (owner-confirmed). Keep it a working paper until its publication status changes. Its corrected PDF path is `files/He_JMP_zoning_2024.pdf`.
- The theme displays `publication` in paper details and list metadata; leave `publication_short` empty to show the full R&R status on the homepage.
- Owner-confirmed updates: PhD awarded in 2025; Google research data scientist since June 2026; contact email `hedongyang00@gmail.com`. Do not restore the removed phone number, street address, or CV.
