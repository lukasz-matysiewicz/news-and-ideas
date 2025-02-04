# 3mdeb blog documentation

## Table of contents

<!-- toc -->

- [Deployment status](#deployment-status)
- [Usage](#usage)
  * [Add new post](#add-new-post)
    + [Categories and tags](#categories-and-tags)
  * [Local preview](#local-preview)
  * [Deployment on https://beta.3mdeb.com](#deployment-on-https---beta3mdebcom)
  * [Deployment on `production` blog](#deployment-on--production--blog)
- [Good practices](#good-practices)
  * [Grammarly](#grammarly)
  * [Markdown](#markdown)
  * [Single or multiple authors](#single-or-multiple-authors)
  * [SEO best known methods](#seo-best-known-methods)

<!-- tocstop -->

## Deployment status

Production deploy status:

[![Build Status](https://travis-ci.com/3mdeb/news-and-ideas.svg?branch=master)](https://travis-ci.com/3mdeb/news-and-ideas)

Beta deploy status:

[![Build Status](https://travis-ci.com/3mdeb/news-and-ideas.svg?branch=develop)](https://travis-ci.com/3mdeb/news-and-ideas)

## Usage

### Add new post

To add new post to our blog, first prepare local repository:

1. Clone repository: `git clone git@github.com:3mdeb/news-and-ideas.git`
1. Change directory: `cd news-and-ideas`
1. Create new file using [blog post template](blog/content/post/YYYY-MM-DD-template-post.md)
```
cd blog/content/post
cp YYYY-MM-DD-template-post.md `date +%Y-%m-%d`-my-new-blog-post.md
vim `date +%Y-%m-%d`-my-new-blog-post.md
```
1. Familiarize yourself with [good practices](#good-practices) section.
1. Use [Markdown](#Markdown) to write your blog post. You can use [local preview](#local-preview)
1. Finished blog post should got to review. Please create Github Pull Request
   to `develop` branch as described [here](#deployment)
1. If deployment to beta doesn't show any issues please ask maintainer for sync
   to [master](#deployment).

#### Categories and tags

We have several categories you can choose from:

- Firmware
- IoT
- Miscellaneous
- OS Dev
- App Dev
- Security
- Manufacturing

Basically, there is a huge pool of tags we have, and you can add any tags you
like.

### Local preview

1. Remove `public` directory: `rm -rf blog/public`
1. Generate blog: `docker run --rm -it -v $PWD/blog:/src -u hugo jguyomard/hugo-builder hugo`
1. Generated files can be found in `blog/public`

There is possibility to check whether new post is well formatted:
1. Run local server: `docker run --rm -it -v $PWD/blog:/src -p 1313:1313 -u hugo jguyomard/hugo-builder hugo server -w --bind=0.0.0.0`
1. Go to [http://localhost:1313/](http://localhost:1313/) to view the changes.

### Deployment on https://beta.3mdeb.com

1. Push commits with your blog post to your branch. There are no strict rules
   for branch naming. It should refer to the post title.

1. Create a Pull Request, targeting `develop` branch.

1. Once your Pull Request gets merged to `develop`, the blog should be
   automatically deployed to the [beta](https://beta.blog.3mdeb.com). You can
   check the deploy job status on the
   [travis-ci.com](https://travis-ci.com/3mdeb/news-and-ideas)

### Deployment on `production` blog

When the blog's status in [beta](https://beta.blog.3mdeb.com) is acceptable,
we can deploy to [production](https://blog.3dmeb.com). To do that, simply
create the Pull Request from `develop` to `master`. Once it gets merged, the
same version of blog should be deployed to
[production](https://blog.3mdeb.com). You can check the deploy job status on the
[travis-ci.com](https://travis-ci.com/3mdeb/news-and-ideas)

## Good practices

### Grammarly

Grammarly is a great, free tool for all bloggers and anyone who needs to write
documentation in English.
It will let you know if you skipped a coma or made a typo, as well, as it will
check advanced grammar mistakes, too. Bear in mind, that the free version has
its limits, so you need to keep an eye on it at all times and still, you are
the one who distinguishes when to use a/an or the, as it only suggests changes.

Two versions of Grammarly are available: a plugin for Chrome/Chromium or online
application. You need to create an account (it's for free) to be able to use
Grammarly.

Visit the website [Grammarly](https://app.grammarly.com/) and create an account.

>It is a MUST-HAVE application for anyone who writes posts or documentation, so
feel obliged to use it.

---

### Markdown

There is on the internet a great tool for anyone writing posts, that is
[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).
It is awesome because it is short and clear.

However, here are some most important commands:

- #, ##, ###, ####, #####, ###### - headers (just like in HTML `<h1><h2><h3>`
etc.)
- `- some text` - it is simply a list item for an unordered list. - `<li>` in
HTML
- `1. some text` - an ordered list. Number does not matter, it will be ordered
automatically.
- `[Visible text](URL)` - a link
- ` - inline code. Can be used as an inline quote
- ` x3 - block of code. You can write, next to it (connected) a programming
language

If your post includes any images, they must be located in `blog/static/img`
directory. To link them in file written in Markdown, use the format below:

```
![alt text](/img/image_name.jpg)
```

**Remember about newlines before markdown tables, lists, quotes (>) and blocks
of text (\`\`\`).**

I hope this will help. To see more, visit [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

>You can write attach inline HTML into Markdown and it will work!
>`<span style="color: blue">Some text</span>`

### Single or multiple authors

In general, author meta-field MUST be strictly formatted (lowercase, non-polish
letters):

```
author: name.surname
```

If post has multiple authors, author meta-field MUST be formatted as below:

```
author:
    - name.surname
    - name.surname
```

### SEO best known methods

* Meta description - each post should have single-sentence description with
  proper keywords (try to add as many keywords as possible)
> previously set in the Yoast SEO plugin [TBD - how to set them now]

* Tags selection - use proper tags (good examples are tags for articles of our
	competition and results from the Google first site)

* Graphic/image title - description with keywords related to whole article. All
images uploaded to WordPress should be edited in terms of SEO (WP-admin panel in
the `Media` tab). It is required to complete the `Caption` field and add tags
with `Meta Tag manager` -> `Add Meta Tag` (at the bottom).
