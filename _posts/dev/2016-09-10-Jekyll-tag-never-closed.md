---
title: Liquid syntax error - 'highlight' tag was never closed
layout: post
category: [dev, Jekyll]
---

- 해결책
	- `config.yml`
    {% highlight yaml %}
        excerpt_separator: "" 
    {% endhighlight %} 


[credit: Slaks](http://blog.slaks.net/2013-08-09/jekyll-tag-was-never-closed/)