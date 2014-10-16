---
layout: default
---

<div class="home">
<!--
  <h1 class="page-heading">Posts</h1>

  <ul class="post-list">
    {% for post in site.posts %}
    <li>
      <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>

      <h2>
          <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
        </h2>
    </li>
    {% endfor %}
  </ul>
-->
<ul class='page-list'>
{% assign pages = site.pages | sort:"weight"  %}
{% for page in pages %}
  {% if page.title %}

  {% if page.top == false %}
  <li><a class="page-sub-link"  href="{{ page.url | prepend: site.baseurl }}">{{ page.title }}</a>
  {% else %}
  <li><a class="page-link"  href="{{ page.url | prepend: site.baseurl }}">{{ page.title }}</a></li>
  {% endif %}

    {% if page.anchors %}

      {% for an in page.anchors %}
        <li> <a class="page-sub-link" href="{{ page.url | prepend: site.baseurl }}#{{an[0]}}"> {{an[1]}}</a> </li>
      {% endfor %}

    {% endif %}

  {% endif %}
{% endfor %}
</ul>

  <p class="rss-subscribe">subscribe <a href="{{ " /feed.xml " | prepend: site.baseurl }}">via RSS</a>
  </p>

</div>
