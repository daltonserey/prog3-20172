---
---
# Programação 3 / Computação / UFCG, 2017.2

> Professor: [Dalton Serey](http://daltonserey..github.io)<br>
> Sala: CAA304 (ou CAA203)<br>
> Slack: [prog3ufcg.slack.com](http://prog3.ufcg.slack.com)

{% assign curso = site.data %}

{% for aula in curso.aulas %}
<h2>{{ aula.nome }} <small>({{ aula.dia }})</small></h2>
  <ul>
  {% for id in aula.licoes_ids %}
  <li>{{curso.licoes[id].titulo}} {% if curso.licoes[id].slides %} (<a href="{{curso.licoes[id].slides}}" target="_blank">slides</a>){% endif %}</li>
  {% endfor %}
  </ul>
{% endfor %}
