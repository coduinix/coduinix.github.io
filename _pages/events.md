---
layout: single
title: Events
permalink: /events
---

{% for event in site.events %}
  <section class="event-entry">
    <h2><a href="{{ event.url }}">{{ event.title }}</a></h2>
    <p class="event-dates">
      {% if event.start_date %}
        {{ event.start_date }}{% if event.end_date and event.end_date != event.start_date %} – {{ event.end_date }}{% endif %}
      {% endif %}
    </p>

    {% if event.talks %}
      <ul>
        {% for et in event.talks %}
          {% assign talk_id = et.id | default: et %}

          {% assign talk_page = site.talks | where: "id", talk_id | first %}
          {% if talk_page == nil %}
            {% assign talk_page = site.talks | where: "title", talk_id | first %}
            {% if talk_page == nil %}
              {% for t in site.talks %}
                {% if t.title | slugify == talk_id %}
                  {% assign talk_page = t %}
                  {% break %}
                {% endif %}
              {% endfor %}
            {% endif %}
          {% endif %}

          <li>
            {% if talk_page %}
              <a href="{{ talk_page.url }}">{{ talk_page.title }}</a>
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
