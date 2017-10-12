---
id: 631
title: Make the internet a faster place
date: 2012-05-27T10:12:33+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=631
permalink: /2012/05/27/make-the-internet-a-faster-place/
categories:
  - Pagespeed
tags:
  - cookies
  - Google
  - pagespeed
---
[<img class="alignleft size-full wp-image-634" title="Webmastertools Pagespeed" src="http://www.artofcode.de/wp-content/uploads/2012/05/Bildschirmfoto-2012-05-27-um-12.11.12.png" alt="" width="766" height="161" srcset="http://www.artofcode.de/wp-content/uploads/2012/05/Bildschirmfoto-2012-05-27-um-12.11.12.png 766w, http://www.artofcode.de/wp-content/uploads/2012/05/Bildschirmfoto-2012-05-27-um-12.11.12-300x63.png 300w" sizes="(max-width: 766px) 100vw, 766px" />](http://www.artofcode.de/wp-content/uploads/2012/05/Bildschirmfoto-2012-05-27-um-12.11.12.png)Viele schreiben sich dieses Kredo auf die Fahnen und manche tun auch etwas dafür. Google z.B. baut permanent Tools, die das Web schneller machen sollen. Für den Endbenutzer steht der Chrome zur Verfügung, der in Sachen Javascript immer noch ein Schippe drauf legt. Für Sysadmins gibt es <a href="https://developers.google.com/speed/pagespeed/mod" target="_blank">mod_pagespeed </a>für den Apache oder <a href="http://www.chromium.org/spdy/spdy-whitepaper" target="_blank">SPDY</a>. Daten werden immer schneller ausgeliefert, CDN System verteilen die Last, Proxys wie <a href="https://www.varnish-cache.org/" target="_blank" class="broken_link">Varnish</a> beschleunigen eine Seiten noch einmal enorm, in dem Anfragen erst gar nicht zum Backend gehen, sondern die Antworten andere Benutzer aus dem Cache ausgeliefert werden.

Wie kann es aber sein, dass trotz dieser Anpassungen, kaum eine Seite über eine OnLoad unter 2 Sekunden kommt? Ein paar Stichproben gefälligst?

  * www.amazon.de: **3.041s**
  * www.zalando.de: **9.005s**
  * www.wikipedia.de: **0.49s**

uuuh? 0.49s bei Wikipedia. Damit ist schon einmal ein möglicher Grund aus dem Weg geräumt worden. Serverpower ist nicht alles, wenn es um Ladezeiten geht. Ich muss keine Statistik bemühen um sagen zu dürfen, dass Wikipedia mit Sicherheit weiterhin deutlich mehr Zugriffe hat als Zalando. Gucken wir uns doch mal eine weitere Metrik an. Wie viele Cookies werden den so beim Besuchen der jeweiligen Seite abgelegt?

  * www.amazon.com: **47**
  * www.zalando.de: **59**
  * www.wikipedia.de: **1**

Na was haben wir den hier? Wikipedia legt bei seinem Benutzer gerade mal einen einzigen Cookie ab. Das ist nicht nur vorbildlich, was Datenschutz angeht, sondern auch einfach zu erklären. Wikipedia hat kein Interesse an der Analyse von Kunden, da Wikipedia nichts verkaufen will und damit auch nicht das Kaufverhalten Analysieren muss. Amazon und Zalando setzen exzessiv auf Trackings von Criteo, Zanox und Co. Diese ganzen Cookiewurfmaschinen erhöhen die Onload der Seite enorm. Als Resultat sehen wir hier, dass Zalando mit 9 Sekunden drastisch langsamer ist als andere Seiten.

> While site speed is a new signal, it doesn&#8217;t carry as much weight as the relevance of a page. Currently, fewer than 1% of search queries are affected by the site speed signal in our implementation and the signal for site speed only applies for visitors searching in English on Google.com at this point. We launched this change a few weeks back after rigorous testing. If you haven&#8217;t seen much change to your site rankings, then this site speed change possibly did not impact your site. (http://googlewebmastercentral.blogspot.de/2010/04/using-site-speed-in-web-search-ranking.html)

Google hat bereits 2010 gesagt, dass Geschwindigkeit einen Einfluss auf Suchergebnisse hat, diese aber noch nicht dramatisch beeinflusst. Ich denke aber, dass wird sich in nächster Zeit weiter ändern. Die Faustregel von Marketing Fuzzies lautet bereits: &#8222;Über 2 Sekunden Ladezeit erhöht die Absprungrate von Kunden enorm&#8220;. Was muss getan werden? Abschaffung dieser Tools ist fast nicht möglich, da man auf einer Webseite nun mal keinen direkten Kundenkontakt hat und anhand dieser Metriken sich eine Art gruppiertes Kundenprofil aufbaut. Aber es muss andere technischen Lösungen geben oder andere Möglichkeiten an das Feedback vom Kunden zu kommen. Seine Wünsche auf andere Art und Weise ermitteln um im ein Hilfreiches und befriedigendes Kauferlebnis zu ermöglichen. Den diese ganzen Banner nerven mittlerweile enorm. Schön ist nur, dass die Anzahl von Bannern meistens antiproportional zur Qualität der Seite steht. Werde ich mit Bannern überschwemmt, verlasse ich die Seite meist direkt&#8230;