---
id: 392
title: PHPUnit mit zeitabhängigen Tests
date: 2010-10-24T12:02:26+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=392
permalink: /2010/10/24/phpunit-mit-zeitabhangigen-tests/
categories:
  - Php
  - PHPUnit
  - Webentwicklung
tags:
  - Php
  - PHPUnit
  - Testcases
  - Time
---
Testcases sind schon was feines. Richtig angewendet kann man mit ihnen nicht nur die Stabilität der gesamten Software erhöhen sondern erhält sich damit meistens auch eien roten Pfaden durch die Software, da neue Funktionalitäten nur in Abhängigkeit der bereits vorhandenen Testcases erstellt werden dürfen. Häufig hatte ich jedoch ein Problem bei meinen Testcases, wenn es Zeitabhängigkeiten geht. Funktionen die je nach aktueller Uhrzeit ein anderes Verhalten zeigen stellen meist ein Problem dar.

Ich habe mich diesem Problem auf eine etwas unkonventionelle Weise angenommen. So nutze ich die verschiedenen Zeitzonen, die wir so auf der Welt haben um die gewünschte Uhrzeit zu erzeugen und die PHP Funktion time() so dazu zu zwingen die gewünschte Zeit auszugeben. Noch einfacher wäre es natürlich gewesen, wenn man in PHP auch Funktion überladen kann, aber das bleibt wohl vorerst ein Punkt auf der Wunschliste von PHP.

Hier aber der Code um die gewünschte Zeit **näherungsweise** zu erzeugen (leider können wir ja nur Stundenschritte machen). Die Funktion _setTimezoneByOffset_ habe ich dabei direkt von der <a href="http://php.net/manual/de/function.date-default-timezone-set.php" target="_blank">php.net Seite</a> und sie an meine Bedürfnisse leicht angepasst. Diese Methode kann dann in den entsprechenden Testcases in der setup Methode aufgerufen werden.

<pre name="code" class="php">/**
 *
 * @param string $desiredTime
 * @return boolean
 */
function setTimezoneByDesired($desiredTime) {

    //calculate offset (approximation)
    $current = time();
    $wanted = strtotime($desiredTime);
    $offset = (integer) round(($wanted - $current)/60/60);

    $testTimestamp = time();
    date_default_timezone_set('Europe/Berlin');
    $testLocaltime = localtime($testTimestamp, true);
    $testHour = $testLocaltime['tm_hour'];


    $abbrarray = timezone_abbreviations_list();
    foreach ($abbrarray as $abbr) {
        foreach ($abbr as $city) {
            date_default_timezone_set($city['timezone_id']);
            $testLocaltime = localtime($testTimestamp, true);
            $hour = $testLocaltime['tm_hour'];
            $testOffset = $hour - $testHour;
            if ($testOffset == $offset) {
                return true;
            }
        }
    }
    return false;
}
</pre>