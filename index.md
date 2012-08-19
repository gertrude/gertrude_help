---
layout: page
title: Welcome to Gertrude
tagline: the #ant.org channel bot
---
{% include JB/setup %}

## gertrude is chrisb's bot

More accurately, gertrude is the channel bot for the [#ant.org channel on irc.z.je](irc://irc.z.je/#ant.org), and this site
is her online help system.

Gertrude has been performing mildly useful functions for the denizens of #ant.org since 1996, on and off. She was originally
conceived as a sort of FAQ system for Acorn's RC5 cracking team on distributed.net, if you remember any of those things.

Gertrude comprises a set of plugins that provide various utlity functions. Each of which has its own page:

<ul class="posts">  
{% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


