---
layout: post
title:  "A glance at FOSSASIA Calendar API"
date:   2015-06-16 22:57:00
categories: gsoc fossasia
tags:
- api
- php
comments: true
---

I am a frequent user of popular social platforms API : Facebook, Twitter, Instagram.. Among others, Facebook Graph API is the API I'm most familiarized with, and despite its lack of documentation, [Graph API](https://developers.facebook.com/docs/graph-api) makes me from time to time admire the engineering mind of those who built it. It's simple, has a clear design, and communicates a consistent philosophy. I inspired a lot from it to implement [Calendar API](http://api.fossasia.net/ics-collector/CalendarAPI.php?source=all), a simple web service at FOSSASIA.

Why naming it API, and not web service ? I don't know, I was always confused about the two, and as far as I concerns, they are the same. The only difference might be the word 'web' that tights web service to HTTP, and the scenario involving two distant machines, while API is somewhat more general (an API call of an in-app library for example).

This is my first time building a web service, and so far there wasn't any technical challenge : it's just simple & straightforward PHP script with `echo $results;` in the end. Mainly because the hard part is still ahead : getting people to know about it, and maintaining it while there are (hopefully) enough clients who care to yell at API developers when they do something wrong. The API I built supposed to parse the merged ics file of all communities event feeds, and expose it to users, in various formats, with filters and options.

Don't worry if the last sentence doesn't make sense. We'll dive in the details right away.

There are currently two possible formats : `.ics` and `.json`. Users set the format with `format` parameter, for e.g. `format=ics`. `ics` format is suited for developers who want to import events to their calendar app / wordpress plugin. `json` format is for front-end developers who want to build a web / mobile web without the hassle of parsing ics feed.

A few filter parameters are possible to refine search results : `source` to filter by community,
`from`, `to` to filter by events time, `limit` to limit number of events returned, and `fields` to custom select event fields. Finally, `sort` parameter is to order the results by certain criteria. Currently only datetime ordering (ascending or descending) are available.

As I said, the Calendar API is inspired a lot from Facebook API. Those are the ideas I pickpocketed from their design :

* multiple values parameter. With `source` and `fields`, multiple values can be provided, separated using comma.
* `fields` parameter. The motivation for event field filter is because the event has lots of different fields, and different users are interested in different information.

There is also the "linked edge" characteristic of Graph API that I find very interesting : connection between related data make them more useful. It is done that way mainly because the API reflects Facebook own data model, but there is nothing thats prevents unconnected data to be presented in a connected way. So I'll hope to implement something similar for FOSSASIA Calendar API in the futur.

(Right now the API serves events from Freifunk community, not FOSSASIA, mostly because there are not enough event feeds from FOSSASIA community to collect. So if you want to help us, you can add ics feed of your open-source community to FOSSASIA API. [Here's how](https://github.com/fossasia/common.api.fossasia.net/blob/dev/ics-collector/API.md#how-it-works-)).

References :

+ FOSSASIA Calendar API : [http://api.fossasia.net/ics-collector/CalendarAPI.php?source=all](http://api.fossasia.net/ics-collector/CalendarAPI.php?source=all)
+ [http://blog.fossasia.org](http://blog.fossasia.org)