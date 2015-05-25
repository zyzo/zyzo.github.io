---
layout: post
title:  "FOSSASIA API calendar service"
date:   2015-05-25 00:00:00
categories: gsoc fossasia
tags:
- iCalendar
- php
comments: true
---

ICalendar is a file format residing under [RFC 5445](https://tools.ietf.org/html/rfc5545), to represent and exchange calendaring and scheduling information. The file extension is generally `.ics`, and `.iCal` for Apple software. The aim was to define a standard format that could be understood everywhere, independent of calendar vendors : Google Calendar, Apple iCal, Yahoo! Calendar, Mozilla Lightning, Microsoft Outlook... Nevertheless, true interoperability doesn't seem to be the reality. My next task at FOSSASIA is to this circumvent this situation and create a calendar service / vizualisation from ICalendar sources provided by different open-source communities, hence probably from different vendors and format deviations.

To have an idea of the problem I'm dealing with, I gathered a bunch of ics files, mostly [provided by freifunk communities](https://github.com/freifunk/directory.api.freifunk.net/blob/master/directory.json), using these little [node scripts](https://github.com/zyzo/api-collector), and try to spot a not-so-standard implementation. To have a less mechanical approach, I developed a visual tool to debug my ics. Here are some screenshots of the app:

<img src="/images/ics-debugging-tool-2.png" alt="Ics debugging tool screenshot 1" title="Parser input, general info panel and the heatmap" style="
    width: 65%;
">  *<span style="float : right;">Screenshot 1</span>*

<span style="float:right">*Screenshot 2*</span>

![Ics debugging tool screenshot 1](/images/ics-debugging-tool-1.png "Detailed calendar")

The tool is fairly simple : you copy the ics text into the text area (or choose between example files), press `Feed me !`, the system will get running and provide some outputs. There is a general info panel, a yearly heatmap, and a calendar. It is a great help for me since I can just switch between ics files, see all the events counts in the heatmap, and event detailed information in the calendar.

Source code & demo version coming soon ! Built with ics parser library [https://github.com/johngrogg/ics-parser](https://github.com/johngrogg/ics-parser), CalHeatmap [https://kamisama.github.io/cal-heatmap/](https://kamisama.github.io/cal-heatmap/) and jquery plugin [FullCalendar.io](http://fullcalendar.io/).

References : 

+ [Freifunk community map](http://freifunk.net/wie-mache-ich-mit/community-finden/)
+ [http://blog.fossasia.org](http://blog.fossasia.org)
+ [https://github.com/johngrogg/ics-parser](https://github.com/johngrogg/ics-parser)
+ [https://kamisama.github.io/cal-heatmap/](https://kamisama.github.io/cal-heatmap/)
+ [FullCalendar.io](http://fullcalendar.io/)