---
id: 241
title: PHP Html Cache
date: 2010-08-28T20:25:17+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=241
permalink: /2010/08/28/php-html-cache/
categories:
  - Php
  - Webentwicklung
tags:
  - cache
  - perfomanz
  - Php
---
Um die Last auf der Startseite zu verringern, ist es ratsam diese zu Cachen. Wäre das dann schon mal klar gestellt. Die Frage, die ich mir jedoch häufig stelle, ist: &#8222;Wie geht es am effektivsten?&#8220;. Maximal effektiv wäre es eigentlich, wenn nur noch das gecachte Objekt geladen wird. Sonst nichts!

<!--more-->

Den größten Gewinn an Leistung bekommt man also meist nur, wenn man das komplette Framework umgeht. Viele der dort initialisierten Klassen (bei Zend z.B. die Routen und eine Datenbankverbindung), sind hier noch nicht nötig. Erst nach der ersten Eingabe des Benutzers, kommt das Framework zum Zuge. Um diesen, nennen wir ihn mal Wasserkopf, zu umgehen verwende ich zwei Mechanismen.

### Ausgabepuffer

Egal wie man seine Seite generiert, alles muss irgendwann ausgegeben werden. Und um diesen Inhalt gesammelt abzuholen, verwendet man den folgenden Code. (In diesem Beispiel wird eine Instanz von Zend erstellt)

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="php" style="font-family:monospace;"><span style="color: #990000;">ob_start</span><span style="color: #009900;">&#40;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
<span style="color: #000088;">$application</span><span style="color: #339933;">-&gt;</span><span style="color: #004000;">bootstrap</span><span style="color: #009900;">&#40;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">-&gt;</span><span style="color: #004000;">run</span><span style="color: #009900;">&#40;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
<span style="color: #000088;">$content</span> <span style="color: #339933;">=</span> <span style="color: #990000;">ob_get_contents</span><span style="color: #009900;">&#40;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
<span style="color: #990000;">ob_end_flush</span><span style="color: #009900;">&#40;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span></pre>
      </td>
    </tr>
  </table>
</div>

In der Variable _$content_ befindet sich nun die gesamte Seite, die soeben generiert wurde. Jetzt müssen wir nur noch überprüfen, um welche Seite es sich handelt und diese dann speichern

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="php" style="font-family:monospace;"><span style="color: #b1b100;">if</span> <span style="color: #009900;">&#40;</span> <span style="color: #000088;">$_SERVER</span><span style="color: #009900;">&#91;</span><span style="color: #0000ff;">'REQUEST_URI'</span><span style="color: #009900;">&#93;</span> <span style="color: #339933;">==</span> <span style="color: #0000ff;">'/'</span> <span style="color: #009900;">&#41;</span><span style="color: #009900;">&#123;</span>
    <span style="color: #000088;">$cache</span> <span style="color: #339933;">=</span> APPLICATION_PATH <span style="color: #339933;">.</span> <span style="color: #0000ff;">'/../public/cache/index.html'</span><span style="color: #339933;">;</span>
    <span style="color: #b1b100;">if</span> <span style="color: #009900;">&#40;</span> <span style="color: #339933;">!</span><span style="color: #990000;">file_exists</span><span style="color: #009900;">&#40;</span><span style="color: #000088;">$cache</span><span style="color: #009900;">&#41;</span> <span style="color: #009900;">&#41;</span><span style="color: #009900;">&#123;</span>
        <span style="color: #000088;">$fp</span> <span style="color: #339933;">=</span> <span style="color: #990000;">fopen</span><span style="color: #009900;">&#40;</span><span style="color: #000088;">$cache</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">'a+'</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
        <span style="color: #990000;">fwrite</span><span style="color: #009900;">&#40;</span><span style="color: #000088;">$fp</span><span style="color: #339933;">,</span><span style="color: #000088;">$content</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
        <span style="color: #990000;">fclose</span><span style="color: #009900;">&#40;</span><span style="color: #000088;">$fp</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
    <span style="color: #009900;">&#125;</span>
<span style="color: #009900;">&#125;</span></pre>
      </td>
    </tr>
  </table>
</div>

Der Pfad, in dem die HTML Datei gespeichert wird, muss sich unter dem &#8218;Document Root&#8216; befinden. Diese Voraussetzung wird für die folgenden htaccess Regeln benötigt

<pre>RewriteCond %{REQUEST_URI} ^/$
RewriteCond %{REQUEST_METHOD} !POST
RewriteCond %{DOCUMENT_ROOT}/cache/index.html -f
RewriteRule ^(.*) "/cache/index.html" [L]
</pre>

Diese überprüfen, ob es sich um eine GET Anfrage handelt und ob eine Datei im Cache Ordner vorhanden ist. Wenn dem so ist, wird diese ausgegeben und so der gesamte Wasserkopf der PHP Applikation umgangen. Dies nimmt für die Landingpage eurer Seite eine Menge Last vom Server und die Seite antwortet schneller.