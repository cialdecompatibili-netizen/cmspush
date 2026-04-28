---
layout: single
title: "Novità"
permalink: /novita/
author_profile: false
---

{% capture changelog %}
{% include_relative CHANGELOG.md %}
{% endcapture %}
{{ changelog | markdownify }}
