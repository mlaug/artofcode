---
id: 567
title: 'The Line must be drawn here &#8211; Code Design braucht Diktatoren'
date: 2011-12-20T21:44:34+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=567
permalink: /2011/12/20/the-line-must-be-drawn-here-code-design-braucht-diktatoren/
image: /wp-content/uploads/2011/10/Bildschirmfoto-2011-10-16-um-22.14.13.png
categories:
  - Webentwicklung
tags:
  - code design
---
Code Design ist ein schwieriges Unterfangen. Entscheidungen müssen früh gefällt werden um dem Entwickler Team einen Standard an die Hand zu geben, der es ermöglicht eine Software zu generieren, die auch nach langer Entwicklungszeit wartbar und testbar bleibt. Soll die Software monolithisch aufgebaut werden oder soll man die Komponenten doch lieber dezentral über eine Pipeline ihre Jobs abarbeiten lassen. Es gibt viele Möglichkeiten und grundsätzlich kann man sich sicher sein, dass man zu irgendeinem Zeitpunkt merkt, dass man Fehler gemacht hat und umdenken muss.

**Was aber klappt nie? Demokratie&#8230;**

Hinter einem guten und flexiblen Softwaredesign steht ein Diktator. Jemand der Ahnung vom Werk hat und beim Design ganz klar ansagt, wie die Software zu gestalten ist. Open Source ist eine super Sache und ich plädiere dafür, so viel wie möglich der Community zur Verfügung zu stellen. Ich würde aber niemals behaupten, dass man in der Open Source Gemeinde eine Software findet, die ein sauberes und elegantes Design besitzt. Beispiel? WordPress! Eine Blogsoftware, die so weit verbreitet ist und auf die ich hier selber setze. Aber möchte ich ein Plugin für WordPress schreiben? Um Himmels Willen, Nein! Das Konzept von Hooks und Plugins ist so verwoben, dass man teilweise einen Knoten im Kopf bekommt, wenn man mal einen Blick unter die Haube wagt.

Also, wenn ihr die Verantwortung für ein Softwareprojekt übernehmt und eine klare Vorstellung von der Software habt, dann lasst euch nicht rein reden. Lasst euch vorher beraten, holt euch Ideen von anderen, aber während der Entwicklung gebt klare Richtlinien und lasst niemanden ausbrechen. Denkt daran, Entwickler sind Schweine, die machen das Code stinkt! Euer Software Design ist dabei wie ein großer Besen, der von selber sauber macht&#8230;

**Zieht die Linie früh und lasst sie niemanden überschreiten!