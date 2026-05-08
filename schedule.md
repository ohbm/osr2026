---
layout: page
title: Schedule
permalink: /schedule/
---

{% assign schedule_days = site.data.osr_schedule.days %}
{% assign schedule_base_url = site.url %}
{% assign schedule_path = '/schedule/' | relative_url %}

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
<a href="{{ '/downloads/OSRschedule.ics' | relative_url }}" download="OSR2026_Bordeaux.ics">Download the full OSR calendar (.ics)</a><br>
<em>You can import that file into Apple Calendar, Outlook, or Google Calendar, and each event below also has its own Google Calendar link.</em>
</p>

<p style="text-align: center;">
<em>The schedule below follows the current OHBM 2026 OSR program.</em>
</p>

{% for day in schedule_days %}
<div id="schedule-{{ day.id }}" class="schedule-block{% if forloop.first %} active{% endif %}">
    <h4>{{ day.heading }}</h4>
    <div class="schedule-content">
        <table class="osr-schedule">
            <tr>
                <td><b>Time</b></td>
                <td><b>OPEN SCIENCE ROOM</b></td>
                <td><b>Crowdcast</b></td>
                <td><b>Calendar</b></td>
            </tr>
            {% for entry in day.entries %}
            {% assign session = nil %}
            {% if entry.session_id %}
              {% assign session = site.data.osr_sessions | where: "id", entry.session_id | first %}
            {% endif %}
            {% assign crowdcast_url = session.crowdcast_url | default: entry.crowdcast_url %}
            {% assign feedback_url = session.feedback_url | default: entry.feedback_url %}
            <tr>
                <td>{% if session %}{{ session.start_time }}-{{ session.end_time }}{% else %}{{ entry.start_time }}-{{ entry.end_time }}{% endif %}</td>
                <td>
                    {% if session %}
                      {% assign session_href = session.page_url | append: "#" | append: session.anchor | relative_url %}
                      {% capture calendar_title %}{% if session.category == "panel" %}Panel {{ session.number }}: {% elsif session.category == "tabletalk" %}Table Talk {{ session.number }}: {% endif %}{{ session.title }}{% endcapture %}
                      <div>
                        {% if session.category == "roundtable" %}
                          <a href="{{ session_href }}">{{ session.title }}</a>
                        {% elsif session.category == "panel" %}
                          <a href="{{ session_href }}">Panel {{ session.number }}:</a> {{ session.title }}
                        {% else %}
                          <a href="{{ session_href }}">Table Talk {{ session.number }}:</a> {{ session.title }}
                        {% endif %}
                      </div>
                    {% else %}
                      {% if entry.page_url contains '://' %}
                        {% assign session_href = entry.page_url %}
                      {% else %}
                        {% assign session_href = entry.page_url | relative_url %}
                      {% endif %}
                      {% assign calendar_title = entry.title %}
                      <div>{% if entry.page_url == "/schedule/" %}{{ entry.title }}{% else %}<a href="{{ session_href }}">{{ entry.title }}</a>{% endif %}</div>
                    {% endif %}
                    {% if site.show_feedback_forms and feedback_url %}
                      <div><a href="{{ feedback_url }}" target="_blank" rel="noopener">Feedback form</a></div>
                    {% endif %}
                </td>
                <td>{% if crowdcast_url %}<a href="{{ crowdcast_url }}" target="_blank" rel="noopener">Watch live</a>{% endif %}</td>
                <td><a href="https://calendar.google.com/calendar/render?action=TEMPLATE&text={{ calendar_title | uri_escape }}&dates={% if session %}{{ session.gcal_start }}%2F{{ session.gcal_end }}{% else %}{{ entry.gcal_start }}%2F{{ entry.gcal_end }}{% endif %}&ctz=Europe%2FParis&details=OHBM%20Open%20Science%20Room%202026%20session.%20Full%20schedule%3A%20{{ schedule_base_url | append: schedule_path | uri_escape }}&location={% if session_href contains '://' %}{{ session_href | uri_escape }}{% else %}{{ schedule_base_url | append: session_href | uri_escape }}{% endif %}" target="_blank" rel="noopener">Add to Google Calendar</a></td>
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
