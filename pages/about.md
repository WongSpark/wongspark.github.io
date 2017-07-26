---
layout: page
title: About
description: coder,giser and almost a cooker.
keywords: Spark Wong
comments: true
menu: 关于
permalink: /about/
---

参加工作已经有一段时间了，此前一直都是处在‘狂奔’的状态，为学习新的技能，为追寻长久的方向，一路风风火火的过来，感觉也不错。

前进的感觉虽然让人着迷，但是，时常回顾一下来时路也是必要的。毕竟路走的太远就会迷失过去的自己。

这里是一个追寻者的博客，写技术文也写散文，写代码也写心情，写目标也写心路

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
