---
layout: default
---

{% include docs.md %}

---

# <a name="tools"/></a> Tools

<ul>
{% for tool in site.data.tools %}
  <li><a href="{{ tool.url }}">{{ tool.name }}</a></li>
{% endfor %}
</ul>

---

# <a name="ports"/></a> Microservices addresses

<table class='table'>
  <tbody>
  {% for project in site.data.projects %}
    <tr>
      <td><a href="http://apps.{{ site.domain }}:{{ project.port }}">{{ project.name }} (port {{ project.port }})</a></td>
      <td><a href="http://apps.{{ site.domain }}:{{ project.port }}/swagger/index.html">Swagger</a></td>
      <td><a href="http://jenkins.{{ site.domain }}/job/{{ project.name }}/">Jenkins</a></td>
    </tr>
  {% endfor %}
  </tbody>
</table>

