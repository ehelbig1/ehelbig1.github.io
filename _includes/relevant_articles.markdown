{% assign category = include.category %}
{% assign tag = include.tag %}
{% title = include.title %}

# Relevant Articles
<ul style="list-style-type: none;">
  {% for post in site.posts %}
    {% if post.category == category and post.tag == tag and post.title != title %}
      <li>
        <a href="{{ post.url }}">
          {{ post.title }}
        </a>
      </li>
    {% endif %}
  {% endfor %}
</ul>