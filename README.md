# Monorepo release workflow

In this repository, I want to test a monorepo release workflow I have in mind for a project I'm working on.

## The idea

> Edit: I try release-please for now. It seems to be a good fit for my needs.

Whenever a change is pushed to the `main` branch or a pull request is merged, if the `package.json` file of any package in the monorepo contains a version change, the following workflow will be triggered:

1. **Detect Version Changes**:
   - The workflow will detect if any `package.json` files contain version changes.
2. **Validate Version Bump**:

   - The workflow will analyze the commit messages using the **Conventional Commits** specification to determine the nature of the changes (major, minor, patch).
   - It will check that the version bump in `package.json` is appropriate based on the commits. If the bump does not match the type of changes, the workflow will fail with an error.
   - For example:
     - If the version was bumped from `1.0.0` to `1.0.1` but a commit message includes `feat:`, a warning will be raised that the minor version should have been bumped.
     - If the version change is from `1.0.0` to `1.0.1` but a breaking change (`fix!:`) is detected, the workflow will fail and warn that a major version bump is needed.

3. **Publish New Version**:
   - If the version bump is valid, the workflow will:
     - Automatically create a new GitHub release tagged with `<package-name>@<version>`.
     - Use **Lerna** to publish the package with the updated version.
     - Generate a changelog based on all the **Conventional Commits** since the last release and include it in the release description.

## Key Workflow Features

- **Automatic Version Detection**: The workflow automatically detects which packages have version changes and compares the new version to the previous version.
- **Commit Message Validation**: By analyzing **Conventional Commits**, the workflow ensures that the version bump is appropriate (major, minor, or patch).
- **GitHub Release Creation**: Once the version bump is validated, a new release is automatically created with the appropriate version tag and a changelog.
- **Changelog Generation**: The changelog is generated based on the commit messages and is included in the GitHub release description.

## Workflow Example

Here’s an example of how this workflow operates:

1. A developer makes a commit with the message: `feat: add new utility function`.
2. The `package.json` version in `packages/test-package-a` is updated from `1.0.0` to `1.0.1`.
3. The workflow detects the version change and checks the commits for the `test-package-a` package.
4. Since the commit type is a `feat:`, the workflow raises a warning that the version should be `1.1.0` (minor bump) instead of `1.0.1` (patch bump).
5. The workflow fails, prompting the developer to update the version accordingly.
6. Once the version is updated to `1.1.0`, the workflow runs again, passes the validation, and creates a new GitHub release tagged `test-package-a@1.1.0` with a changelog.

## Workflow Features

- **GitHub Actions**: The workflow uses GitHub Actions to automatically run on `push` or `pull_request`.
- **Lerna**: Lerna is used to handle version bumps, changelog generation, and GitHub releases.
- **Conventional Commits**: If the Conventional Commits format is used the workflow can automatically determine the type of changes (major, minor, patch). Further, it can generate a changelog based on the commit messages.

## Links

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Lerna](https://lerna.js.org/)
- [GitHub Actions](https://docs.github.com/en/actions)
- [GitHub Releases](https://docs.github.com/en/github/administering-a-repository/managing-releases-in-a-repository)
- [Monorepo](https://en.wikipedia.org/wiki/Monorepo)
- [Semantic Versioning](https://semver.org/)
- [Changelog](https://keepachangelog.com/en/1.0.0/)
- [GitHub Changelog Generator](https://github.com/github-changelog-generator/github-changelog-generator)
