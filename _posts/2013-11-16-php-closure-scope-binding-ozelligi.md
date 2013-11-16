---
layout: post
title: "PHP Closure Scope Binding özelliği"
description: "PHP Closure Scope Binding özelliği"
modified: 2013-11-16
tags: [PHP]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

PHP'de `Closure` objelerinin scopelerini `bind` methodunu kullanarak değiştirebiliriz.

{% highlight php %}
<?php

class ClosureDispatcher
{

    public function call(Closure $closure, $scope = NULL) {
        if(!is_null($scope)) 
            $closure = Closure::bind($closure, $scope, $scope);
        
        return $closure();
    }
}

class Person
{
    protected $name = 'Aristona';
}

$dispatcher = new ClosureDispatcher();
$person     = new Person();

$ret = $dispatcher->call(function () {
        return $this->name; // Aristona
}, $person);

{% endhighlight %}

Kafa karıştırıcı bir konu gibi görünüyor ancak hiçte öyle değil!


