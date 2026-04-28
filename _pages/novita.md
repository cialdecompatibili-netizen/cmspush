---
layout: single
title: "Novità"
permalink: /novita/
author_profile: false
---

{% capture changelog_content %}{% include changelog.md %}{% endcapture %}
{{ changelog_content | markdownify }}
