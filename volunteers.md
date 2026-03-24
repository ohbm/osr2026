---
layout: page
title: Team
---

The 2026 Open Science Special Interest Group and Open Science Room team is listed below using the current member information shared by the organizers.

<br>

<html>

{% assign sections = "OSSIG Executive|OSR Leadership|Program and Community Leads|2026 Volunteers" | split: "|" %}
{% for section in sections %}
{% assign members = site.data.volunteers | where: "Section", section | sort: "SortOrder" %}
{% if members.size > 0 %}
<h2 style="text-align: center;">{{ section }}</h2>
{% assign n_rows = members.size | plus: 1 | divided_by: 2 | minus: 1 %}
<table class="people">
{% for row in (0..n_rows) %}
    <tr class="people">
    {% for col in (0..1) %}
        <td class="people">
        {% assign idx = row | times: 2 | plus: col %}
        {% assign member = members[idx] %}
        {% if member.Name %}

            {% assign img_path = "/img/speakers/placeholder.jpg" %}
            {% assign names = member.Name | downcase | split: " " %}
            {% for file in site.static_files %}
                {% if file.path contains names.first and file.path contains names[1] %}
                    {% assign img_path = file.path %}
                {% endif %}
            {% endfor %}

            <aside class="speaker-card">
            <header>
                <img src="{{ site.baseurl }}{{ img_path }}" style="height:200px; border-radius:50%;">
                <h3>{{ member.Name }}</h3>
                <h4>{{ member.Job }}</h4>
                {% if member.Affiliation %}<h6>{{ member.Affiliation }}</h6>{% endif %}
                {% if member.MemberType or member.Committee %}
                <p>
                    {% if member.MemberType %}{{ member.MemberType }}{% endif %}
                    {% if member.MemberType and member.Committee %} | {% endif %}
                    {% if member.Committee %}{{ member.Committee }}{% endif %}
                </p>
                {% endif %}
                <h4>
                {% include profile-social-links.html person=member %}
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
{% endif %}
{% endfor %}

</html>

### Thank You
We are grateful to everyone contributing time and energy to the Open Science Room and the broader OSSIG community this year.
