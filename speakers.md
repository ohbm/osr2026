---
layout: page
title: Speakers
---


We are excited to host this year's named panelists, moderators, and table-talk organizers in the Open Science Room.
Click on a speaker below to see their session and presentation details.

Emergent sessions are spontaneous in nature, so their speakers will be added separately as they are confirmed.

<br>

<html>

{% assign n_rows = site.speakers.size | divided_by:2 %}
<table class="people">
{% for row in (0..n_rows) %}
    <tr class="people">
    {% for col in (0..1) %}
        <td class="people">

        {% assign idx = row | times:2 | plus:col %}
        {% assign speaker = site.speakers[idx] %}
        {% if speaker.Name %}

            {% assign img_path = nil %}
            {% assign names = speaker.Name | downcase | split: " " %}
            {% assign img_path = "/img/speakers/placeholder.jpg" %}
            {% for file in site.static_files %}                
                {% if file.path contains names.first and file.path contains names[1] %}
                    {% assign img_path = file.path %}
                {% endif %}
            {% endfor %}

            <aside class="speaker-card {% if speaker.column %} {{ speaker.column }}{% endif %}">
            <header>
                <a style="display:block; color:#05323F" href="{{ site.baseurl }}{{speaker.url}}">
                    <img src="{{ site.baseurl }}{{ img_path }}" style="height:200px; border-radius:50%;">

                    <h3>{{ speaker.Name }}</h3>

                    <h6>{{ speaker.Affiliation }}</h6>
                </a>

                <h4>
                {% include profile-social-links.html person=speaker %}
                </h4>
                <br>
            </header>
            </aside>
        {% endif %}
        </td>
    {% endfor %}
    </tr>
{% endfor %}
</table>

</html>
