---
id: 466
title: $.ajax und Cookieless PHP Session
date: 2010-12-23T15:48:02+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=466
permalink: /2010/12/23/ajax-und-cookieless-php-session/
categories:
  - Php
  - Webentwicklung
---
Der Internet Explorer ist bekannt dafür immer wieder Probleme mit Cookies zu haben und benötigt z.B. in einer IFrame Umgebung bestimmte Modifikationen. So muss man z.B. den Header entsprechend anpassen um Cross-Domain Cookies zu erlauben.

<pre>header('P3P:CP="IDC DSP COR ADM DEVi TAIi PSA PSD IVAi IVDi CONi HIS OUR IND CNT"');
</pre>

Andere Workarounds wäre das versenden von einem Post Request, bevor der IFrame selbst geladen wird

<pre>$('body').prepend('&lt;form id="sessionform" enctype="application/x-www-form-urlencoded" action="http://www.example.de/startsession" target="sessionframe" action="post"&gt;&lt;/form&gt;&lt;iframe id="sessionframe" name="sessionframe" src="http://www.example.de/blank" style="display:none;"&gt;&lt;/iframe&gt;');</pre>

Die beiden Seiten &#8222;startsession&#8220; und &#8222;blank&#8220; müssen dabei nur zur Verfügung gestellt werden und mindestens eine PHP Session initialisieren. Bei Post Request scheinen die meisten Browser (auch Safari) mehr Vertrauen zu haben und erlauben so den Cookie. Nach diesem Aufruf kann man den Iframe einbinden und der Cookie sollte bereits gesetzt sein.

Weiterhin hatte ich aber auch teilweise Probleme mit dem Verlust der PHP Session bei Ajax Requests. Das bekannte Problem von [&#8222;Bad Characters&#8220; im Hostnamen](http://www.php.net/manual/en/function.session-start.php#95372) schien hier nicht zu greifen. Sollte also der Cookie nicht funktionieren musste die SessionId noch auf andere Art und Weise übertragen werden. Dazu initialisierte ich mit dem Aufruf AjaxSetup die Ajax aufrufe durch Jquery und fügte jedem Request den Header hinzu

<pre>$(document).ready(function(){
    $.ajaxSetup({
        data : {
             session_id : '&lt;?php echo $session_id ?&gt;'
        }
    });
});</pre>

Dem HTML Template muss dazu per &#8222;session_id()&#8220; die entsprechende Session ID mitgegeben werden. Diese wird durch das AjaxSetup an jeden Request angehängt und kann so beim initialisieren der Session überprüft werden. (Hier in Zend)

<pre>if ( isset($_GET['session_id']) ){
    Zend_Session::setId($_GET['session_id']);
}</pre>

Und schon klappt die Session auch bei den Ajax Request stetig. Und wieder einmal wird deutlich wie einfach Javascript wäre, wenn es doch nur einen einzigen Browser geben würde!

In wie weit das hier vorgestellte alles sicher ist muss ich selbst noch evaluieren. Hier wäre jedem die <a href="http://phpsec.org/" target="_blank">Seite von PHPSec</a> zu empfehlen.