---
layout: page
title: Samples
permalink: /samples/
---
{% for collection in site.collections %}
  {%if collection.label == "samples" %}
    {% for sample in collection.docs %}
### [{{ sample.title }}]({{ sample.url | relative_url }})
    {% endfor %}
  {% endif %}
{% endfor %}
