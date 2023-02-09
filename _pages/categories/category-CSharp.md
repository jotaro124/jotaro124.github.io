---
title: "C#"
layout: archive
permalink: /categories/CSharp
author_profile: true
sidebar:
    nav: "docs"
---

{% assign posts = site.categories.CSharp%}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}