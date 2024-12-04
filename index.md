---
title: Online gehostete Anweisungen
permalink: index.html
layout: home
---

# Data Analyst Übungen

Diese Übungen unterstützen den Microsoft Kurs [DP-500: DP-500T00: Entwerfen und Implementieren von Analyselösungen auf Unternehmensebene mit Microsoft Azure und Microsoft Power BI](https://docs.microsoft.com/training/courses/dp-500t00)

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/labs'" %}
| ILT Modul | Lab |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} – {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

<!--

## Demos

{% assign demos = site.pages | where_exp:"page", "page.url contains '/Instructions/Demos'" %}
| Module | Demo |
| --- | --- | 
{% for activity in demos  %}| {{ activity.demo.module }} | [{{ activity.demo.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
 
-->
