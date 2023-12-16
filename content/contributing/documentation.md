+++
title = "Documentation"
description = "Help improve Amphitheatre documentation."
weight = 20
+++

We are glad to see you're interested in contributing to the Amphitheatre
documentation. If this is the first Open Source project you've contributed to,
we strongly suggest reading GitHub's excellent guide: How to [Contribute to Open
Source](https://opensource.guide/how-to-contribute).

### Finding Documentation Issues to Work On

You can find a list of open documentation-related issues
[here](https://github.com/amphitheatre-app/docs/issues). When you find something
you would like to work on:

- Express your interest to start working on an issue via comments.
- One of the maintainers will assign the issue for you.
- You can start working on the issue. When you're done, simply submit a pull
  request.

### Requirements for Documentation Pull Requests

When you create a new pull request, we expect some requirements to be met.

- Follow this naming convention for Pull Requests:
- When adding new documentation, add `New Documentation:` before the title. E.g.
  `New Documentation: Getting Started`
- When fixing documentation, add `Fix Documentation:` before the title. E.g.
  `Fix Documentation: Getting Started`
- When updating documentation, add `Update Documentation:` before the title.
  E.g. `Update Documentation: Getting Started`
- If your Pull Request closes an issue, you must write `Closes #ISSUE_NUMBER`
  where the ISSUE_NUMBER is the number in the end of the link url or the
  relevent issue. This will link your pull request to the issue, and when it is
  merged, the issue will close.
- For each pull request made, we run tests to check if there are any broken
  links, the markdown formatting is valid, and the linter is passing.

### Testing Documentation Site Locally

Run a local instance of zola for developing the Amphitheatre Documentation.

```bash
zola serve
```

local build and serve of zola with auto update enabled, then go to
[http://127.0.0.1:1111](http://127.0.0.1:1111)
