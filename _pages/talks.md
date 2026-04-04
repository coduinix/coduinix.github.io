---
layout: single
classes: wide
title: Sessions
permalink: /talks
---

{% for talk in site.talks %}
  <article class="talk-entry">
    <h2><a id="{{talk.slug}}"></a>{{ talk.title }}</h2>
    <div class="abstract">{{ talk.content | markdownify }}</div>

    <h3>Presented at</h3>
    <ul>
      {% assign found_event = false %}
      {% assign sorted_events = site.events | sort: 'start_date' | reverse %}
      {% for event in sorted_events %}
        {% for et in event.talks %}
          {% assign talk_id = et.id | default: et %}

          {% assign talk_short_id = talk.id | split: "/" | last %}
          {% assign match = false %}
          {% if talk_short_id == talk_id %}
            {% assign match = true %}
          {% endif %}

          {% if match %}
            <li>
              <a href="/events#{{ event.slug }}">{{ event.title }}</a>
              {% if et.resources %}
                {% include resource-links.html resources=et.resources %}
              {% endif %}
            </li>
            {% assign found_event = true %}
          {% endif %}
        {% endfor %}
      {% endfor %}

      {% unless found_event %}
        <li>— no recorded events yet</li>
      {% endunless %}
    </ul>
  </article>
{% endfor %}

