---
layout: page
title: 分类目录
permalink: /Category/
---

{% for sec in site.data.list_of_category.section %}
- {{ site.data.list_of_category.alias[sec] | default: sec }}
    {%- for sub in site.data.list_of_category.mapping[sec] %}
  - {{ site.data.list_of_category.alias[sub] | default: sub }}
        {%- for ssub in site.data.list_of_category.mapping[sub] %}
    - {{ site.data.list_of_category.alias[ssub] | default: ssub }}
            {%- for page in site.categories[ssub] %}
      - `{{page.date | date: "%Y-%m-%d"}}` [{{page.title}}]({{ page.url }})
            {%- else %}
        (此目录暂无博客。)
            {%- endfor %}
        {%- else %}
            {%- for page in site.categories[sub] %}
    - `{{page.date | date: "%Y-%m-%d"}}` [{{page.title}}]({{ page.url }})
            {%- else %}
      (此目录暂无博客。)
            {%- endfor %}
        {%- endfor %}
    {%- else %}
        {%- for page in site.categories[sec] %}
  - `{{page.date | date: "%Y-%m-%d"}}` [{{page.title}}]({{ page.url }})
        {%- else %}
    (此目录暂无博客。)
        {%- endfor %}
    {%- endfor %}
{%- endfor %}
