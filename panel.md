---
layout: page
title: Panels and Round Tables
---

{% assign panel_sessions = site.data.osr_sessions | where_exp: "session", "session.category == 'roundtable' or session.category == 'panel'" | sort: "sort_order" %}

The OSR has always been a place to learn and share experiences. In 2026, the current OSR program includes 1 round table and 4 named panels. There is also one additional panel-length slot on Tuesday, June 16 that is still unlabeled, so only the named sessions are listed below. All times on this page are shown for Bordeaux, using the `Europe/Paris` timezone. During the June 15-18, 2026 program, that means CEST (UTC+2).

{% for session in panel_sessions %}
## {% if session.category == "roundtable" %}OSR Round Table{% else %}OSR Panel{% endif %}: {{ session.title }}
<div id="{{ session.anchor }}"></div>
When: {{ session.start_time }}-{{ session.end_time }} CEST (UTC+2) | {{ session.day_label }} ({{ session.weekday }})

{{ session.description }}

This {% if session.category == "roundtable" %}round table{% else %}panel{% endif %} will:
{% for objective in session.objectives %}
{{ forloop.index }}. {{ objective }}
{% endfor %}

{% endfor %}
