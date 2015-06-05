---
layout: post
title:  "Calendario : another JS calendar library"
date:   2015-06-05 17:37:00
categories: gsoc fossasia
tags:
- calendario
- javascript
comments: true
---

Calendario is a lightweight [calendar library](https://github.com/codrops/Calendario) that I happen to find on the net. Their approach to calendaring solution is rather unique : it's not a full-blown library like jQuery [FullCalendar](http://fullcalendar.io/) where everthing is ready to be used. With Calendario, calling init function simply generates plains html source code. The styling part is completely up to user.

Another plus is it's lightweight (350 lines of code more or less). It's easy to dive into the source and make advanced adaptations to specific use-case.

But what's really remarkable is their examples. As I said the styling is out-sourcing to user, and they provide [stunning examples](http://deviprsd21.github.io/Calendario/) that demonstrate how to do it. For me (a developer "designing" stuffs), readily beautiful examples are a crucial selling point.

But (there's always a but..), Calendario is very poorly documented. A pointer to the same [blog post](http://tympanus.net/codrops/2012/11/27/calendario-a-flexible-calendar-plugin/) all over again is not enough ! I was very confusing about how things work at first, especially about the data model. This blog contains what I found out (rather painfully) about Calendario library : 

### Initialization

{% highlight html %}
<div id="calendar_id"></div>
<script>
	$('#calendar_id').calendario(options);
</script>
{% endhighlight %}

### Available options

| Option        | Purpose*                        | Default  |
| ------------- |:---------------------------------:| -----:|
| weeks      | weekday labels | ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
| weekabbrs      | weekday short labels | ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'] |
| months |  month label   |    ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'] |
| monthabbrs | monday short labels | ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'] |
| displayWeekAbbr | display week short labels | false |
| displayMonthAbbr | display month short labels | false |
| startIn | | 1 |
| fillEmpty | | true |
| zone | | '00:00' |
| events | |  [['click'], [focus']]|
| checkupdate | | true |
| weekdays | | 'MON, TUE, WED, THU, FRI'|
| weekends | | 'SAT, SUN' |
| feed | | http://calendario.t15.org/sync/ |
| month | | |
| year | | |
| caldata | calendar events data | |

<br/>
Most of them you can safely ignore, I enlisted them all for the sake of completeness. Checkout the next section for interesting ones.

### Option format
caldata

A list of json objects. The list's keys and values contain following information :  

* key : the event date in MM-DD-YYYY format
* value : an array of event values that occured on the given date. Each case of the array can have following fields :

| Option | Purpose | Value type |
| content|  title of the event | string |
| url | Link to event page | string |
| note | added note for the event, will be included in the html as inside <note> tag | string |
| startday | | string, in MM-DD-YYYY format |
| endday | |  string, in MM-DD-YYYY format | 
| allDay| true for all day event| boolean |

<br/>
<u>Examples</u>
{% highlight javascript %}
var events = {
	'06-06-2015' : [{content: 'Important Meeting at 4', startTime: '04:00', endTime: '06:00', note : 'Everyone please gather at Markketing Department'}],
	'06-07-2015' : [{content : 'First event of July 7', allDay : true}, 
	                {content : 'Second event of July 7', allDay : true}
	               ],
	'07-01-2015' : [{content: 'All day event', allDay: true, note : 'So loongg !'}]
}
{% endhighlight %}

### Wrap-up : Pros and cons of Calendario
* Pros :

	* lightweight
	* fully customizable design
	* beautiful examples

* Cons

	* poor documentation
	* lots of work undone, not recommended for those who want a powerful library that just work

That's about it ! Found something wrong, or just want to help completing this unofficial calendario documentation ? Send me a PR [here](https://github.com/zyzo/zyzo.github.io)

***NOTE** : this documentation is based on Calendario v4.0.0 [https://github.com/deviprsd21/Calendario](https://github.com/deviprsd21/Calendario)

References :

+ [My experiments for FOSSASIA community timeline, powerered by Calendario](https://github.com/zyzo/EventsPanel)
+ [http://blog.fossasia.org](http://blog.fossasia.org)