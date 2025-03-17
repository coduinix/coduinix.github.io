---
title: Coduinix Home
layout: splash
permalink: /
author_profile: true
header:
  overlay_image: /assets/images/banner.jpg
  caption: JCON Europe 2024
  actions:
    - label: "Blog"
      url: "/blog"
    - label: "Talks"
      url: "/talks"
    - label: "Articles"
      url: "/articles"
---
`coduinix.com` is the personal website of Hinse ter Schuur.

[More about me](about.md)

---

# latest blog posts

{% assign posts = site.posts %}

{% assign entries_layout = page.entries_layout | default: 'list' %}
<div class="entries-{{ entries_layout }}">
  {% for post in posts limit: 4 %}
    {% include archive-single.html type=entries_layout %}
  {% endfor %}
</div>

[More blog posts](/blog){: .btn .btn--primary .btn--large}