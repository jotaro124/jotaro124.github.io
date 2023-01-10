---
title: "Assignment"
layout: archive
permalink: /categories/assignment
author_profile: true
sidebar:
    nav: "docs"
---

{% assign posts = site.categories.assignment %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
