---
id: 417
title: Zend und Load Balancer
date: 2010-11-05T21:11:13+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=417
permalink: /2010/11/05/zend-und-load-balancer/
categories:
  - Php
  - Webentwicklung
  - Zend
tags:
  - Load Balancer
  - Zend
---
Letztens habe ich einen Load Balancer mit Apache Proxy aufgebaut. Die Software die darunter laufen sollte war mit dem Zend Framework entwickelt. Bei Zend gibt es nun auch <a href="http://framework.zend.com/manual/de/zend.controller.router.html#zend.controller.router.routes.hostname" target="_self" class="broken_link">Hostname basierte Routen</a>, die für einige Subseiten eingesetzt wurden. Als die ganze Umgebung in einen Loadbalancer umgezogen ist zeigte sich jedoch nur ein Verhalten: Hostbasierte Routen funktionierten nicht! Warum?

Ganz einfach! Zend berücksichtige bei seiner Funktion _getHttpHost_ in der Klasse _Zend\_Controller\_Request_Http_ nicht den vom Proxy gesetzten HTTP\_X\_FORWARDED\_HOST Wert in der $\_SERVER. Dieser beinhaltet beim Load Balancing den ursprünglichen Host und nicht den des Load Balancers. Damit auch im Load Balancing die Hostname Routen funktionieren habe ich die Funktion wie folgt umgeschrieben

```php
public function getHttpHost()
    {

        /**
         * @fix: didn't you thought about load balancing????
         */
        $host = $this-&gt;getServer('HTTP_X_FORWARDED_HOST');

        if (!empty($host)) {
            return $host;
        }

        $host = $this-&gt;getServer('HTTP_HOST');

        if (!empty($host)) {
            return $host;
        }

        $scheme = $this-&gt;getScheme();
        $name   = $this-&gt;getServer('SERVER_NAME');
        $port   = $this-&gt;getServer('SERVER_PORT');

        if (($scheme == self::SCHEME_HTTP && $port == 80) || ($scheme == self::SCHEME_HTTPS && $port == 443)) {
            return $name;
        } else {
            return $name . ':' . $port;
        }
    }
```