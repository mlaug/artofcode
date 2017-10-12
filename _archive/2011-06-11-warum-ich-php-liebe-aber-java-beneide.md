---
id: 516
title: 'Warum ich PHP liebe &#8211; aber Java beneide'
date: 2011-06-11T21:43:52+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=516
permalink: /2011/06/11/warum-ich-php-liebe-aber-java-beneide/
thesis_post_image:
  - http://www.artofcode.de/wp-content/uploads/2011/06/6a0120a85dcdae970b01538e3ee72e970b-800wi.jpg
image: /wp-content/uploads/2011/06/6a0120a85dcdae970b01538e3ee72e970b-800wi.jpg
categories:
  - Allgemein
---
<p style="text-align: left;">
  Wenn man eine Webapplikation bauen möchte, ist, wenn man den Statistken glaubt, PHP die erste Wahl. PHP ist einfach und sein Stack in Apache macht auch den ersten Rollout sehr einfach. Mit PHP kann man komplizierte Sachverhalte sehr einfach ausdrücken und die Fülle an Beispielen und Erweiterungen im Netz sind einfach atemberaubend. Große Webprojekte habe ich bisher fast immer in PHP durchgeführt. Man kann sehr schnell einen Prototypen bauen und mit dem Kunden ins Feedback gehen. Die einfache Einbindung in HTML ist zwar Fluch und Segen zugleich, unterstützt einen jedoch sehr beim Erstellen von kleinen Beispielen. PHP ist mittlerweile auf einem Stand, wo man es wesentlich ernster nehmen muss. Warum man das bisher immer noch nicht macht?
</p>

Naja vielleicht aus den folgenden Gründen, die mich immer wieder zur Räson bringen und auch mal Python oder Java verwenden lassen. Man kann es vielleicht mit den Worten von Barney Stinsen und der &#8222;Hot and Crazy&#8220; Scale beschreiben. PHP war erst ganz weit unten im Bereich _crazy_, dann kam die Version 5.x raus und PHP wurde einiges _hotter_. Dann wurden Namespaces eingeführt und ich musste die böse Erfahrung machen, dass PHP bei seinen Funktion nicht _case-sensitive_ ist => _crazy_. Dann kam Bergmann mit seinen PHPUnit Tests => _hot_ 🙂

**1. PHP verzeiht sehr viel**

Ja, PHP ist sehr gut zu uns. Der Intepreter baut die Variable noch unseren Vorgaben von String zu Integer zu Float zu Array zu Objekt. Wir sind hier  kaum an Konventionen gebunden. Kleine Scripte können dadurch sehr schnell Form annehmen, sind in der Regel jedoch anschließend unwartbar. Sobald man also größere Projekte mit mehr als 2 Entwicklern starten möchte, wäre eine Art optionale Typensicherheit in PHP sehr hilfreich. Nicht nur bei den Parameterlisten (das ist ja mittlerweile teilweise möglich), sondern gerade bei den Rückgabewerten einer Funktion.

<pre class="brush:php">public function foo(array $a, My_Class $b) //das geht schon

public function foo(integer $a, string $b) //das schon wieder nicht

public array function foo() //und das schon gar nicht 🙁</pre>

Ich kriege häufig die Krise, wenn ich eine Methode bekomme, die in einem Fall einen String zurück liefert und in einerm anderen Fall ein Array. Das macht einfach keinen Sinn und den Code, den man durch so eine Funktion _vielleicht_ gespart hat, benöigt man wieder für die Fallunterscheidung beim Aufruf der Funktion. Bringt also nichts&#8230;

**2. Exceptions innerhalb von Methoden**

Wirft man innerhalb von PHP eine Exception (wenn überhaupt, viele bevorzugen ja doch eher die &#8222;return false&#8220; Methodologie) kann man diese Tatsache durch die Spezifikation der Methode nicht nach außen deutlich machen.

<pre class="brush:php">public function blub() throws Exception //leider auch nicht</pre>

Wer die Methode also nicht kennt und vorher nicht untersucht, weiß bis zu ihrem Auftreten erstmal nichts von der Exception. Und wer guckt sich schon fremde Methoden vor ihrer Verwendung an 🙂

**3. Threads**

Meiner Meinung nach der zentrale Punkt, warum PHP immer noch nicht vollständig ernst genommen wird. Korrekter Einsatz und Synchronisation sind mit eine Disziplin in der IT, die nicht jeder versteht. Wenn die Möglichkeit nun in einer Sprache vollständig fehlt, könnte man auch meinen, dass man die Entwickler hier auch vor sich selbst schützen möchte. Nach dem Motto &#8222;Messer, Feuer, Threads und Licht, sind für PHPler nichts&#8220; schützt man hier den Entwickler vor sich selbst!? Und kommt mir jetzt nicht mit pcntl_fork, solange mir hier niemand auch eine vernünftige Art der Kommunikation zwischen Parent und Child Prozess zeigen kann&#8230;

**4. Performance**

Ich denke hier muss ich nicht groß darauf rumreiten. PHP wird immer schneller, aber an die Performance einer Sprache wie C++ oder Java wird es so schnell (wenn überhaupt) nicht rankommen.

**5. IDE**

Nach meinem Geschmack habe ich bisher noch keine IDE gefunden, die eine saubere Integration von PHP (zusammen mit PHPUnit und dem ganzen restlichen Bergmann Clan) zur Verfügung stellt. Man ist entweder auf kostenpflichte Tools wie die PHP IDE von Zend (die allerdings auch auf Eclipse basiert) gebunden oder erweitert eine IDE wie NetBeans um die nötigen Addons. Aus der Box kommt hier aber wenig. ****

**6. Copy&Paste, keine Innovation
  
** 

All diese hübschen Erfindungen zur Qualitätssicherung wie Jenkins oder Unittests kommen alle aus der Java Welt. Warum kommt keine Innovation in diese Richtung aus der PHP Welt? Warum muss erst nachgeahmt werden? PHP scheint sich aus allen anderen Sprachen die guten Sachen rauszupicken. Das ist gut, aber langweilig. Ich will auch mal was mit PHP machen, was andere nicht machen können.

Das sind all die Gründe, warum ich PHP immer weider untreu werde und manche Projekte in Java (oder auch gern mal ein bisschen Python) durchführe. Aber wer mag schon mit einer Sprache die ganze Zeit vorlieb nehmen. Monogamie ist schon genug Anstrengung in den Real Life Dingen.