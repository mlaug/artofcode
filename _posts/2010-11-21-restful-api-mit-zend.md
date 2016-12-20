---
id: 445
title: RESTful API mit Zend und der PUT Request
date: 2010-11-21T13:42:54+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=445
permalink: /2010/11/21/restful-api-mit-zend/
categories:
  - API
  - RESTful
  - Webentwicklung
---
Seit einiger Zeit und auch gerade nach dem Besuch des [PHP World Kongresses in München](http://www.artofcode.de/2010/11/14/php-world-kongress-in-munchen-tag-1/) beschäftige ich mich intensiver mit dem Thema REST. Ich habe schon häufiger eine API damit gebaut jedoch nie alle Möglichkeiten ausgenutzt und vor allem NIE alle Standards eingehalten. Und ich liebe Standards. Die Möglichkeit an einer Stelle etwas zu ändern was positiv durch das gesamte kaskadiert, weil alles standardisiert ist: Das ist schon fein!

Die größte Stärke von REST liegt darin, das es auf dem HTTP Standard aufbaut. Ein Anwender, der die API in sein System implementieren will, weiß somit a priori wie die CRUD Operationen aussehen werden.

**INDEX:** Eine Auflistung alles Elemente oder ein Startpunkt mit den entsprechenden Links (und den dazugehörigen Relations) wird zur Verfügung gestellt

**GET:** Mit der Übergabe einer ID wird ein Element gesucht und dessen Informationen ausgegeben

**POST:** Ein neues Element wird erzeugt

**PUT:** Ein bereits Element wird aktualisiert

**DELETE:** Ein Element wird gelöscht

So einfach ist es manchmal. Lediglich die Parameter, die man z.B. bei einer GET Operation übergeben kann um entsprechende Suchen durchzuführen ist nicht direkt erkenntlich. Im Zusammespiel mit einer sauberen Baumstruktur der URLs kann hier eine wirklich schöne API entstehen die einfach und effektiv ist. Das diesem Konzept nicht alle treu sind zeigt vor allem die API von Twitter. Hier wird nicht nur die POST Methode mehrfach überlagert (PUT wird nicht verwendet) sondern auch die Baumstruktur ist teilweise nicht verständlich. Vorallem weil sich die Operationen, die ausgeführt werden sollen (z.b. create) nicht explizit über die HTTP Aktionen implementiert wurden sondern durch Key Wörter innerhalb der Baumstruktur.

Ein Beispiel wäre das löschen eines Elementes, was durch die Übermittlung des Wortes &#8222;destroy&#8220; innerhalb der Baumstruktur getriggert wird.

<pre>/direct_messages/destroy/456
</pre>

Sehr unschön so was 🙂 aber Twitter hält diese Schwäche auch nicht geheim, so lesen wir <a rel="nofollow" href="http://dev.twitter.com/pages/every_developer" target="_blank" class="broken_link">hier</a>:

> Methods to retrieve data from the Twitter API require a `GET` request. Methods that submit, change, or destroy data require a `POST`. A `DELETE` request is also accepted for methods that destroy data. API Methods that require a particular HTTP method will return an error if you do not make your request with the correct one. <a rel="nofollow" href="http://dev.twitter.com/pages/responses_errors" class="broken_link">HTTP Response Codes</a> are meaningful

Der letzte Satz ist dabei der Hammer. Folgt man dem Link, fehlen in der Auflistung die wirklich &#8222;meaningfulen&#8220; Stati völlig. Wie z.B. 201 CREATED

Meckern kann jedoch jeder, daher habe ich mich mal selbst an einer korrekten Implementierung versucht. Ich wollte mir mal die REST Controller von Zend angucken und mich mal selber mit den PUT und DELETE Methoden beschäftigen, die bei Twitter gar nicht oder nur als alternative zum Einsatz kommen.

Dabei ist mir aufgefallen, dass die DELETE Methode sich wunderbar implementieren lässt. Die PUT Methode jedoch ein einziger Grauss ist. Diese Methode wurde wohl erst mit RESTful APIs wieder entdeckt und ist überall sehr Stiefmütterlich implementiert worden. Wenn man genau nachschaut ist der PUT Request sogar wieder aus dem HTML5 Standard rausgeflogen. Anders kann ich es mir auch nicht erklären, dass man die Parameter, die übergeben werden, über die STDIN von PHP abholen muss.

<pre class="code">public function putAction() {
    $put = array();
    parse_str(file_get_contents('php://input'), $put);
}
</pre>

Das ich den STDIN von PHP irgendwann mal wirklich abseits der Konsole gebrauchen kann wäre mir nie in den Sinn gekommen 🙂