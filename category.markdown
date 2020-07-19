---
layout: page
title: 目录
permalink: /Category/
---

{% for sec in site.data.list_of_category.section %}
- {{sec}}
  {%- for sub in site.data.list_of_category.mapping[sec] %}
  - {{sub}}
    {%- for ssub in site.data.list_of_category.mapping[sub] %}
    - {{ssub}}
      {%- for page in site.categories[ssub] %}
      - [{{page.title}}]({{ page.url }}) created at {{page.date | slice:0,10}}.
      {%- else %}
        (No posts in this category now.)
      {%- endfor %}
    {%- else %}
      {%- for page in site.categories[sub] %}
      - [{{page.title}}]({{ page.url }}) created at {{page.date | slice:0,10}}.
      {%- else %}
        (No posts in this category now.)
      {%- endfor %}
    {%- endfor %}
  {%- else %}
    {%- for page in site.categories[sec] %}
    - [{{page.title}}]({{ page.url }}) created at {{page.date | slice:0,10}}.
    {%- else %}
      (No posts in this category now.)
    {%- endfor %}
  {%- endfor %}
{%- endfor %}
