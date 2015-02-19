---
layout: default
---

{% include docs.md %}

# Tools

<ul>
{% for tool in site.data.tools %}
  <li><a href="{{ tool.url }}">{{ tool.name }}</a></li>
{% endfor %}
</ul>

# Microservices addresses

<table class='table'>
  <tbody>
  {% for project in site.data.projects.projects %}
    <tr>
      <td><a href="http://0.0.0.0:{{ project.port }}">{{ project.name }} (port {{ project.port }})</a></td>
      <td><a href="http://0.0.0.0:{{ project.port }}/swagger/index.html">Swagger</a></td>
      <td><a href="#">Jenkins</a></td>
    </tr>
  {% endfor %}
  </tbody>
</table>

