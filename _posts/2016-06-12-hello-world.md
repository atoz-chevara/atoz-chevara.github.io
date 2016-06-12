---
layout: post
title: Hello World
description: "Postingan Awal."
modified: 2016-06-13
tags: [hello]
image:
  background: triangular.png
---

Here be a sample post with a custom background image. To utilize this "feature" just add the following YAML to a post's front matter.

{% highlight yaml %}
image:
  background: filename.png
{% endhighlight %}

This little bit of YAML makes the assumption that your background image asset is in the `/images` folder. If you place it somewhere else or are hotlinking from the web, just include the full http(s):// URL. Either way you should have a background image that is tiled.

If you want to set a background image for the entire site just add `background: filename.png` to your `_config.yml` and BOOM --- background images on every page!

<iframe style="background-image: url('iphone6.png'); max-width: initial; padding: 93px 25px 95px 25px;  display:block; margin:auto;margin-top:30px; border:none;" src="http://atoz-chevara.github.io/places-app/" width="349" height="617" scrolling="no" class="lazy-hidden"></iframe>
