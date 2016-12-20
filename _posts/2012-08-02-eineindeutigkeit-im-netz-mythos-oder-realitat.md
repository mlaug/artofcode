---
id: 645
title: 'Eineindeutigkeit im Netz &#8211; Mythos oder Realität'
date: 2012-08-02T11:21:42+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=645
permalink: /2012/08/02/eineindeutigkeit-im-netz-mythos-oder-realitat/
thesis_keywords:
  - fingerprint, browser
categories:
  - Allgemein
  - IT und Wissenschaft
tags:
  - anonym
  - fingerprinting
  - frauddetection
---
Im Internet ist man anonym unterwegs! Totaler Quatsch mittlerweile. Wie in meinem <a href="http://www.artofcode.de/2012/05/27/make-the-internet-a-faster-place/" target="_blank">Artikel über Cookies</a> habe ich gezeigt, dass mittlerweile alle eCommerce Plattformen (und natürlich auch andere) stark auf das setzen von Cookies setzen um den Benutzer bei einem erneuten Besuchen der Seite wiederzuerkennen. Bei einer erfolgreichen Wiedererkennung versucht man dann im besten Fall die Seiten an die Bedürfnisse des Benutzers anzupassen und ihm Artikel anzubieten, die ihn Interessen könnten.

Hier sieht man das Beispiel bei Amazon, nachdem ich nach eine CI Modul gesucht habe.

[<img class="aligncenter  wp-image-646" title="Amazon Crosselling" src="http://www.artofcode.de/wp-content/uploads/2012/08/Bildschirmfoto-2012-08-02-um-11.50.51-1024x230.png" alt="" width="848" height="230" />](http://www.artofcode.de/wp-content/uploads/2012/08/Bildschirmfoto-2012-08-02-um-11.50.51.png)

Auf den ersten Blick ist das natürlich ein hilfreiches Feature, dass bei genauerer Betrachtung aber auch zu Missbrauch führen kann. Die Anonymität ist damit auf jeden Fall schon obsolet. Aber wo ist das Problem, dann lösche ich halt meine Cookies oder besuche Seiten im &#8222;privaten Modus&#8220;. Reicht das aus, um seine Anonymität wieder herzustellen? Mitnichten!

Findige Entwickler haben bereits neue Methoden gefunden und setzen hierbei auf Wahrscheinlichkeiten:

> There is a mathematical quantity which allows us to measure how close a fact comes to revealing somebody’s identity uniquely. That quantity is called entropy, and it’s often measured in bits. Intuitively you can think of entropy being generalization of the number of different possibilities there are for a random variable: if there are two possibilities, there is 1 bit of entropy; if there are four possibilities, there are 2 bits of entropy, etc. Adding one more bit of entropy doubles the number of possibilitie

So hat die [EFF](https://panopticlick.eff.org/) ein Script entwickelt, mit dessen Hilfe es möglich ist einen <a href="https://panopticlick.eff.org/index.php?action=log&js=yes" target="_blank" class="broken_link">Fingerabdruck von der Browserkonfiguration zu erstellen</a>. Welche Plugins sind aktiv und/oder installiert? Welche Bildschirmauflösung wird verwendet? Alle diese Informationen können zusammengefasst werden und stellen einen ziemlich einzigartigen Fingerabdruck her je nachdem wie viele Daten man sammelt. Basierend auf dieser Technologie hat man aktuell 2.5 Millionen Daten gesammelt, von denen sich mein eigener Abdruck als einzigartig herrausgestellt hat. Wollen wir nicht alle ein wenig einzigartig sein? Naja hier vielleicht mal nicht.

So ist es also möglich ohne die Verwendung von Cookies einen Benutzer im Netz zu verfolgen oder noch schlimmer trotz des Löschen von Cookies und anderen Daten. Solange keine Einstellungen geändert werden, ist der Rechner im Netz verfolgbar und das Domainübergreifend! Wenn man nun von dem üblichen Internetbenutzer ausgeht, der seinen Rechner nicht mehr ändert, nachdem er ihn bei MediaMarkt gekauft und installiert hat, besitzt man mit der Fingerprinting eine gute und sichere Methode Benutzer immer wieder zu erkennen.

Aber nehmen wir das ganze Script doch mal auseinander und prüfen, welche Eigenschaften einen Browser so einzigartig machen.

**Plugins:**

<a href="www.pinlady.net/PluginDetect" target="_blank" class="broken_link">PinLady</a> stellt für diesen Zweck ein Plugin in jQuery bereit mit dem Plugins nach der Version abgefragt werden können. Bei der Vielzahl von Plugins die es mittlerweile für Firefox, Safari und Co gibt, steht die Chance gut, dass fast jeder eine ziemlich einzigartige Kombination davon hat. Hierbei werden auch die Versionsnummern berücksichtigt, die aber gleichzeitig auch durch automatische Updates sich häufig ändern. Das gleich gilt da natürlich auch für den Firefox, der mittlerweile nicht nur Auto Update besitzt, sondern auch einen kurzen Release Zyklus.

**Fonts:**

Mit Hilfe von Flash oder Java wird Zugriff auf Systeminformationen genommen. Dabei geht es hauptsächlich um die Verfügbarkeit von Fonts. Meiner Meinung nach allerdings ein Schwaches Merkmal. Selten bis gar nicht greifen Leute auf die Fonts ihres Systems zurück und verwenden die vom System vorinstallierten Typen. Aber ups&#8230;

[<img class="aligncenter size-full wp-image-654" title="Auswertung Fonts" src="http://www.artofcode.de/wp-content/uploads/2012/08/Bildschirmfoto-2012-08-02-um-12.54.14.png" alt="" width="233" height="48" />](http://www.artofcode.de/wp-content/uploads/2012/08/Bildschirmfoto-2012-08-02-um-12.54.14.png)

Anscheinend besitzt nur 1 aus 2326966 Browsern diese Anzahl und Auswahl von Fonts. Diese Eigenschaft zeigt damit eine überraschend und erstaunlich hohe Entropie.

**Hardware und Einstellungen:**

Zu guter Letzt werden auch verwendete Auflösung und mögliche Auflösungen hinzugefügt. Somit quantifiziert die Hardware nochmal die möglichen Kombinationen

**Resumee:**

Was kann man von dieser Technik also erwarten? Gut ist die Unabhängigkeit von Cookies, die Dauer von 3 -4 Sekunden bis zur vollständigen Ermittlung ist wiederrum ein Manko. Für Frauddetection ist dies jedoch eine gute Technik um sich unabhängig von Cookies zu machen. Ein Fingerabdruck ist aber weiterhin keine eindeutige Zuordnung zu der Person hinter dem Bildschirm, sondern nur eine Identifikation des Endgeräts. Eine Technik hierzu fehlt noch vollständig.

Aber es ist doch überraschend wie Einzigartig ein Browser sein kann&#8230;