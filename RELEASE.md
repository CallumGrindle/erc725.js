# Release Process

Releases are published to npm using [Release Please](https://github.com/googleapis/release-please).

This package automates CHANGELOG generation, version bumps and npm releases by parsing the
git history, looking for [Conventional Commit messages](https://www.conventionalcommits.org/).

When changes and feature PRs are merged from develop to main, `release-please` will open and maintain a release PR with the updated CHANGELOG and new version number. When this PR is merged, a release will be created and the package published to NPM.

## How should I write my commits?

Commits should follow the [Conventional Commit messages standard](https://www.conventionalcommits.org/).

The following commit prefixes will result in changes in the CHANGELOG:

- `fix:` which represents bug fixes, and correlates to a [SemVer](https://semver.org/)
  patch.
- `feat:` which represents a new feature, and correlates to a minor version increase.
  (indicated by the `!`) and will result in a SemVer major version increase.
- `feat!:`, or `fix!:`, `refactor!:`, etc., which represent a breaking change
- `build:` Changes that affect the build system or external dependencies.
- `ci:` Changes to our CI configuration files and scripts.
- `docs:` Documentation only changes.
- `perf:` A code change that improves performance.
- `refactor:` A code change that neither fixes a bug nor adds a feature.
- `style:` Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc).
- `test:` Adding missing tests or correcting existing tests.
- `chore:` Other

### What if my PR contains multiple fixes or features?

You can represent multiple changes in a single commit,
using footers:

```txt
feat: adds v4 UUID to crypto

This adds support for v4 UUIDs to the library.

fix(utils): unicode no longer throws exception
  PiperOrigin-RevId: 345559154
  BREAKING-CHANGE: encode method no longer throws.
  Source-Link: googleapis/googleapis@5e0dcb2

feat(utils): update encode to support unicode
  PiperOrigin-RevId: 345559182
  Source-Link: googleapis/googleapis@e5eef86
```

The above commit message will contain:

1. an entry for the **"adds v4 UUID to crypto"** feature.
2. an entry for the fix **"unicode no longer throws exception"**, along with a note
   that it's a breaking change.
3. an entry for the feature **"update encode to support unicode"**.

> :warning: **Important:** The additional messages must be added to the bottom of the commit.

## How do I change the version number?

When a commit to the main branch has `Release-As: x.x.x` (case insensitive) in the **commit body**, Release Please will open a new pull request for the specified version.

**Empty commit example:**

`git commit --allow-empty -m "chore: release 2.0.0" -m "Release-As: 2.0.0"` results in the following commit message:

```txt
chore: release 2.0.0

Release-As: 2.0.0
```

## How can I fix release notes?

If you have merged a pull request and would like to amend the commit message
used to generate the release notes for that commit, you can edit the body of
the merged pull requests and add a section like:

```
BEGIN_COMMIT_OVERRIDE
feat: add ability to override merged commit message

fix: another message
chore: a third message
END_COMMIT_OVERRIDE
```

The next time Release Please runs, it will use that override section as the
commit message instead of the merged commit message.
