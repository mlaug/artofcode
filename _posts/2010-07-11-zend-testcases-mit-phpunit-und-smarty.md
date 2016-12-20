---
id: 39
title: Zend Testcases mit PHPUnit und Smarty
date: 2010-07-11T15:55:56+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=39
permalink: /2010/07/11/zend-testcases-mit-phpunit-und-smarty/
categories:
  - Php
  - Webentwicklung
tags:
  - Bug
  - Php
  - Smarty
  - Zend
---
[<img class="alignleft size-full wp-image-43" title="logo_small" src="http://www.artofcode.de/wp-content/uploads/2010/07/logo_small.gif" alt="" width="123" height="23" />](http://www.artofcode.de/wp-content/uploads/2010/07/logo_small.gif)Da ich ein großer Fan von der Smarty Template Engine bin, habe ich diese als Standard in alle meine Projekte, die auf Zend Framework basieren, eingebaut. Die Einrichtung ist relativ simpel und kann <a title="Anleitung für Smarty im Zend Framework" href="http://devzone.zend.com/article/120" target="_blank" class="broken_link">hier</a> nachvollzogen werden. Ein Problem scheinen jedoch die TestCases zu haben.
  
<!--more-->


  
Nachdem ich nämlich dazu übergegangen bin auch die Actions meiner Controller mit der dispatch Methode zu testen, bekomme ich immer ein und den selben Fehler

```Fatal error: Call to a member function assign() on a non-object```

Hier scheint das View Objekt, welches ich in der Bootstrap mit meiner Smarty_View Klasse ersetze, nicht mehr vorhanden zu sein.

```php
$viewRenderer = Zend_Controller_Action_HelperBroker::getStaticHelper('ViewRenderer');
        $viewRenderer->setView($view)
                     ->setViewBasePathSpec($view->getEngine()->template_dir)
                     ->setViewScriptPathSpec(':controller/:action.:suffix')
                     ->setViewScriptPathNoControllerSpec(':action.:suffix')
                     ->setViewSuffix('htm');
```

Aus Php5.2 kannte ich ja noch das Problem, das häufig Referenzen verloren gehen, habe aber schon längst auf Php5.3 umgestellt. Hier scheint das Problem also nicht zu liegen. Nach einiger Recherche im inneren von Zend und ohne das Problem zu lösen, habe ich auf folgendem Umweg die Testcases zum Laufen gekriegt. Dazu habe ich die init Methode der Controller um folgenden Code erweitert

```php
if ( $this->view == null ){
    $rend = Zend_Controller_Action_HelperBroker::getStaticHelper('ViewRenderer');
    $this->view = $rend->view;
}
```

Jetzt laufen die TestCases wieder und ich kann ohne Problem auf die Dispatch Methode zurück greifen. Der Fehler kommt übrigens **nicht**, wenn man zuvor in der Bootstrap die oben dargestellten Code nicht ausführt, also den Standard ZendView unangetastet lässt.