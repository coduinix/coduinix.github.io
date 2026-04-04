---
layout: single
classes: wide
title: Events
permalink: /events
---

<div class="entries-list">

{% assign sorted_events = site.events | sort: 'start_date' | reverse %}
{% for event in sorted_events %}
  <div class="list__item">
    <h2><a id="{{event.slug}}"></a>{{ event.title }}</h2>
    <p class="page__meta">
      {% if event.start_date %}
        <span class="page__meta-date">
        <i class="fas fa-calendar" aria-hidden="true"></i> {{ event.start_date }}{% if event.end_date and event.end_date != event.start_date %} – {{ event.end_date }}{% endif %}
        </span>
      {% endif %}
      {% if event.location %}
        <span class="page__meta-location">
        &nbsp; <i class="fas fa-map-pin" aria-hidden="true"></i> {{ event.location }}
        </span>
      {% endif %}
    </p>

    {% if event.talks %}
      <ul>
        {% for et in event.talks %}
          {% assign talk_id = et.id | default: et %}

          {% assign talk_page = site.talks | where: "slug", talk_id | first %}

          <li>
            {% if talk_page %}
              <a href="/talks#{{ talk_id }}">{{ talk_page.title }}</a>
            {% else %}
              {{ talk_id }}
            {% endif %}

            {% if et.resources %}
              {% include resource-links.html resources=et.resources %}
            {% endif %}
          </li>
        {% endfor %}
      </ul>
    {% endif %}
  </div>
{% endfor %}
</div>