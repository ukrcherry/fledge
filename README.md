
<!-- README.md is generated from README.Rmd. Please edit that file -->
fledge
======

The goal of fledge is to streamline the process of versioning R packages and updating NEWS. Numbers are cheap, why not use them?

Currently, this "works for me", use at your own risk.

Workflow for development
------------------------

1.  In commit messages to `master`, mark everything that should go to `NEWS.md` with a bullet point (`-` or `*`).
2.  When you want to assign a version number to the current state of your R package, call

    ``` r
    fledge::bump_version()
    ```

3.  Edit `NEWS.md` as appropriate.
4.  If you have updated `NEWS.md`, call

    ``` r
    fledge::tag_version()
    ```

This has the following effects:

-   `NEWS.md` is composed from bits in your most recent commit messages, no need to resolve merge conflicts in `NEWS.md` anymore
-   A new version number is assigned automatically (this is modeled after `usethis::use_version()`)
-   A tag matching the version number is assigned automatically, with the most recent news included in the tag's message
    -   When you push the tag, contributors `@`-mentioned in the NEWS will be notified
    -   The tag will be listed on GitHub in the "Releases", it's very easy to convert it to a proper release by copying the message that's already part of the tag

Workflow for releasing to CRAN
------------------------------

1.  Call

    ``` r
    fledge::bump_version("patch")
    ```

    (or `"minor"` or `"major"` as appropriate).
2.  Edit `NEWS.md`, convert the changelog to a higher-level description of features and bug fixes.
3.  Call

    ``` r
    fledge::tag_version()
    ```

4.  Make last-minute adjustments before releasing to CRAN.
5.  When accepted, call

    ``` r
    fledge::tag_version(force = TRUE)
    ```

    *(not yet implemented)*.

NEWS generation
---------------

New entries are added to `NEWS.md` from commit messages to commits in `master`. Only top-level commits are considered (roughly equivalent to `git log --first-parent`.) In those commits, the messages are parsed. Only lines that start with a star `*` or a dash `-` are included, and a line that starts with three dashes makes sure that everything past that line isn't included. Example: The following commit message:

    Merge f-fancy-feature to master, closes #5678

    - Added fancy feature (#5678).

    - Fixed bug as a side effect (#9012).

    ---

    The fancy feature consists of the follwing components:

    - foo
    - bar
    - baz

will be added as:

    - Added fancy feature (#5678).
    - Fixed bug as a side effect (#9012).

to `NEWS.md`.

Documentation
-------------

The main entry point is `bump_version()`, which does the following:

1.  `update_news()`: collects `NEWS` entries from top-level commits
2.  `update_version()`: bump version in `DESCRIPTION`, add header to `NEWS.md`
3.  `tag_version()`: commit `DESCRIPTION` and `NEWS.md`, create tag with message

If you haven't committed since updating `NEWS.md` and `DESCRIPTION`, you can also edit `NEWS.md` and call `tag_version()` again. Both the commit and the tag will be updated.

Installation
------------

Install from GitHub via

``` r
remotes::install_github("krlmlr/fledge")
```
