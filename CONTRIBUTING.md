# Contributing

This repository holds the `Jaws-Platform` organization profile:
`profile/README.md` is what [github.com/Jaws-Platform](https://github.com/Jaws-Platform)
shows. It has no build step and deploys nothing. Coding agents should read
[AGENTS.md](AGENTS.md).

Because it is the organization's public `.github` repository, GitHub also
uses its community health files (this `CONTRIBUTING.md`, and the pull
request templates in `.github/`) as defaults for any organization repository
that has none of its own. Every Jaws repository ships its own, as the
conventions require.

## Local development setup

Requirements: Node.js 24, [pre-commit](https://pre-commit.com) and the GitHub CLI (`gh`).

```sh
git clone https://github.com/Jaws-Platform/.github.git jaws-platform-github
cd jaws-platform-github
npm ci                                         # commitlint
pre-commit install --hook-type commit-msg      # local commit-msg hook
```

The `commit-msg` hook runs commitlint locally. It is a convenience only;
**CI is the authoritative check**.

Run the CI checks locally before opening a pull request:

```sh
npm test
```

## Commits

Each commit message is a **single-line** Conventional Commit:

```
type(scope)?: subject
```

- At most 100 characters.
- Allowed types: `feat`, `fix`, `docs`, `chore`, `refactor`, `ci`, `build`,
  `perf`, `style`, `test`, `revert`.
- Breaking changes are marked with `!`, for example `feat!: drop a profile section`.
- The **only** content allowed after the header is a blank line followed by
  `Co-authored-by: Name <email>` trailers. No body text. Details belong in the
  pull request description.

commitlint enforces this (`@commitlint/config-conventional` plus the local
`only-coauthor-trailers` rule in `tools/commitlint/`). The fixtures in
`tests/commitlint/` show accepted and rejected messages; CI checks them.

## Pull requests

- After the initial scaffold commit, **every change reaches `main` through a
  pull request**. Direct pushes are blocked.
- Merges are **squash or rebase only**. No merge commits; history is linear.
- The **title must be a valid Conventional Commit**, because a squash merge
  uses it as the commit message.
- Use the typed template for your change and fill in every section:

  ```sh
  gh pr create --template feat.md      # or fix.md, docs.md, chore.md, refactor.md, ci.md
  ```

  In the browser, append `?template=<type>.md` to the compare URL. `fix.md`
  also asks for Bug, Root cause and Reproduction.

CI runs these jobs on every pull request, and all of them are required on
`main`:

| Job | Checks |
|---|---|
| `commitlint` | Every commit in the range `base..head`, so rebase-merged commits are all checked |
| `pr-title` | The title is a Conventional Commit and passes the commit header rules |
| `commitlint-rules` | Unit tests for the local rule, and the fixture messages |

## Releases and versioning

Releases are managed by [Release Please](https://github.com/googleapis/release-please).

- One version for the whole repository (`release-type: simple`), tagged
  `vX.Y.Z`. Release Please owns `CHANGELOG.md` and `version.txt`; nobody edits
  them by hand.
- On every push to `main`, the `release-please` workflow opens or updates a
  release pull request authored by `jaws-platform-release-bot[bot]`, a GitHub
  App owned by the `Jaws-Platform` organization (Contents and Pull requests,
  read and write). The repository secrets `RELEASE_BOT_CLIENT_ID` and
  `RELEASE_BOT_PRIVATE_KEY` are its credentials. CI runs on it like any other
  pull request.
- **Merging the Release Please pull request is the deliberate, human-reviewed
  act that releases a version.** Nobody creates tags by hand.
- The profile is live as soon as a change merges to `main`; a release only
  records it in the changelog.

| Change | Commit type | Bump |
|---|---|---|
| Removing a profile section or a default community health file | `feat!` / `fix!` | major (minor while `0.x`) |
| A new profile section or default file | `feat` | minor |
| A correction to the profile | `fix` | patch |
| `docs`, `chore`, `ci`, `refactor`, `test`, `style`, `build` | no release on their own |

## Repository settings

`scripts/apply-repo-settings.sh` applies the settings through the GitHub API:

- Squash and rebase merges only; squash commits use the pull request title and
  a blank body. Head branches are deleted after merge.
- A ruleset on `main` requires a pull request, squash or rebase merges, every
  CI job as a status check and linear history, and blocks force pushes and
  deletion, with no bypass actors.
