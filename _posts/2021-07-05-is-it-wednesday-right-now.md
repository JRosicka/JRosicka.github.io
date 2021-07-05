---
layout: post
title: Is it Wednesday Right Now?
date:   2021-07-06 16:00:00
categories: 
---

{% assign todayDate = site.time | date: "%Y-%m-%d" %}
{% case todayDate %}
    {% when '2021-07-06' %}Yeah
    {% else %}Probably not
{% endcase %}
