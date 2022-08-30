---
title: "Web-Hacking"
layout: archive
permalink: /categories/web-hacking
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.web-hacking %}
{% for post in posts %}{% include archive-single.html type=page.entries_layout %} {% endfor %}