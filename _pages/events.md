---
layout: single
title: Events
permalink: /events
---

{% for event in site.events %}
  <section class="event-entry">
    <h2><a id="{{event.slug}}"></a>{{ event.title }}</h2>
    <p class="event-dates">
      {% if event.start_date %}
        {{ event.start_date }}{% if event.end_date and event.end_date != event.start_date %} – {{ event.end_date }}{% endif %}
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
  </section>
{% endfor %}
