# Git Homepage [![CI](https://github.com/git/git-scm.com/actions/workflows/ci.yml/badge.svg)](https://github.com/git/git-scm.com/actions/workflows/ci.yml) [![Help Contribute to Open Source](https://www.codetriage.com/git/git-scm.com/badges/users.svg)](https://www.codetriage.com/git/git-scm.com)

This is the repository for the [git-scm.com](https://git-scm.com) site.  It is meant to be the
first place a person new to Git will land and download or learn about the
Git SCM system.

This site is built with [Hugo](https://gohugo.io/) and served via GitHub Pages.

## Local development setup

You'll need the extended version of [Hugo](https://gohugo.io/). On Windows, we recommend using the Windows Subsystem for Linux (WSL). You can serve the site locally via

    $ hugo serve -w

The site should be running on http://127.0.0.1:1313. Note that it may be advisable to do this in a sparse checkout that excludes large parts of `content/`, to speed up the rendering time.

To test the site locally _with_ the search enabled, run this instead:

    $ hugo
    $ npx -y pagefind --site public --serve

That site should be running on http://127.0.0.1:1414

Note that this will take about 7 times as long, and the site will not be re-rendered and live-reloaded in the browser (unlike with `hugo serve -w`).

## Update manual pages

First, install the Ruby prerequisites:

	$ bundler install

Then, you can build the manual pages using a local Git source clone like this:

    $ ruby ./script/update-docs.rb /path/to/git/.git en

This will populate the manual pages for all Git versions. You can also populate them only for a specific Git version (faster):

    $ version=v2.23.0
    $ REBUILD_DOC=$version ruby ./script/update-docs.rb /path/to/git/.git en

Or you can populate the man pages from GitHub (much slower) like this:

    $ export GITHUB_API_TOKEN=github_personal_auth_token
    $ REBUILD_DOC=$version ruby ./script/update-docs.rb remote en  # specific version

Similarly, you can also populate the localized man pages. From a local clone of https://github.com/jnavila/git-html-l10n :

    $ ruby ./script/update-docs.rb /path/to/git-html-l10n/.git l10n  # all versions
    $ REBUILD_DOC=$version ruby ./script/update-docs.rb /path/to/git-html-l10n/.git l10n  # specific version

Or you can do it from GitHub (much slower) like this:

    $ export GITHUB_API_TOKEN=github_personal_auth_token
    $ REBUILD_DOC=$version ruby ./script/update-docs.rb remote l10n  # specific version

## Update the `Downloads` pages

Now you need to get the latest downloads for the downloads pages:

    $ ruby ./script/update-download-data.rb

## Update the ProGit book

First, you will have to get the necessary prerequisites:

    $ bundler install

Now you'll probably want some book data.

You'll have to get the book content from a repository on your computer by specifying the path:

    $ git clone https://github.com/progit/progit2-fr ../progit2-fr
    $ ruby ./script/update-book2.rb fr ../progit2-fr

That will generate the book content from the Asciidoc files and write the files to the local tree, ready to be committed and served via Hugo.

Alternatively, you need to have access to the [Pro Git project on GitHub](https://github.com/progit/progit2) through the API.

    $ export GITHUB_API_TOKEN=github_personal_auth_token
    $ ruby ./script/update-book2.rb en

If you have 2FA enabled, you'll need to create a [Personal Access Token](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).

If you want to build the book for all available languages, just skip the language code:

    $ ruby ./script/update-book2.rb

## Contributing

If you wish to contribute to this website, please [fork it on GitHub](https://github.com/git/git-scm.com), push your
change to a named branch, then send a pull request. If it is a big feature,
you might want to [start an issue](https://github.com/git/git-scm.com/issues/new) first to make sure it's something that will
be accepted. If it involves code, please also write tests for it.

## Adding new GUI

The [list of GUI clients](https://git-scm.com/downloads/guis) has been constructed by the community for a long time. If you want to add another tool you'll need to follow a few steps:

1. Add a new `.md` file with the GUI client details: https://github.com/git/git-scm.com/tree/main/content/data/guis
    1. The fields need to be enclosed within `---` lines
    2. The fields `name`, `project_url`, `price`, `license` should be very straightforward to fill.
    3. The field `image_tag` corresponds to the path of the image of the tool (should start with `images/guis/`).
    4. `platforms` is a list of at least 1 platform in which the tool is supported. The possibilities are: `Windows`, `Mac`, `Linux`, `Android`, and `iOS`
    5. `order` can be filled with the biggest number already existing, plus 1 (Adding to the bottom - this will be covered in the following steps). This is the only field whose value should _not_ be enclosed in double-quote characters.
    6. `trend_name` is an optional field that can be used for helping sorting the clients (also covered in the next steps)

2. Add the image to `static/images/guis/<GUI_CLIENT_NAME>@2x.png` and `static/images/guis/<GUI_CLIENT_NAME>.png` making sure the aspect ratio matches a 588:332 image.

## Useful links regarding working with Hugo and Pagefind

* https://gohugo.io/
* https://gohugo.io/content-management/shortcodes/
* https://pagefind.app/

## License

The source code for the site is licensed under the MIT license, which you can find in
the MIT-LICENSE.txt file.

All graphical assets are licensed under the
[Creative Commons Attribution 3.0 Unported License](https://creativecommons.org/licenses/by/3.0/).
