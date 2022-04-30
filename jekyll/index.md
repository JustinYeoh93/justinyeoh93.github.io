# Index
<ul>
{% for post in site.categories.jekyll %}
    <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
{% endfor %}
</ul>