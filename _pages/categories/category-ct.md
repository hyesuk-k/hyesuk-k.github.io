---
title: "CodingTest"
layout: archive
permalink: categories/ct
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories['ct']%}
{% for post in posts %} 
  {% include archive-single.html type=page.entries_layout %} 
{% endfor %}
