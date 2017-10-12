---
id: 460
title: FCGI und Duplicate Header
date: 2010-12-07T14:34:02+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=460
permalink: /2010/12/07/fcgi-und-duplicate-header/
categories:
  - Allgemein
---
Bei der Umstellung von FCGI in Zusammenhang mit Zend kann es vorkommen, dass man in seinem Apache folgenden Log eintrag finde

> FastCGI: comm with server &#8222;/home/ydadmin/htdocs/live/public_fcgi/index.php&#8220; aborted: error parsing headers: duplicate header &#8218;Content-Type&#8216;

Dies passiert leider, da FCGI sehr streng mit den HTTP Standards umgeht und dem Apache hier sofort einen 500er schickt. Die häufigste Fehlerquelle, die ich gefunden habe war hierbei der aufruf _$this->_forward_ der die HTTP Header korrumpiert. Nachdem ich diese aus meine Applikation rausgenommen habe, klappten die URL Aufrufe wieder einwandfrei.