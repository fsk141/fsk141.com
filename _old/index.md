---
layout: page
title: Fsk141.com
<!-- tagline: Supporting tagline -->
---
## Recent Posts

<div class="row-fluid">
<div class="span6">
	<ul class="posts">
		{% for post in site.posts limit:5 %}
		<li>
			<h4><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h4>
	    	<span>{{ post.date | date_to_string }}</span>
		</li>
		{% endfor %}
	</ul>
</div>
</div>

<div class="row-fluid">
<div class="span12">
	{% for category in site.categories %} 
		<h2 id="{{ category[0] }}-ref">{{ category[0] | join: "/" }}</h2>
		<ul>
			{% assign pages_list = category[1] %}  
		</ul>
	{% endfor %}
</div>
</div>
