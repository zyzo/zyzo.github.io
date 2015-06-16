---
layout: post
title:  "Get Facebook comments with large user pictures"
date:   2015-06-17 02:02:00
categories: facebook-graph-api
comments: true
tags: 
- php
---

On the `/user` edge, user pictures url are not returned by default, and when users ask for it, the response image is not in biggest format. This query should get the *largest* user pictures associated with post :

{% highlight php %}
	/user{picture.type(large)}
{% endhighlight %}

There are other use cases where this syntax applies as well

* get comments of a feed

{% highlight php%}
	/feed?fields='from{picture.type(large)}'
{% endhighlight %}

* get feeds of an user 

{% highlight php%}
	/userid/feed?fields={from{picture.type(large)}}
{% endhighlight %}

Examples tested with Facebook Graph API > v2.0, using [Graph Explorer](https://developers.facebook.com/tools/explorer/145634995501895/?method=GET&path=me%3Ffields%3Did%2Cname) and [PHP SDK](https://developers.facebook.com/docs/reference/php/4.0.0).
