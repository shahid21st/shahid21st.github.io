---
layout: page
title: Tags
permalink: tags/
---

<h2>{{page.title}}</h2>

{% comment %}
=======================
The following part extracts all the tags from your posts and sort tags, so that you do not need to manually collect your tags to a place.
=======================
{% endcomment %}
{% assign rawtags = "" %}
{% for post in site.posts %}
    {% assign ttags = post.tags | join:'|' | append:'|' %}
    {% assign rawtags = rawtags | append:ttags %}
{% endfor %}
{% assign rawtags = rawtags | split:'|' | sort %}



{% comment %}
=======================
The following part removes dulpicated tags and invalid tags like blank tag.
=======================
{% endcomment %}
{% assign tags = "" %}
{% for tag in rawtags %}
    {% if tag != "" %}
        {% if tags == "" %}
            {% assign tags = tag | split:'|' %}
        {% endif %}
        {% unless tags contains tag %}
            {% assign tags = tags | join:'|' | append:'|' | append:tag | split:'|' %}
        {% endunless %}
    {% endif %}
{% endfor %}


{% for tag in tags %}
  <h3 id="{{ tag | escape }}">{{ tag }}</h3>
  <ul>
  {% for post in site.tags[tag] %}
    <li><a href="{{site.baseurl}}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
  </ul>
{% endfor %}
