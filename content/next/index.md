---
draft: true
---

# Conventional Pull Requests (next)

## Summary

The Conventional Pull Requests specification is a lightweight convention for writing pull request
titles and descriptions. It extends [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
from the individual commit to the unit of work that actually gets reviewed and merged: the pull request
(PR), also called a merge request (MR).

Most teams squash-merge pull requests, so **the PR title becomes the commit message on the main branch**.
If the title follows Conventional Commits, every Conventional Commits tool for changelogs, releases and
version bumps keeps working, even when the individual commits on the branch are messy. The description
gives reviewers the context they need to review the change quickly, and leaves a record for anyone who
reads the history later.

A conventional pull request looks like this:

---

```text
<type>[optional scope][!]: <description>

## Summary
<what changes and why>

## Testing
<how the change was verified>

[## Breaking Changes]
[optional additional sections]

[optional footer(s)]
```

---

A pull request has the following structural elements, which tell reviewers and tooling what to expect:

1. **Title:** a valid Conventional Commits header. With a squash merge it becomes the commit subject on
   the main branch.
1. **fix:** a PR of type `fix` patches a bug. This corresponds to [`PATCH`](https://semver.org/#summary)
   in Semantic Versioning.
1. **feat:** a PR of type `feat` adds a new feature. This corresponds to
   [`MINOR`](https://semver.org/#summary) in Semantic Versioning.
1. **BREAKING CHANGE:** a PR whose title has a `!` after the type or scope introduces a breaking API
   change. Its description has a `## Breaking Changes` section that explains the migration. This
   corresponds to [`MAJOR`](https://semver.org/#summary) in Semantic Versioning. A breaking change can be
   part of a PR of any type.
1. **Summary and Testing:** every description says what changes, why, and how the change was verified.
1. **Footers:** issues are linked with closing keywords (`Closes #123`) or non-closing references
   (`Refs #123`), in the same `Token: value` format as git trailers.
1. **Scope:** a conventional pull request contains one logical change, small enough to review in one
   sitting.

Types other than `fix:` and `feat:` are allowed. [@commitlint/config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional)
(based on the [Angular convention](https://github.com/angular/angular/blob/main/contributing-docs/commit-message-guidelines.md))
recommends `build:`, `chore:`, `ci:`, `docs:`, `style:`, `refactor:`, `perf:`, `test:`, and others.

## Examples

### Pull request for a new feature that closes an issue

```text
feat(search): add fuzzy matching to the product search

## Summary
Customers frequently misspell product names and get zero results. This adds
trigram-based fuzzy matching as a fallback when an exact search returns
nothing. Exact matches still rank first.

## Testing
- Added unit tests for the trigram scorer
- Ran the search benchmark: p95 latency 41 ms → 44 ms

Closes #482
```

### Pull request with `!` to draw attention to a breaking change

```text
feat(api)!: require an API version header on every request

## Summary
Requests without an `Api-Version` header were routed to whatever version
was newest, which broke clients silently on every release.

## Breaking Changes
Requests without an `Api-Version` header now get `400 Bad Request`.
To migrate, send `Api-Version: 2026-01-01` to keep the current behavior.

## Testing
Contract tests updated; staging soak test passed for 24 hours.

Closes #1290
```

### Pull request for a UI change with screenshots

```text
fix(checkout): keep the pay button visible on small screens

## Summary
On viewports narrower than 360 px the sticky footer covered the pay button.

## Screenshots
| Before | After |
| ------ | ----- |
| ![before](https://example.com/before.png) | ![after](https://example.com/after.png) |

## Testing
Checked in the device simulator at 320, 360 and 414 px wide.

Fixes #77
```

### Pull request with no testing section

```text
docs: correct the retry example in the README

## Summary
The example passed milliseconds to `retry()`, which takes seconds.
```

### Pull request with notes for reviewers and several footers

```text
refactor(billing): split invoice generation out of the order service

## Summary
The order service owns invoicing logic that billing needs to change
independently. This moves invoice generation into `billing/invoices`
with no behavior change.

## Testing
Existing integration suite passes unchanged; no new tests needed.

## Notes for Reviewers
Most of the diff is a file move. Start with `billing/invoices/service.ts`;
the remaining files only update imports.

Refs #301
Refs #305
Co-authored-by: Sam Rivera <sam@example.com>
```

### Draft pull request

A PR that is not ready for review is opened as a draft using the platform's draft feature. Its title
already follows the specification:

```text
perf(db): batch inserts in the import job
```

On a platform without drafts, the title is temporarily prefixed with `Draft:` and the prefix is removed
when the PR is ready:

```text
Draft: perf(db): batch inserts in the import job
```

### Revert pull request

```text
revert: feat(search): add fuzzy matching to the product search

## Summary
Fuzzy matching doubled CPU on the search cluster under peak load.
Reverting until the index-side implementation in #510 lands.

## Testing
Revert applied cleanly; search test suite passes.

Refs: 4f2a9c1
Refs #482
```

## Specification

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT",
"RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in
[RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

In this document, "pull request" also means a merge request or any equivalent request to merge a set
of changes after review.

### Title

1. A pull request title MUST be a valid [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/)
   header: a type, which consists of a noun such as `feat` or `fix`, followed by the OPTIONAL scope, the
   OPTIONAL `!`, and the REQUIRED terminal colon and space.
1. The type `feat` MUST be used when the pull request adds a new feature to the application or library.
1. The type `fix` MUST be used when the pull request fixes a bug.
1. Types other than `feat` and `fix` MAY be used. Projects SHOULD document the types they accept.
1. A scope MAY be provided after a type. A scope MUST consist of a noun describing a section of the
   codebase surrounded by parentheses, e.g., `fix(parser):`.
1. A description MUST immediately follow the colon and space after the type/scope prefix. The
   description is a short summary of the pull request as a whole, not of its last commit.
1. The description SHOULD be written in the imperative mood ("add", not "added" or "adds") and MUST NOT
   end with a period.
1. The whole title SHOULD be no longer than 72 characters. A title of 50 characters or fewer is
   RECOMMENDED.
1. The title MUST NOT contain issue tracker keys, status markers or tags (such as `[WIP]` or
   `JIRA-123`) inside the type, scope or description. References belong in the footers.
1. A pull request that introduces a breaking change MUST indicate it with a `!` immediately before the
   `:` in the title.

### Description

1. A pull request MUST have a description. The description is written in Markdown and organized into
   sections that start with level-two headings (`## `).
1. The description MUST contain a `Summary` section that explains what the pull request changes and
   why. The Summary SHOULD be understandable without following links.
1. The description SHOULD contain a `Testing` section that explains how the change was verified. It MAY
   be omitted when no verification applies, for example a typo fix in documentation.
1. When the title contains `!`, the description MUST contain a `Breaking Changes` section that describes
   what breaks and how consumers migrate.
1. A pull request that changes a user interface SHOULD include a `Screenshots` section with images or
   recordings of the change.
1. Additional sections MAY be used, for example `Notes for Reviewers`, `Checklist` or `Deployment`.
   Projects MAY provide a pull request template that defines additional sections.
1. Section headings MUST NOT be treated as case-sensitive by tooling.
1. The title and description MUST be kept accurate as the pull request changes, so that they describe
   the change that is merged.

### Footers

1. One or more footers MAY be provided at the end of the description, separated from the last section by
   one blank line.
1. Each footer MUST consist of a word token, followed by either a `:<space>` or `<space>#` separator,
   followed by a string value. This is inspired by the
   [git trailer convention](https://git-scm.com/docs/git-interpret-trailers).
1. A footer's token MUST use `-` in place of whitespace characters, e.g., `Co-authored-by`. An exception
   is made for `BREAKING CHANGE`, which MAY also be used as a token.
1. An issue that the pull request resolves MUST be referenced with a closing keyword footer (`Closes`,
   `Fixes` or `Resolves`), one footer per issue.
1. An issue or change that is related but not resolved SHOULD be referenced with a `Refs` footer.
1. Work by additional authors SHOULD be credited with `Co-authored-by: Name <email>` footers.
1. A `BREAKING CHANGE:` footer MAY repeat the breaking change description so that it is carried into
   the squashed commit body. If used, `BREAKING CHANGE` MUST be uppercase. `BREAKING-CHANGE` MUST be
   synonymous with `BREAKING CHANGE` when used as a token in a footer.

### Scope and lifecycle

1. A pull request SHOULD contain one logical change. Unrelated changes, including refactoring mixed with
   behavior changes, SHOULD be split into separate pull requests.
1. A pull request SHOULD be small enough to review in one sitting. Fewer than 400 changed lines,
   excluding generated files, lockfiles and mechanical changes, is RECOMMENDED. A larger pull request
   SHOULD explain in its description why it was not split.
1. A pull request that is not ready for review MUST be marked as a draft using the platform's draft
   feature. Where no draft feature exists, the title MAY be prefixed with `Draft: `, and tooling MUST
   ignore that prefix when validating the title.
1. The author SHOULD review their own diff and SHOULD ensure that automated checks pass before requesting
   review.
1. When a pull request is squash-merged, its title MUST be used as the subject of the resulting commit.
   Its Summary and footers MAY be used as the commit body.
1. The units of information that make up a conventional pull request MUST NOT be treated as
   case-sensitive by tooling, with the exception of `BREAKING CHANGE`, which MUST be uppercase.

## Why Use Conventional Pull Requests

- One title convention for the main branch, whatever the branch's own commits look like.
- Changelogs and semantic version bumps generated from merged pull requests.
- Faster reviews, because every description answers "what, why, and how was it tested" up front.
- A searchable history in which each merge explains its own reasoning.
- Small, focused pull requests, which are easier to review, revert and bisect.
- An easier start for new contributors, who can see what a good pull request looks like.

## FAQ

### How does this relate to Conventional Commits?

Conventional Pull Requests is built on [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/).
A conventional PR title is a Conventional Commits header, and the footer rules are the same. This
specification adds rules for the description, and for the size and lifecycle of the pull request, which
Conventional Commits does not cover.

### Do the individual commits in my pull request need to follow Conventional Commits?

No, not if the pull request is squash-merged. Only the title has to follow the specification, because
it becomes the commit on the main branch. If your project uses merge commits or rebase merges, every
commit lands on the main branch and each one SHOULD follow Conventional Commits.

### Which merge strategy should I use?

Squash merging with the PR title as the commit subject is the most common choice and the one this
specification is designed for. On GitHub, set "Default commit message" for squash merging to "Pull
request title" or "Pull request title and description".

### What do I do if my pull request fits more than one type?

Split it into several pull requests if you can. If it really is one change, choose the type with the
largest effect on users. `feat` and `fix` take precedence over the other types.

### Should the description in the title start with an uppercase or lowercase letter?

The type and scope are conventionally lowercase. Sources disagree on the description: the Angular and
Node.js guidelines use lowercase, while others capitalize. Either is allowed. Pick one for your project
and enforce it with tooling.

### Why the imperative mood?

The title describes what merging the pull request will do: "if applied, this pull request will *add
fuzzy matching*". This matches the messages git itself generates, such as "Merge branch" and "Revert".

### Is 400 lines a hard limit?

No. Code review research consistently finds that reviewers catch fewer defects as a change grows beyond
a few hundred lines. The [SmartBear study at Cisco](https://smartbear.com/learn/code-review/best-practices-for-peer-code-review/)
recommends 200–400 lines per review, and [Google's engineering practices](https://google.github.io/eng-practices/review/developer/small-cls.html)
call about 100 lines reasonable and 1,000 usually too large. The number is a signal to consider splitting,
not a gate.

### What do I do if I used the wrong type?

Before merging, edit the title. That is one of the advantages of putting the convention on the PR: the
title stays editable until the merge. After a squash merge, the commit is on the main branch, and the
remedies are the same as in Conventional Commits: rewrite history before a release if your project
allows it, or let the changelog miss the change.

### How should revert pull requests be handled?

Use the `revert` type and repeat the reverted title as the description, e.g.
`revert: feat(search): add fuzzy matching`. Reference the reverted commits with `Refs:` footers and
explain in the Summary why the revert was needed.

### Does this work for GitLab merge requests and other platforms?

Yes. Everything except the closing keywords and draft feature is platform-independent, and GitLab,
Gitea, Bitbucket and Azure DevOps all have equivalents of both. GitLab's own `Draft:` title prefix is the
same prefix this specification allows on platforms without a draft feature.

### How should reviewers write their comments?

This specification covers what the author writes. For review comments we recommend
[Conventional Comments](https://conventionalcomments.org/) (`suggestion (non-blocking): …`), which
makes the intent and urgency of each comment explicit.

### Should AI-assisted changes be disclosed?

Follow your project's policy. Where a project requires disclosure, a short note in the Summary or in
`Notes for Reviewers` is the RECOMMENDED place for it, because it reaches reviewers before they read the
diff.

### Do all my contributors need to use the Conventional Pull Requests specification?

No. Maintainers can fix the title and description before merging, in the same way that squash merging
lets them fix commit messages. Linting the title in CI catches most mistakes automatically.
