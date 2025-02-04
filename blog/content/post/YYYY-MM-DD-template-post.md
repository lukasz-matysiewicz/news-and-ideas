---
title: Template post title
abstract: Abstract first sentence.
          Abstract second sentence.
          Abstract third sentence.
cover: /covers/image-file.png
author: name.surname
layout: post
published: false
date: YYYY-MM-DD
archives: "YYYY"

tags:
  - tag 1
  - tag 2
categories:
  - Firmware
  - IoT
  - Miscellaneous
  - OS Dev
  - App Dev
  - Security
  - Manufacturing

---

Your post content

> post abstract in the header is required for the posts summary in the blog list
  and must contain from 3 to 5 sentences, please note that abstract would be used
  for social media and because of that should be focused on keywords/hashtags

> post cover image should be located in `blog/static/covers/` directory or may be
  linked to `blog/static/img/` if image is used in post content

> author meta-field MUST be strictly formatted (lowercase, non-polish letters):

```
author: name.surname
```

> if post has multiple authors, author meta-field MUST be strictly formatted:

```
author:
    - name.surname
    - name.surname
```

> remember about newlines before lists, tables, quotes blocks (>) and blocks of
  text (\`\`\`)

> copy all post images to `blog/static/img` directory. Example usage:

![alt-text](/img/file-name.jpg)

> example usage of asciinema videos:

[![asciicast](https://asciinema.org/a/xJC0QaKuHrMAPhhj5KMZUhMEO.svg)](https://asciinema.org/a/xJC0QaKuHrMAPhhj5KMZUhMEO?speed=1)

> remember to change published meta-field to `true` when post is done

## Summary

Summary of the post.

OPTIONAL ending (may be based on post content):

If you think we can help in improving the security of your firmware or you
looking for someone who can boost your product by leveraging advanced features
of used hardware platform, feel free to [book a call with us](https://calendly.com/3mdeb/consulting-remote-meeting)
or drop us email to `contact<at>3mdeb<dot>com`. If you are interested in similar
content feel free to [sing up to our newsletter](http://eepurl.com/gfoekD)
