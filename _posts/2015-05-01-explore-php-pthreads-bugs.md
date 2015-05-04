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

I'm developing a utility library called [meteor-ddp-php](https://github.com/zyzo/meteor-ddp-php), which makes heavy use of PHP [pthreads](https://github.com/krakjoe/pthreads). Once in a while, I encounters weird, unexplainable bugs that contradicts all my knowledge about computer & programming languages. They just really get me on my nerves. I want to share with you that experience in this post.

Suppose I have a class `B` extending `Thread` and a class `A` which initiates and starts `B`:

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

Logically, the log I expected is (notice I put a considerable delay in `B::run()` - I didn't use `sleep` because it is not thread-safe) :

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

it works just fine. Seems like the problem is not that `$b->start()` is blocking, but the blocking point is rather at the return of `startB()` function ! 

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

Well, it did. The example works as expected, with this little *fix*. That's just crazy ! The scope of the variable shoudn't have anything to do with the blocking thread problem.

Using the second fix between these two, I dodged the bullet and go on with my application.

I must admit I didn't go over [300+ issues](https://github.com/krakjoe/pthreads/issues) on the github page, and in one of them may lie the answer for all this madness. But still, these problems just demonstrate the myth of multithreading in PHP. [PHP isn't anywhere near thread-safe](https://neosmart.net/blog/2008/dont-believe-the-lies-php-isnt-thread-safe-yet/) yet, even at basic language blocs like function call or variable scope it seems. Working with pthreads requires extreme care and patience. Visit [here](https://github.com/zyzo/meteor-ddp-php/commit/71df57680954260bcc6dadfc7b244fba4bce895b) for real-life example.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             

