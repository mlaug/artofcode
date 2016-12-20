---
id: 23
title: Honeypot mit PhpMyAdmin
date: 2010-07-02T10:45:54+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=23
permalink: /2010/07/02/honeypot-mit-phpmyadmin/
categories:
  - Php
  - Webentwicklung
tags:
  - honeypot
  - mysql
  - Php
  - phpmyadmin
  - sicherheit
---
Ich verwalte eine Menge von Server für Dritte. Immer wenn ich einen neuen Server installiere dauert es nicht lange bis Bots versuchen hier nach Schwachstellen zu suchen. Dabei ist die beliebteste Anlaufstelle das PHP Administration Tool für MySql Datenbank <a title="PhpMyAdmin" href="http://www.phpmyadmin.net/home_page/index.php" target="_blank">PhpMyAdmin</a>. So sieht man in seinen Logfiles häufig folgende Einträge

<!--more-->

```bash
173.51.209.6 - - [21/Feb/2010:19:34:49 +0100] "GET //phpmyadmin/main.php HTTP/1.1" 302 475 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows 98)"
173.51.209.6 - - [21/Feb/2010:19:34:49 +0100] "GET //pma/main.php HTTP/1.1" 302 475 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows 98)"
173.51.209.6 - - [21/Feb/2010:19:34:50 +0100] "GET //admin/main.php HTTP/1.1" 302 472 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows 98)"
173.51.209.6 - - [21/Feb/2010:19:34:59 +0100] "GET //myadmin/main.php HTTP/1.1" 302 475 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows 98)"
173.51.209.6 - - [21/Feb/2010:19:35:00 +0100] "GET //PHPMYADMIN/main.php HTTP/1.1" 302 475 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows 98)"
```

Natürlich ist diese Software nicht installiert, vor allem nicht auf einem von Außen erreichbaren Server. Es gibt mir jedoch einen Hinweis darauf, das diese IP in den nächsten Stunden nichts Gutes vor hat. Also sollte sie es auch nicht stören, wenn sie gesperrt wird. Dafür habe ich ich in meinem htdocs Ordner folgendes script unter dem Unterordner &#8218;phpmyadmin&#8216; abgelegt

```php
define("N",chr(13).chr(10));
$fp = fopen('../.htaccess','a+');
fputs($fp, N . "deny from " . $_SERVER['REMOTE_ADDR']);
fclose($fp);
```

Versucht nun jemand auf den PhPMyAdmin zuzugreifen wird seine IP über die htaccess Datei einfach gesperrt. Ist kein großer Schritt in Richtung Sicherheit, gibt einem jedoch eine gewissen Genugtuung, wenn jeden Tag so an die 10 neuen Einträge in der htaccess kommen. Da jedoch nur die dümmsten aller Cracker eine feste IP verwenden, sollte man alle 24h die Einträge wieder aus der htaccess nehmen.

Zum Schluss aber noch ein kleiner Hinweis. Erzählt euren Praktis von diesem &#8222;Feature&#8220;, ansonsten sperren diese mal ganz schnell die ganze Firma aus, wenn sie wie aus der Schule gewohnt überall mit PhpMyAdmin arbeiten (ich erinnere mich da nur zu Gut an meine Zeiten in der Berufsschule :D)