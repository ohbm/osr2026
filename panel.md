---
layout: page
title: Panels and Round Tables
---

{% assign scheduled_sessions = site.data.osr_sessions | sort: "sort_order" %}

The OSR has always been a place to learn and share experiences. In 2026, the current OSR program includes 1 round table and 4 named panels. There is also one additional panel-length slot on Tuesday, June 16 that is still unlabeled, so only the named sessions are listed below. All times on this page are shown for Bordeaux, using the `Europe/Paris` timezone. During the June 15-18, 2026 program, that means CEST (UTC+2).

{% for session in scheduled_sessions %}
{% if session.category == "roundtable" or session.category == "panel" %}
## {% if session.category == "roundtable" %}OSR Round Table{% else %}OSR Panel{% endif %}: {{ session.title }}
<div id="{{ session.anchor }}"></div>
When: {{ session.start_time }}-{{ session.end_time }} CEST (UTC+2) | {{ session.day_label }} ({{ session.weekday }})

{{ session.description }}

This {% if session.category == "roundtable" %}round table{% else %}panel{% endif %} will:
{% for objective in session.objectives %}
{{ forloop.index }}. {{ objective }}
{% endfor %}

{% endif %}
{% endfor %}
