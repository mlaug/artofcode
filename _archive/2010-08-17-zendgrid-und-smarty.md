---
id: 199
title: ZendGrid und Smarty
date: 2010-08-17T18:27:44+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=199
permalink: /2010/08/17/zendgrid-und-smarty/
categories:
  - Php
  - Smarty
  - Webentwicklung
  - Zend
tags:
  - grid
  - Php
  - Smarty
  - Zend
---
Auch wenn zur Zeit Diskussionen aufkommen, <a href="http://www.virtualchaos.co.uk/blog/2008/09/14/to-smarty-or-not-to-smarty/" target="_self">ob Smarty bzw. Template Engines in PHP überhaupt noch benötigt werden</a>, so setze ich es zur Zeit immer noch in einer Menge von Projekten ein. Da das Zend Framework nativ nicht mit einer Unterstützung von Smarty daher kommt, habe ich mit dem Zend_View Interface meine eigene Klasse als Wrapper für Smarty erstellt. Dabei habe ich mich zu großen Teilen an die Anmerkungen und Kommentare der <a href="http://devzone.zend.com/article/120" target="_blank" rel="nofollow" class="broken_link">DevZone</a> von Zend gehalten.

Um nun auch das <a href="http://zfdatagrid.com/" target="_self" rel="nofollow">ZendGrid</a> zu verwenden, musste ich ein paar Anpassungen vornehmen, um eine Instanz des Grids zu erstellen.

<pre class="php">$grid = Bvb_Grid_Data::factory('Bvb_Grid_Deploy_Table', new Zend_Config(array()), $id);
$grid-&gt;setView(new Zend_View());
</pre>

Auf diese Weise war es zumindest schon einmal möglich einen Grid anzuzeigen. Wenn jedoch jetzt auch noch die Filterfunktionen funktionieren sollen, muss eine Methode in der Klasse _Bvb\_Grid\_Deploy_Table_ angepasst werden.

<pre class="php">public function getHeaderScript(){
    return $this-&gt;_printScript();
}
protected function _printScript (){
    [...]
    $this-&gt;getView()-&gt;headScript()-&gt;appendScript($script);
    return $script;
}
</pre>

Auf diese Weise kann der generierte Javascript Code über die _getHeaderScript_ Methode abgeholt werden und so in Smarty als Variable direkt in den Header geparsed werden. Das wurde zuvor von der _appendScript Methode_ geleistet, die in der Smarty Klasse jedoch nicht implementiert wurde.