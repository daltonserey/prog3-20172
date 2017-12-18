---
---
# Programação 3 / Computação / UFCG, 2017.2

> Professor: [Dalton Serey](http://daltonserey..github.io)<br>
> Sala: CAA304 (ou CAA203)<br>
> Slack: [prog3ufcg.slack.com](http://prog3.ufcg.slack.com)

{% assign curso = site.data %}

{% for licao in curso.licoes %}
<h2>{{ licao.titulo }}</h2>
  <li>slides {{licao.slides }} 
  </ul>
{% endfor %}
