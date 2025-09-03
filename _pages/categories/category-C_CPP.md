---
title: "C/C++"
layout: archive
permalink: /categories/C_CPP
author_profile: true
sidebar:
    nav: "docs"
---

{% assign posts = site.categories.C_CPP%}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}