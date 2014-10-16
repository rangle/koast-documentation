---
layout: default
---
<div class="side-toc">
<ul class="page-small-list">
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
</div>
<div class="post">

  <header class="post-header">
    <h1 class="post-title">{{ page.title }}</h1>
  </header>

  <article class="post-content">
    {{ content }}
  </article>

</div>
