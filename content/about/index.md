---
title: About
type: about
draft: false
---

# About

Conventional Pull Requests applies the [Conventional Commits](https://www.conventionalcommits.org/)
specification, and the structure of its website, to pull requests. The rules in the specification were
not invented here. They are what the following widely used guidelines agree on:

- [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) and the
  [Angular commit message guidelines](https://github.com/angular/angular/blob/main/contributing-docs/commit-message-guidelines.md):
  the title format.
- [Google engineering practices](https://google.github.io/eng-practices/review/developer/): writing good
  change descriptions and keeping changes small.
- [GitHub documentation](https://docs.github.com/en/pull-requests): pull request templates, draft pull
  requests, closing keywords, co-author trailers and squash merge settings.
- [GitLab code review guidelines](https://docs.gitlab.com/development/code_review/): merge request size,
  self-review and review etiquette.
- [Kubernetes](https://www.kubernetes.dev/docs/guide/pull-requests/) and
  [Node.js](https://github.com/nodejs/node/blob/main/doc/contributing/pull-requests.md) pull request
  guides: title length, one change per pull request and passing CI before review.
- [The SmartBear study of code review at Cisco](https://smartbear.com/learn/code-review/best-practices-for-peer-code-review/):
  the recommended review size.
- [Chris Beams, "How to Write a Git Commit Message"](https://cbea.ms/git-commit/): imperative mood
  and title length.
- [Conventional Comments](https://conventionalcomments.org/): the companion convention for review
  comments.

Where these sources disagree, for example on whether the description in the title starts with a capital
letter, the specification leaves the choice to each project.

## Tooling for Conventional Pull Requests

- [action-semantic-pull-request](https://github.com/amannn/action-semantic-pull-request): a GitHub
  Action that validates PR titles against Conventional Commits.
- [commitlint](https://commitlint.js.org/): lint PR titles, or squashed commit messages, in CI.
- [release-please](https://github.com/googleapis/release-please) and
  [semantic-release](https://github.com/semantic-release/semantic-release): generate changelogs and
  version bumps from the squashed commits that conventional pull requests produce.
- [GitHub pull request templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository):
  give contributors the `Summary` and `Testing` sections to fill in. This repository's
  [template](https://github.com/conventional-pull-requests/conventionalpullrequest.org/blob/main/.github/pull_request_template.md)
  is a starting point.

## Projects Using Conventional Pull Requests

- [Conventional Pull Requests](https://github.com/conventional-pull-requests/conventionalpullrequest.org):
  this website.

Using the specification in your project? Open a pull request (a conventional one, please) to add it to
this list.
