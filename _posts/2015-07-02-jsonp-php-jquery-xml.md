---
layout: post
title:  "How to implement JSONP to download XML resource"
date:   2015-07-09 17:05:00
categories: gsoc fossasia php
tags:
- jsonp
- fossasia-api
comments: true
---

According to the same-origin security policy, web page can freely share images, stylesheets, scripts... but web fonts and AJAX requests are only accessible from the same domain.

Sometimes this is not desired behaviour, such as when getting a json / xml file from a different domain, using AJAX. In such cases, it all comes down to two solutions :

1- The source sets its header to allow specific domain cross-origin resource sharing (CORS). In PHP, this is done with `header` function :

{% highlight php %}
<?php
header('Access-Control-Allow-Origin: *');
{% endhighlight %}

2- **Use JSONP**.

JSONP is short for JSON padding, a well-known "workaround" to solve CORS. It only works with GET.


To send AJAX requests using JSONP, the server must first knows how to respond to JSONP requests. It usually detects JSONP requests using `callback` parameter.

{% highlight php linenos=table %}
<?php
// compute result
[...]

// output result
JsonpHelper::outputXML($result);

// json helper class - credits : @andibraeu
class JsonpHelper {
	private static function is_valid_callback($subject)
	{
	    $identifier_syntax
	      = '/^[$_\p{L}][$_\p{L}\p{Mn}\p{Mc}\p{Nd}\p{Pc}\x{200C}\x{200D}]*+$/u';
	    $reserved_words = array('break', 'do', 'instanceof', 'typeof', 'case',
	      'else', 'new', 'var', 'catch', 'finally', 'return', 'void', 'continue', 
	      'for', 'switch', 'while', 'debugger', 'function', 'this', 'with', 
	      'default', 'if', 'throw', 'delete', 'in', 'try', 'class', 'enum', 
	      'extends', 'super', 'const', 'export', 'import', 'implements', 'let', 
	      'private', 'public', 'yield', 'interface', 'package', 'protected', 
	      'static', 'null', 'true', 'false');
	    return preg_match($identifier_syntax, $subject)
	        && ! in_array(mb_strtolower($subject, 'UTF-8'), $reserved_words);
	}

	public static function outputXML($string) {
		# JSON if no callback
		if( ! isset($_GET['callback']) )
		    exit($string);
		$string = str_replace("\n", " ", str_replace("'", '"', $string));
		# JSONP if valid callback
		if(JsonpHelper::is_valid_callback($_GET['callback']))
		    exit("{$_GET['callback']}('$string')");
		# Otherwise, bad request
		header('status: 400 Bad Request', true, 400);
	}
}
{% endhighlight %}

Line 29 is dedicated to replace all single-quote characters to double-quote, and new lines to simple space. This is to be able to pass xml document as valid javasript string parameter. 

Then, the client can use jQuery to get the document from a different domain (it works on same domain as well, which is convenient for testing & development) :

{% highlight javascript linenos=table %}
$.ajax({
	url : source_url,
	dataType : 'jsonp',  // if dataType is set to 'jsonp' 
			    // a 'callback' parameter will be appended to the request url
	success: function(data) {
		$data = $($.parseXML(data));
		items = $data.find('item');
	}
});
{% endhighlight %}

Something worth noticing on the client side is on line 5 : the data needs to be converted to xml first, with `$.parseXML` before converting to jQuery object. Converting the result string directly to jQuery won't work : <span style='text-decoration : line-through'>`$data = $(data)`;</span>.

That's about it ! To know more about JSONP, please refer to its wiki <a href="https://en.wikipedia.org/wiki/JSONP">https://en.wikipedia.org/wiki/JSONP</a>.

References :

+ [http://blog.fossasia.org](http://blog.fossasia.org)
