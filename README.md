# Documentation for Amphitheatre

This site is built with [Zola](https://www.getzola.org/). Content is written in
[CommonMark](https://commonmark.org/), a strongly defined, highly compatible
specification of [Markdown](https://www.markdownguide.org/). Pull requests
welcome!

## Developing

``` bash
$ zola serve # dev server at http://127.0.0.1:1111
```

This will build and serve the site using a local server. Before starting, Zola
will delete the output directory (by default public in project root) to start
from a clean slate.

The serve command will watch all your content and provide live reload without a
hard refresh if possible. If you are using WSL2 on Windows, make sure to store
the website on the WSL file system.

Some changes cannot be handled automatically and thus live reload may not always
work. If you fail to see your change or get an error, try restarting zola serve.

## Deploying

The site is automatically deployed when commits land in `master`, via
[Netlify](https://www.netlify.com/).

If you are the maintainer of a community translation fork and would like to
deploy via Netlify instead of GitHub pages, please ping @wangeguo in an issue to
request a Netlify team membership and DNS update.

## Want to help with the translation?

If you feel okay with translating quite alone, you can fork the repo, post a
comment on the [Community Translation
Announcements](https://github.com/amphitheatre-app/docs/issues/1) issue page to
inform others that you're doing the translation and go for it.

## Credits

Heavily inspired by
https://github.com/GoogleContainerTools/skaffold/tree/main/docs