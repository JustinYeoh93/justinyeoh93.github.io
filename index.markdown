---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---
<link rel="shortcut icon" type="image/x-icon" href="favicon.ico">

# Introduction
This is a blog post where I store my lessons learnt while diving into the realm of software engineering.

I will mostly cover stuff in regards to backend and slowly moving towards more exciting technologies such as AR/VR and blockchain.

Below is the high level overview of the timeline I'm looking at.

|Year|Direction|
|-|-|
|2021|Infrastructure IaC CI/CD|
|2022|Backend, AI, IoT|
|2023|Blockchain, AR/VR|
|2024~|Product design, MVP development|

Favicon by: <a target="_blank" href="https://icons8.com/icon/80671/screwdriver">Screwdriver</a> icon by <a target="_blank" href="https://icons8.com">Icons8</a>

# Posts
## GoLang
<ul>
{% for post in site.categories.golang %}
    <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
{% endfor %}
</ul>

## Jekyll
<ul>
{% for post in site.categories.jekyll %}
    <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
{% endfor %}
</ul>

## Python
<ul>
{% for post in site.categories.python %}
    <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
{% endfor %}
</ul>


