---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

This page is not routinely maintained, please refer to my <a href="https://frivasa.github.io/files/vita.pdf" style="color: blue; text-decoration: underline;text-decoration-style: dotted;">CV</a> for a more up to date list. For a full list of publications I am an author on see my NASA ADS page <a href="https://ui.adsabs.harvard.edu/search/filter_author_facet_hier_fq_author=AND&filter_author_facet_hier_fq_author=author_facet_hier%3A%221%2FRivas%2C%20F%2FRivas%2C%20Fernando%22&fq=%7B!type%3Daqp%20v%3D%24fq_author%7D&fq_author=(author_facet_hier%3A%221%2FRivas%2C%20F%2FRivas%2C%20Fernando%22)&q=author%3A%22Rivas%2C%20Fernando%22&sort=date%20desc%2C%20bibcode%20desc&p_=0" style="color: blue; text-decoration: underline;text-decoration-style: dotted;">here</a>.

{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
