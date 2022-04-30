# List of Posts
## Jekyll
<ul>
{% for post in site.categories.jekyll limit %}
    <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
{% endfor %}
</ul>