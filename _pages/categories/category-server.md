---
title: "Server Basic"
layout: archive
permalink: /categories/server
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.server %}
{% for post in posts %}{% include archive-single.html type=page.entries_layout %} {% endfor %}