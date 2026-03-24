---
layout: page
title: Table Topic Discussions
---

{% assign table_talks = site.data.osr_sessions | where: "category", "tabletalk" | sort: "sort_order" %}

Table talks are informal discussions across the community, open to trainees, early-career researchers, and senior researchers alike. In 2026, the current OSR program includes 4 table talks focused on practical questions in open science. All times on this page are shown for Bordeaux, using the `Europe/Paris` timezone. During the June 15-18, 2026 program, that means CEST (UTC+2).

### OHBM 2026 program

{% for session in table_talks %}
#### Table Talk {{ session.number }}: {{ session.title }}
<div id="{{ session.anchor }}"></div>
When: {{ session.start_time }}-{{ session.end_time }} CEST (UTC+2) | {{ session.day_label }} ({{ session.weekday }})

{{ session.description }}

This table talk will:
{% for objective in session.objectives %}
{{ forloop.index }}. {{ objective }}
{% endfor %}

{% endfor %}
