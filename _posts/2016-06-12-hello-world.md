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

<div style="display: flex;" >
<div style="flex-grow: 1;"> </div>
<div style="background-image: url('/images/iphone6.png'); width: 401px; height: 806px; ">
<iframe style=" margin-top: 92px; margin-left: 26px;" src="http://atoz-chevara.github.io/places-app/" width="349" height="617" scrolling="no" class="lazy-hidden"></iframe>
</div>
<div style="flex-grow: 1;"> </div>
</div>