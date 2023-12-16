+++
title = "Development"
description = "Help make Amphitheatre better."
weight = 10
+++

Thank you for taking the time to make a contribution to Amphitheatre. The following
document is a set of guidelines and instructions for contributing to Amphitheatre.

When contributing to our repositories, please consider first discussing the
change you wish to make by opening an issue.

### Recommended Reading

- [Rust](https://www.rust-lang.org/learn/get-started) (CLI, Backend / Api Server)
- [Flutter](https://docs.flutter.dev/) (Desktop)
- [Zola](https://www.getzola.org/documentation/getting-started/overview/) (Docs)

### How do I submit a pull request?

If your contribution requires changes to a project repository, look at the
`DEVELOPMENT.md` file if the repo has one to ensure you have the prerequisites
installed. It may also include other necessary steps (such as instructions on
running tests), but broadly, you will have to carry out the following steps:

- [Fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo) the repository.
- [Clone](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) your fork repository.
- Create a branch for the issue: `git checkout -b {{BRANCH_NAME}}`
- Make any changes deemed necessary.
- Commit your changes with a sign-off: `git commit -s`

A sign-off is a single line added to your commit messages that certifies that
you wrote and/or have the right to the contributed changes. The signature should
look as such:

```
Signed-off-by: John Doe <john.doe@email.com>
```

Also, git can automatically add the signature by adding the -s flag to the
commit command, `git commit -s`

The full text of the certification is available
[here](https://developercertificate.org/).

- Push to GitHub: `git push origin {{BRANCH_NAME}}`
- [Create the pull
  request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork).

### Code Testing

All submitted PRs go through a set of tests and reviews. You can run most of
these tests before a PR is submitted. In fact, we recommend it, because it will
save on many possible review iterations and automated tests. The testing
guidelines can be found here:

- [Contributor's Guide to Testing](@/contributing/testing.md)

### License

By contributing, you agree that your contributions will be licensed as described
in related repository's LICENSE file.
