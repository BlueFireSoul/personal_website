# Dongyang He's personal website

Live website: [dongyanghe.netlify.app](https://dongyanghe.netlify.app/)

This is a Hugo site using the bundled Academic 4.8.0 theme, with an RStudio/blogdown workflow. Project-specific editing rules and known issues are in [AGENTS.md](AGENTS.md).

## Common edits

| Content | Source |
| --- | --- |
| Biography, education, profile links | `content/authors/admin/_index.md` |
| Homepage sections and teaching | `content/home/` |
| Working papers | `content/publication/` |
| Downloadable papers | `static/files/` |
| Navigation | `config/_default/menus.toml` |
| Appearance, description, contact details | `config/_default/params.toml` |
| Hugo configuration | `config.toml` |

## Local preview

Use a Hugo Extended version compatible with the legacy theme. The bundled theme README specifies versions 0.73–0.74, and its example configuration pins 0.74.3; the production build version is not recorded at the repository root.

From the repository root, with Hugo available:

```sh
hugo server --bind 127.0.0.1
```

Alternatively, open `job-market-website.Rproj` in RStudio and, with blogdown installed, run these individually as needed:

```r
blogdown::serve_site()
blogdown::build_site()
blogdown::stop_server()
```

The existing `new.r` contains the same workflow. Use blogdown for R Markdown changes; Hugo does not render `.Rmd` files directly in this project.

## Validate without replacing tracked output

The repository tracks `public/` and `resources/_gen/`. Once a compatible Hugo binary is installed, use temporary output directories for a validation build:

```sh
site_check_dir=$(mktemp -d "${TMPDIR:-/tmp}/personal-website-check.XXXXXX")
HUGO_RESOURCEDIR="$site_check_dir/resources" hugo --destination "$site_check_dir/public"
git diff --check
git status --short
```

Inspect the generated pages and verify changed links, including exact PDF filename capitalization. No automated project test suite is configured.

Validation on 2026-09-07 succeeded with Hugo Extended 0.74.3 downloaded to `/private/tmp/personal-website-hugo/hugo` and verified against the release checksum. It is a temporary build tool, not installed on PATH. R 4.4.0 is installed; blogdown is not. Use `HUGO_RESOURCEDIR` to redirect generated resources with this Hugo version; it does not support a `--resourceDir` flag.

## Hosting

Netlify deploys this repository from `main`, runs `hugo` at the repository root, and publishes `public/` (dashboard verified 2026-09-08). Push approved updates to `main`, then check the production deployment and live pages. Root `netlify.toml` pins Hugo Extended 0.74.3 and runs `hugo --cleanDestinationDir`; the clean flag prevents stale generated files from surviving a deployment. The configuration inside the theme directory is an upstream example.
