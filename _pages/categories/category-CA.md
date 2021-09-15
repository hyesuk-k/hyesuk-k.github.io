---
title: "Computer Architecture"
layout: archive
permalink: categories/CA
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories['CA']%}
{% for post in posts %} 
  {% include archive-single.html type=page.entries_layout %} 
{% endfor %}
