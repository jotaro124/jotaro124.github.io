---
title: "모의 A형 테스트"
layout: archive
permalink: /categories/MockATest
author_profile: true
sidebar:
    nav: "docs"
---

{% assign posts = site.categories.MockATest %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}