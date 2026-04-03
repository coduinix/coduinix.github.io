---
layout: single
title: Talks
permalink: /talks
---

{% for talk in site.talks %}
  <article class="talk-entry">
    <h2><a href="{{ talk.url }}">{{ talk.title }}</a></h2>
    <div class="abstract">{{ talk.content | markdownify }}</div>

    <h3>Presented at</h3>
    <ul>
      {% assign found_event = false %}
      {% for event in site.events %}
        {% for et in event.talks %}
          {% assign talk_id = et.id | default: et %}

          {% assign match = false %}
          {% if talk.id and talk.id == talk_id %}
            {% assign match = true %}
          {% elsif talk.title | slugify == talk_id %}
            {% assign match = true %}
          {% endif %}

          {% if match %}
            <li>
              <a href="{{ event.url }}">{{ event.title }}</a>
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

