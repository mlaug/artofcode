---
id: 480
title: Unsichere PHP Projekte
date: 2011-01-09T14:31:37+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=480
permalink: /2011/01/09/unsichere-php-projekte/
categories:
  - Bugs
  - Netzgemeinde
  - Php
---
PHP Projekte gibt es viele da draußen, und viele davon sind weit entfernt davon sicher zu sein. Meine eigenen würde ich selbst auch nie als vollständig sicher betrachten, dass wäre auch zu schön und eine absolute Utopie. Ich glaube aber das viele Unsicherheiten sich vermeiden lassen, wenn man die globals von PHP nicht allzu leichtfertig einsetzt. Ein POST oder GET Parameter ungefiltert auszulesen sollte eigentlich nicht passieren und ist durch Frameworks u.ä. auch nicht mehr durch Faulheit zu erklären. Wie viele jedoch noch darauf aufbauen 🙂 Seht selbst: [auf Google Code](http://www.google.com/codesearch?as_q=lang:php+echo\s%2B\$_%28GET|POST|REQUEST|COOKIE%29&btnG){.broken_link} 🙂