---
layout: post
title:  "Explore PHP pthreads bugs"
date:   2015-05-01 08:16:00
categories: php 
comments: true
tags:
- php
- pthreads
---

I'm developing a little utility library called [meteor-ddp-php](https://github.com/zyzo/meteor-ddp-php), which makes heavily use of PHP [pthreads](https://github.com/krakjoe/pthreads) library. Once in a while, I encounters weird, unexplicable bugs that contradicts all my knowledge about computer & programming languages. They just really get me on my nerves. I want to share with you that rare experience in this post.

Suppose I have a class `B` extending `Thread` and a class `A` which initiates an instance of `B` and starts the child thread that `B` represents. Finally, I create an instance of `A` and get them all running :

*test.php*
{% highlight php %}
<?php
class B extends Thread {
    public function run() {
        $i = 100000000;
        while($i--);
        echo 'I\'m feeling lucky' . PHP_EOL;
    }
}

class A {
    public function startB() {
        $b = new B();
        $b->start();
    }
}

$a = new A();
$a->startB();
echo 'Be what\'s next' . PHP_EOL;
{% endhighlight %}

Logically, what I expected is (notice I put a considerable delay in `B::run()` - I didn't use `sleep` because it is not thread-safe) :

	Be what's next
	I'm feeling lucky

Instead, this is what I got by running `php test.php` : 

	I'm feeling lucky
	Be what's next

That's just weird ! Apparently my child thread is blocking my parent thread, until it terminated. But wait for it. If I move the the `echo` line to the end of `startB()` method :

{% highlight php startinline=true %}
class A {
    public function startB() {
        $b = new B();
        $b->start();
        echo 'Be what\'s next' . PHP_EOL;
    }
}
{% endhighlight %}

it works just fine. Seems like the problem is not that `$b->start()` is blocking, but the blocking point is rather at the return of `startB()` function ! That's the first crazy thing.

Now, just for fun, how about I changed the variable `$b` in `startB()` from function-scope to object-scope instead ? It shouldn't make any difference right ?

{% highlight php startinline=true %}
...
class A {
	private $b;
    public function startB() {
        $this->b = new B();
        $this->b->start();
    }
}
$a = new A();
$a->startB();
echo 'Be what\'s next' . PHP_EOL;
{% endhighlight %}

Well, it did. The example works as expected, with this little *fix*. That's the second crazy thing. The scope of the variable shoudn't have anything to do with the blocking thread problem.

I must admit I didn't go over 300+ issues on the github page, and on one of them may lie the answer for all this madness. But still, these problems just demonstrate how frustrating and dangerous threads in PHP could be. Visit [here](https://github.com/zyzo/meteor-ddp-php/commits/devel) for real-life example.

