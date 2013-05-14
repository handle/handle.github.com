---
layout: page
title: 贷成的首页
tagline: 夫唯道，善贷且成！
---
{% include JB/setup %}

## 个人简介
    
    author :贷成
    email : handleyan@gmail.com
    github : handle
    
## 我的博文

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## To-Do

This theme is still unfinished. If you'd like to be added as a contributor, [please fork](http://github.com/plusjade/jekyll-bootstrap)!
We need to clean up the themes, make theme usage guides with theme-specific markup examples.


