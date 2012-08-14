---
layout: page
title: Welcome to Gertrude
tagline: the #ant.org channel bot
---
{% include JB/setup %}

## Welcome to gertrude

This is the online help for gertrude - gertrude is the channel bot for the [#ant.org channel on irc.z.je](irc://irc.z.je/#ant.org). 

Gertrude comprises a set of plugins that provide various utlity functions. Each of which has its own page:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



