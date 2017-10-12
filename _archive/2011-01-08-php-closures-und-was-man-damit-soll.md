---
id: 472
title: 'PHP Closures und was man damit soll&#8230;'
date: 2011-01-08T17:00:05+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=472
permalink: /2011/01/08/php-closures-und-was-man-damit-soll/
categories:
  - Php
  - Webentwicklung
---
Mit der Version 5.3 von PHP wurde anonyme Funktionen eingeführt, so genannte Lambda Funktionen. Den Namen haben diese Funktionen (wie auch in anderen Programmsprachen) vom [Lambda Kalkül](http://de.wikipedia.org/wiki/Lambda-Kalk%C3%BCl), eine formale Sprache zur Definition von Funktionen ohne großen Wert auf den Namen der Funktion zu legen. &#8222;_Namen sind Schall und Rauch_&#8220; wie einst schon Faust zu Gretchen sagte.

Aber was genau bringen mit jetzt anonyme Funktionen? Schneller und speichereffizienter? Ich vergleiche mal diese Beiden Scripte miteinander

<pre class="brush:php">&lt;?php

$start = 0;
$end = 1000000;
$closure = function($x) use (&$start){
	return $start + $x;
};        

do{
	$start = $closure(1);
}while($start &lt; $end);

echo memory_get_usage()/1024;</pre>

<pre class="brush:php">&lt;?php

function adder($a,$b){
	return $a+$b;
}

$start = 0;
$end = 1000000;

do{
    $start = adder($start,1);
}while($start &lt; $end);

echo memory_get_usage()/1024;</pre>

Auf der Konsole ausgeführt ergibt sich folgendes Bild

[](http://www.artofcode.de/wp-content/uploads/2011/01/Bildschirmfoto-2011-01-08-um-17.50.10.png)[<img class="aligncenter size-full wp-image-474" title="Bildschirmfoto 2011-01-08 um 17.50.10" src="http://www.artofcode.de/wp-content/uploads/2011/01/Bildschirmfoto-2011-01-08-um-17.50.101.png" alt="" width="453" height="144" srcset="http://www.artofcode.de/wp-content/uploads/2011/01/Bildschirmfoto-2011-01-08-um-17.50.101.png 453w, http://www.artofcode.de/wp-content/uploads/2011/01/Bildschirmfoto-2011-01-08-um-17.50.101-300x95.png 300w" sizes="(max-width: 453px) 100vw, 453px" />](http://www.artofcode.de/wp-content/uploads/2011/01/Bildschirmfoto-2011-01-08-um-17.50.101.png)

Kaum ein Unterschied also, wobei die &#8222;alte&#8220; Methode, wo man die Funktion zuvor deklariert anscheinend leicht bessere Laufzeiteigenschaften besitzt. Wenn man jetzt noch berücksichtigt, dass zur Laufzeit generierte Methoden nicht in den OpCode Cache landen ist zugunsten von Performance und Speicher die &#8222;alte&#8220; Methode zu bevorzugen.

Ob Closures jetzt eleganter sind, kann ich selbst zur Zeit gar nicht beurteilen, fallen mir doch gerade keine Einsatzmöglichkeiten ein, wo Closures wirklich effektiveren und auch besser lesbaren Code produzieren. Fällt mir doch ein UseCase ein, werde ich mal darüber schreiben 🙂