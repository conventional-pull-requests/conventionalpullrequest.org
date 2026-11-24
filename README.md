# Conventional Pull Requests

[![Conventional Pull Requests](https://img.shields.io/badge/Conventional%20Pull%20Requests-1.0.0--beta.1-%234F46E5)](https://github.com/conventional-pull-requests/conventionalpullrequest)
[![Build](https://github.com/conventional-pull-requests/conventionalpullrequest/actions/workflows/deploy.yml/badge.svg)](https://github.com/conventional-pull-requests/conventionalpullrequest/actions/workflows/deploy.yml)

A specification for writing pull request titles and descriptions that people and machines can read.
It builds on [Conventional Commits](https://www.conventionalcommits.org/) and consolidates what
GitHub, GitLab, Google, Angular, Kubernetes, Node.js and code review research agree on.

```text
feat(search): add fuzzy matching to the product search

## Summary
Customers frequently misspell product names and get zero results. ...

## Testing
Added unit tests for the trigram scorer.

Closes #482
```

Read the full specification in [`content/v1.0.0-beta.1/index.md`](content/v1.0.0-beta.1/index.md),
or on the website.

## Repo layout

This repository follows the layout of
[conventionalcommits.org](https://github.com/conventional-commits/conventionalcommits.org):

| Path | Contents |
| --- | --- |
| `content/<version>/index.md` | One folder per released version of the specification. |
| `content/next/index.md` | The working draft. **All changes to the specification go here first.** |
| `content/about/index.md` | The About page: sources, tooling and users. |
| `config.yaml` | Site configuration, languages and the version list. |
| `themes/conventional-pull-requests/` | The Hugo theme: layouts, partials and plain CSS. It has no JavaScript and no asset build step. |
| `.github/` | Pull request and issue templates, CI and deployment workflows. |

## Running the site locally

Install [Hugo](https://gohugo.io/installation/) 0.139 or later (the extended edition is not required), then run:

```sh
hugo server
```

Or, with Docker: `docker compose up` (this also renders the `next` draft).

Open <http://localhost:1313/>. To build the static site into `public/`, run `hugo --minify`.

## Adding a translation

1. Create the file: `hugo new content <version>/index.<lang>.md`, e.g. `content/v1.0.0-beta.1/index.es.md`.
2. Translate the English text. Keep RFC 2119 keywords (MUST, SHOULD, ...) in English and uppercase.
3. Add the language under `languages:` in `config.yaml` with its `languageName`, `title` and `params`
   (`description`, `actions`, `versions`). Set `params.rtl: true` for right-to-left languages.

## Releasing a new version

1. Finalize `content/next/index.md`.
2. Copy it to `content/<new-version>/index.md` and set `draft: false`.
3. Add the new version to `versions.list` in `config.yaml`. The home page redirects to `versions.current`.
4. Set `versions.current` in `config.yaml`, and update the badge in this README.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Pull requests to this repository follow the specification
they define, and CI checks the title.

## Badge

Show that your project uses the specification:

```md
[![Conventional Pull Requests](https://img.shields.io/badge/Conventional%20Pull%20Requests-1.0.0--beta.1-%234F46E5)](https://github.com/conventional-pull-requests/conventionalpullrequest)
```

## License

- The specification text (`content/`) is licensed under
  [Creative Commons Attribution 4.0 International](LICENSE-CONTENT).
- The website code and theme are licensed under the [MIT License](LICENSE).

## Thank you

To the [Conventional Commits](https://www.conventionalcommits.org/) authors, whose specification
and website this project builds on, and to [Semantic Versioning](https://semver.org/), which inspired
them both.
