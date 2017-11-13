---
layout: page
title: Writing
---
<ul class="posts">
  {% for post in site.posts %}
  {% endfor %}
    <!-- {% unless post.next %}
      <h3>{{ post.date | date: '%Y' }}</h3>
    {% else %}
      {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
      {% if year != nyear %}
        <h3>{{ post.date | date: '%Y' }}</h3>
      {% endif %}
    {% endunless %} -->
  

    {% for category in site.categories %}
      {% capture proname %}{{ category | first }}{% endcapture %}
      {% if proname != 'project' %}
        <h2 style="margin-top:50px;"><span class="label label-info">{{ category | first }}</span></h2>
        {% for post in category.last %}
          
          <li itemscope>
            <a href="{{ site.github.url }}{{ post.url }}">{{ post.title }}</a>
            <span style="font-size:12px;margin-left:15px;color:#c3c3c3">{{ post.date | date:"%Y-%m-%d"}}</span>
            <p class="post-date"><span><i class="fa fa-calendar" aria-hidden="true"></i> {{ post.date | date: "%B %-d" }} - <i class="fa fa-clock-o" aria-hidden="true"></i> {% include read-time.html %}</span></p>
          </li>
          
        {% endfor %}
      {% endif %}
    {% endfor %}

  
</ul>
