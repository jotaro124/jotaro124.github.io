---
title: "모의 A형 기출"
layout: archive
permalink: /categories/MockAPast
author_profile: true
sidebar:
    nav: "docs"
---

{% assign posts = site.categories.MockAPast %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}