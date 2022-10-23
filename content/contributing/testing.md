+++
title = "Testing"
description = "Contributor's Guide to Testing"
weight = 50
+++

## Testing Your Code

Amphitheathre uses github actions to run automated tests on any PR, before
merging. However, a PR will not be reviewed before all tests are green, so to
save time and prevent your PR from going stale, it is best to test it before
submitting the PR.

## Opening A Pull Request

### Draft Mode

You may open a pull request in [draft
mode](https://github.blog/2019-02-14-introducing-draft-pull-requests). All
automated tests will still run against the PR, but the PR will not be assigned
for review. Once a PR is ready for review, transition it from Draft mode, and
code owners will be notified.

### Pre-Requisites for PR Merge

In order for a PR to be merged, the following conditions should exist: 
1. The PR has passed all the automated tests (style, build & conformance tests). 
2. PR commits have been signed with the --signoff option. 
3. PR was reviewed and approved by a code owner. 
4. PR is rebased against upstream's master branch.