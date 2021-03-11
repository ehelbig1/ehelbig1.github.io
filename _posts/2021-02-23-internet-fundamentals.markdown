---
layout: post
title:  "How the Internet Works"
date:   2021-02-23 20:17:24 -0500
category: internet
tag: fundamentals
---

&nbsp;
&nbsp;

***

# Summary

Browsing the internet may seem like a simple task but under the hood it is actually quite complicated.
Having a basic understating of how the internet operates under the hood is fundamental for working in the
software industry.

![The Internet](/assets/internet.jpg)

At a 10,000 foot view, the internet is nothing more than a group of computers spread across the world.
These computers are able to send and receive messages, often referred to as packets, from each other.
Similar to the postal service, each computer is given an address where packets can be sent. These addresses
are called <em>Internet Protocol (IP)</em> addresses and are represented by four numbers separated by a period.
For example **127.0.0.1**.

&nbsp;
&nbsp;

***

# Clients and Servers


&nbsp;
&nbsp;

***

# Relevant Articles
<ul style="list-style-type: none;">
  {% for post in site.posts %}
    {% if post.category == page.category and post.tag == page.tag and post.title != page.title %}
      <li>
        <a href="{{ post.url }}">
          {{ post.title }}
        </a>
      </li>
    {% endif %}
  {% endfor %}
</ul>