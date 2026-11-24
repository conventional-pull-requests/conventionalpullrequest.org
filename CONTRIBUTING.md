# Contributing

Thank you for helping improve Conventional Pull Requests. By participating you agree to follow the
[Code of Conduct](CODE_OF_CONDUCT.md).

## What to contribute

- **Changes to the specification:** open an issue first to discuss the change. Changes are made in
  `content/next/index.md`, never in a released version.
- **Typos and clarifications:** for wording that doesn't change the meaning, open a pull request
  directly. Fixes to a released version are accepted only when the meaning stays the same.
- **Translations:** see "Adding a translation" in the [README](README.md#adding-a-translation).
- **Tooling and projects:** add them to the lists in `content/about/index.md`.
- **Website:** changes to the theme in `themes/conventional-pull-requests/`.

## Workflow

We use [GitHub flow](https://docs.github.com/en/get-started/using-github/github-flow):

1. Fork the repository and create one branch per change from `main`.
2. Make the change and preview it with `hugo server`.
3. Open a pull request against `main`.

## Pull requests

Pull requests to this repository MUST follow the
[Conventional Pull Requests specification](content/v1.0.0-beta.1/index.md). In short:

- **Title:** a Conventional Commits header, for example `docs(spec): clarify footer separators`. CI
  checks it.
- **Description:** fill in the template. A `## Summary` section is required, and a `## Testing` section
  is expected when the site changes.
- **Size:** one logical change per pull request.
- **Merging:** pull requests are squash-merged, so the commits on your branch don't need to follow
  any convention. Only the title ends up on `main`.

Types used in this repository:

| Type | Use for |
| --- | --- |
| `feat` | New rules or sections in the specification, or new site features |
| `fix` | Corrections to the specification or bugs on the site |
| `docs` | README, CONTRIBUTING and other repository documentation |
| `i18n` | Translations |
| `style` | Theme styling with no content change |
| `ci` | GitHub Actions workflows |
| `chore` | Maintenance |

## Reviews

Reviewers are encouraged to use [Conventional Comments](https://conventionalcomments.org/), for
example `suggestion (non-blocking): ...`. Authors should reply to every comment and push changes as new
commits during review.

## License

By contributing, you agree that your contributions to the specification text (`content/`) are licensed
under [CC BY 4.0](LICENSE-CONTENT) and all other contributions under the [MIT License](LICENSE).
