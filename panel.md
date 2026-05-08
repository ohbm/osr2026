---
layout: page
title: Panels and Round Tables
---

{% assign scheduled_sessions = site.data.osr_sessions | sort: "sort_order" %}

The OSR has always been a place to learn and share experiences. In 2026, the current OSR program includes 1 round table and 4 named panels. All times on this page are shown for Bordeaux, using the `Europe/Paris` timezone. During the June 15-18, 2026 program, that means CEST (UTC+2).

{% for session in scheduled_sessions %}
{% if session.category == "roundtable" or session.category == "panel" %}
<h2 id="{{ session.anchor }}">{% if session.category == "roundtable" %}OSR Round Table{% else %}OSR Panel{% endif %}: {{ session.title }}</h2>
{% assign session_roster = site.data.session_speakers[session.id] %}
{% if session_roster and session_roster.panelists.size > 0 %}
<div style="display:flex; flex-wrap:wrap; gap:12px; margin:10px 0 20px 0; align-items:flex-start;">{% for entry in session_roster.panelists %}{% assign speaker_name = entry.name | default: entry %}{% assign spk = site.speakers | where: "Name", speaker_name | first %}{% if spk %}{% assign speaker_photo = spk.Photo | default: "/img/speakers/placeholder.jpg" %}<div style="text-align:center; width:84px;"><a href="{{ site.baseurl }}{{ spk.url }}" style="text-decoration:none; color:inherit;"><img src="{{ site.baseurl }}{{ speaker_photo }}" alt="{{ spk.Name }}" style="width:80px; height:80px; border-radius:50%; object-fit:cover; display:block; margin:0 auto 5px;"><span style="font-size:0.75em; line-height:1.3; display:block;">{{ spk.Name }}</span></a></div>{% endif %}{% endfor %}</div>
{% endif %}
When: {{ session.start_time }}-{{ session.end_time }} CEST (UTC+2) | {{ session.day_label }} ({{ session.weekday }})

{% if session.crowdcast_url %}<a href="{{ session.crowdcast_url }}" target="_blank" rel="noopener">Watch on Crowdcast</a>{% endif %}

{{ session.description }}

This {% if session.category == "roundtable" %}round table{% else %}panel{% endif %} will:
{% for objective in session.objectives %}
{{ forloop.index }}. {{ objective }}
{% endfor %}

{% if session_roster %}
{% if session_roster.panelists.size > 0 %}
**Speakers:**
{% for entry in session_roster.panelists %}{% assign speaker_name = entry.name | default: entry %}{% assign spk = site.speakers | where: "Name", speaker_name | first %}{% if spk %}- [{{ spk.Name }}]({{ site.baseurl }}{{ spk.url }}){% if spk.Title %}, {{ spk.Title }}{% endif %}{% if spk.Affiliation %} — {{ spk.Affiliation }}{% endif %}
{% if entry.talk_title %}  *{{ entry.talk_title }}*
{% endif %}{% if entry.talk_summary %}
  {{ entry.talk_summary }}
{% endif %}{% endif %}{% endfor %}
{% endif %}
{% if session_roster.moderators.size > 0 %}
**Moderated by:**
{% for name in session_roster.moderators %}{% assign spk = site.speakers | where: "Name", name | first %}{% if spk %}- [{{ spk.Name }}]({{ site.baseurl }}{{ spk.url }}){% if spk.Affiliation %} — {{ spk.Affiliation }}{% endif %}
{% endif %}{% endfor %}
{% endif %}
{% endif %}

{% if site.show_feedback_forms and session.feedback_url %}
<a href="{{ session.feedback_url }}" target="_blank" rel="noopener">Session feedback form</a>
{% endif %}

{% endif %}
{% endfor %}
