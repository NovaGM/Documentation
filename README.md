# Documentation

User-made documentation for [GooseMod](https://goosemod.com).

[View the documentation](https://novagm.github.io/Documentation).

## Adding content

To add content to this documentation, first fork it to have your own copy of
the repo. Clone your fork and make your edits there.

To add a page, add a MarkDown `.md` file to one of the categories
(eg: `module-development/`).

You can also add your own category by adding a directory with a `README.md`
file inside, that describes what your category is about and lists the
category's files by adding this line:

```
{% include list.liquid all=true %}
```

By default Jekyll will pull the first title as the page's title, but that can
be overwritten with front matter at the top of your page:

```yaml
---
title: Your title
# other front matter definitions
---

```

To sort a category's pages, add a `sort` property to the pages' front matter
section:

```yaml
---
# other front matter definitions
sort: <number>
---

```

## Updating the documentation website

Before pushing your edits to the documentation, you may want to see what they
look like. To do so, you need to:

- have a local [Ruby](https://www.ruby-lang.org) environment
- install the `bundler` and `jekyll` gems
  `gem install bundler jekyll`
- install this project's specific requirements:
  `bundle install` (or `bundle update` if you already built it before)
- run Jekyll:
  `bundle exec jekyll serve`
- open the address that is returned on the terminal in your browser
  (usually `http://127.0.0.1:4000/`)

To push your modifications to the documentation, push your modifications to
your fork and submit a pull request to this one.

No particular operation is needed, [Github Pages](https://pages.github.com)
will update the documentation's website whenever commits are pushed to this
repo.

## Additional resources

- GH Pages: [Github Pages docs](https://docs.github.com/en/github/working-with-github-pages/setting-up-a-github-pages-site-with-jekyll)
- Generator: [Jekyll docs](https://jekyllrb.com/docs/)
- Theme: [jekyll-rtd-theme](https://jekyll-rtd-theme.rundocs.io/) and
  [rundocs.io](https://rundocs.io/)

