# AGENTS.md

Personal site built with Hugo and the PaperMod theme, deployed by GitHub Actions to GitHub Pages. There is no test suite. `hugo --minify` is the check that matters.

## Layout

- `hugo.yaml` is the only config: `baseURL`, menu, social links, homepage intro.
- `content/about.md` is `/about/`. It is an empty file with no front matter.
- `content/blogs.md` is `/blogs/` while `content/blogs/` has no pages. It is empty too.
- `content/home-intro.md` is the homepage bio (raw HTML, no front matter).
- `content/blogs/` is the blog section directory. It is empty and untracked until a post is added. Git does not store empty directories.
- `archetypes/default.md` is the template for new pages.
- `layouts/shortcodes/home-intro.html` is the only site layout. It prints the `home-intro` page into the homepage.
- `themes/PaperMod/` is a git submodule. Leave it untouched.
- `public/` is build output. It is gitignored.
- `.github/workflows/deploy.yml` builds and deploys.

There is no `static/` or `assets/` directory. PaperMod still links `/favicon.ico`, `/favicon-16x16.png`, `/favicon-32x32.png`, and `/apple-touch-icon.png`. Add those under `static/` or the requests 404.

## Commands

CI uses Hugo **extended 0.162.0**. The theme must be present or the build fails.

```bash
git submodule update --init
hugo server            # http://localhost:1313/ — drafts hidden
hugo server -D         # show drafts
hugo --minify          # production build, same as CI
hugo new content blogs/<slug>.md
```

PRs run the build only. A push to `main`, or a manual `workflow_dispatch`, uploads `public/` to GitHub Pages. `baseURL` is `https://nidhihemanth09.github.io/`.

## Content rules

`hugo new content blogs/<slug>.md` writes TOML front matter from the archetype: `draft = true`, `date` set to now, title taken from the filename. Production builds skip drafts. Publish by setting `draft = false`.

Give `about.md` a `title` when adding copy. Without one, the route renders with a blank page title.

`content/home-intro.md` is pulled in by `params.homeInfoParams.Content` (`{{< home-intro >}}`). With no front matter, Hugo also publishes `/home-intro/` and puts it in the sitemap. To keep it as source only:

```yaml
---
title: Home intro
build:
  render: never
  list: never
---
```

Leave it out of drafts. A draft is hidden from `site.GetPage`, so the homepage bio disappears.

`markup.goldmark.renderer.unsafe` is `true` so the intro can contain HTML. Keep that on. In the skill-chip block, put the `<span>`s on consecutive lines. A blank line makes Goldmark wrap each chip in `<p>`, and the flex row breaks.

`params.mainSections` is unset. Hugo then uses the section with the most regular pages for the home list, the archive, and next/prev links. One blog section means that section is `blogs`. Set `mainSections` once a second section should show up.

When `content/blogs/` gets its first page, Hugo turns `blogs` into a section and stops using `content/blogs.md` (the file body is dropped; the heading becomes "Blogs"). Replace that file with `content/blogs/_index.md` and set `title` there.

Menu entries are `languages.en.menu.main` in `hugo.yaml` (About weight 5, Blog weight 10).

Social `name` must match an icon in `themes/PaperMod/layouts/_partials/svg.html`. This site uses `github`, `linkedin`, `leetcode`, and `cv`. `title` is the tooltip (`Resume` on `cv`).

## Site changes

- Put overrides in the repo root. Layouts in `layouts/` replace theme layouts of the same path. Extra CSS goes in `assets/css/extended/` (PaperMod appends that glob). HTML in `<head>` or the footer goes in `layouts/_partials/extend_head.html` and `extend_footer.html`.
- Theme updates are a new submodule commit for `themes/PaperMod`, then commit the gitlink. CI checks the submodule out with `submodules: recursive`.
- `params.env` is `production`, so PaperMod takes its production path during `hugo server` as well (pages stay `index, follow`). `defaultTheme` is `auto`.
- Keep config in `hugo.yaml`.
- Do not commit `public/` or `.idea/`. `.hugo_build.lock` is tracked and empty; leave it as it is.
