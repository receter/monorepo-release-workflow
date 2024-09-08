# Monorepo release workflow

In this repository, I want to test a monorepo release workflow I have in mind for a project I'm working on.

## The idea

~~Whenever a change is pushed to the `main` branch or a pull request is merged, if the `package.json` file of any package in the monorepo contains a version change, the following workflow will be triggered.~~

I did decide to use the Google release-please action. It will create a pull request (based on conventional commits) with the correct version change and the changelog. If the pull request is then merged by a human, the release will be created and the packages will be published.

Furthermore, there are two more individual jobs in the workflow that publish the packages to the npm registry after a new release was created by the release-please action.

With this approach the publish jobs can be re-run manually in case of a failure.

## Notes

### Editing the PR description

While you can edit the PR description this will not affect the creation of the content for `CHANGELOG.md`. This will be generated from scratch by the release-please action based on the conventional commits in the PR when the PR is merged.

But the modified PR description will be used for the GitHub release.

> **Note:** When there is another push to the `main` branch after the description was edited, the PR description will be overwritten by the release-please action.

### Changelog Sections

Currently the default configuration for changelog sections is active. This can be changed in `release-please-config.json` file. The default configuration ([source](https://git.io/JqCZL)) is:

```json
  "changelog-sections": [
    { "type": "feat", "section": "Features" },
    { "type": "feature", "section": "Features" },
    { "type": "fix", "section": "Bug Fixes" },
    { "type": "perf", "section": "Performance Improvements" },
    { "type": "revert", "section": "Reverts" },
    { "type": "docs", "section": "Documentation", "hidden": true },
    { "type": "style", "section": "Styles", "hidden": true },
    { "type": "chore", "section": "Miscellaneous Chores", "hidden": true },
    { "type": "refactor", "section": "Code Refactoring", "hidden": true },
    { "type": "test", "section": "Tests", "hidden": true },
    { "type": "build", "section": "Build System", "hidden": true },
    { "type": "ci", "section": "Continuous Integration", "hidden": true }
  ]
```

## Links

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Lerna](https://lerna.js.org/)
- [GitHub Actions](https://docs.github.com/en/actions)
- [GitHub Releases](https://docs.github.com/en/github/administering-a-repository/managing-releases-in-a-repository)
- [Monorepo](https://en.wikipedia.org/wiki/Monorepo)
- [Semantic Versioning](https://semver.org/)
- [Changelog](https://keepachangelog.com/en/1.0.0/)
- [GitHub Changelog Generator](https://github.com/github-changelog-generator/github-changelog-generator)
- [Release Please](https://github.com/googleapis/release-please)
- [Release Please Action](https://github.com/googleapis/release-please-action)
