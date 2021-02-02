---
layout: page
title: Stock-股票
permalink: /stock/
sitemap: false
---

#This is Financial
##This is how I earn money

<div id="index">
    {% for category in categories %}
        {%if category[0] == "stock" }
            <a name="{{ category[0] }}"></a>
            <h2>{{ category[0] | replace:'-', ' ' }} ({{ category | last | size }})</h2>
            {% assign sorted_posts = site.posts | sort: 'title' %}
            {% for post in sorted_posts %}
                {%if post.categories contains category[0]%}
                    <h3><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}" title="{{ post.title }}">{{ post.title }} <p class="date">{{ post.date |  date: "%B %e, %Y" }}</p></a></h3>
                    <p>{{ post.excerpt | strip_html | truncate: 160 }}</p>
                {%endif%}
            {% endfor %}
        {%endif%}
    {% endfor %}
</div>