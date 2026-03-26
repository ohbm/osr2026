---
layout: page
title: Schedule
permalink: /schedule/
---

{% assign schedule_days = site.data.osr_schedule.days %}
{% assign schedule_base_url = "https://ohbm.github.io/osr2026" %}

<div class="schedule-days">
{% for day in schedule_days %}
  <div
    id="day-{{ day.id }}"
    class="schedule-day{% if forloop.first %} active{% endif %}"
    data-schedule-day="{{ day.id }}"
    role="button"
    tabindex="0"
  >{{ day.tab_label }}</div>
{% endfor %}
</div>

<p style="text-align: center;">
<strong>Bordeaux / Europe/Paris</strong><br>
<em>During June 15-18, 2026, Bordeaux is on CEST (UTC+2), not GMT+1.</em>
</p>

<p style="text-align: center;">
<a href="/downloads/OSRschedule.ics" download="OSR2026_Bordeaux.ics">Download the full OSR calendar (.ics)</a><br>
<em>You can import that file into Apple Calendar, Outlook, or Google Calendar, and each event below also has its own Google Calendar link.</em>
</p>

<p style="text-align: center;">
<em>The schedule below follows the current OHBM 2026 OSR program. One Tuesday morning panel-length slot is still unlabeled in the current program, so it is listed here as TBD.</em>
</p>

{% for day in schedule_days %}
<div id="schedule-{{ day.id }}" class="schedule-block{% if forloop.first %} active{% endif %}">
    <h4>{{ day.heading }}</h4>
    <div class="schedule-content">
        <table class="osr-schedule">
            <tr>
                <td><b>Time</b></td>
                <td><b>OPEN SCIENCE ROOM</b></td>
                <td><b>Calendar</b></td>
            </tr>
            {% for entry in day.entries %}
            {% assign session = nil %}
            {% if entry.session_id %}
              {% assign session = site.data.osr_sessions | where: "id", entry.session_id | first %}
            {% endif %}
            <tr>
                <td>{% if session %}{{ session.start_time }}-{{ session.end_time }}{% else %}{{ entry.start_time }}-{{ entry.end_time }}{% endif %}</td>
                <td>
                    {% if session %}
                      {% assign session_href = session.page_url | append: "#" | append: session.anchor %}
                      {% capture calendar_title %}{% if session.category == "panel" %}Panel {{ session.number }}: {% elsif session.category == "tabletalk" %}Table Talk {{ session.number }}: {% endif %}{{ session.title }}{% endcapture %}
                      <div>
                        {% if session.category == "roundtable" %}
                          <a href="{{ session.page_url }}">{{ session.title }}</a>
                        {% elsif session.category == "panel" %}
                          <a href="{{ session.page_url }}">Panel {{ session.number }}:</a> {{ session.title }}
                        {% else %}
                          <a href="{{ session.page_url }}">Table Talk {{ session.number }}:</a> {{ session.title }}
                        {% endif %}
                      </div>
                    {% else %}
                      {% assign session_href = entry.page_url %}
                      {% assign calendar_title = entry.title %}
                      <div>{% if entry.page_url == "/schedule/" %}{{ entry.title }}{% else %}<a href="{{ entry.page_url }}">{{ entry.title }}</a>{% endif %}</div>
                    {% endif %}
                </td>
                <td><a href="https://calendar.google.com/calendar/render?action=TEMPLATE&text={{ calendar_title | uri_escape }}&dates={% if session %}{{ session.gcal_start }}%2F{{ session.gcal_end }}{% else %}{{ entry.gcal_start }}%2F{{ entry.gcal_end }}{% endif %}&ctz=Europe%2FParis&details=OHBM%20Open%20Science%20Room%202026%20session.%20Full%20schedule%3A%20{{ schedule_base_url | append: '/schedule/' | uri_escape }}&location={{ schedule_base_url | append: session_href | uri_escape }}" target="_blank" rel="noopener">Add to Google Calendar</a></td>
            </tr>
            {% endfor %}
        </table>
    </div>
</div>
{% endfor %}

<div class="schedule-leave-space-before-footer">
</div>

<script>
  function showScheduleForDay(dayId) {
    var dayTabs = document.querySelectorAll(".schedule-day");
    var dayBlocks = document.querySelectorAll(".schedule-block");

    dayTabs.forEach(function(tab) {
      tab.classList.toggle("active", tab.dataset.scheduleDay === dayId);
    });

    dayBlocks.forEach(function(block) {
      block.classList.toggle("active", block.id === "schedule-" + dayId);
    });
  }

  document.addEventListener("DOMContentLoaded", function() {
    var dayTabs = document.querySelectorAll(".schedule-day");
    var activeDay = document.querySelector(".schedule-day.active");

    dayTabs.forEach(function(tab) {
      tab.addEventListener("click", function() {
        showScheduleForDay(tab.dataset.scheduleDay);
      });

      tab.addEventListener("keydown", function(event) {
        if (event.key === "Enter" || event.key === " ") {
          event.preventDefault();
          showScheduleForDay(tab.dataset.scheduleDay);
        }
      });
    });

    if (activeDay) {
      showScheduleForDay(activeDay.dataset.scheduleDay);
    }
  });
</script>
