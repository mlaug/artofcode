---
id: 201
title: UTF-8 in der PHP Session
date: 2010-08-17T21:06:13+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=201
permalink: /2010/08/17/utf-8-in-der-php-session/
categories:
  - Allgemein
  - Php
tags:
  - base64
  - Php
  - serialisierung
  - session
  - utf8
---
Häufig habe ich es beobachtet, dass Umlaute und andere Sonderzeichen, die als String in einer Session gespeichert sind, korrumpiert werden. Da das teilweise äußerst nervig ist, habe ich in meine ___sleep_ und ___wakeup_ Methode mit _base64_ ausgestattet. Da base64 nur mit ASCII Zeichen auskommt, hat auch die Session damit keine Problem mehr.

In meinen Objekten speichere ich alle Datenbankrelevanten Daten in einem Array. Über dieses iteriere ich und dekodiere alle Strings mit base64. Dabei berücksichtige ich auch Abhängigkeiten von anderen Objekten. Anbei sehr ihr das fertige Beispiel:

<pre class="php">/**
     * @author mlaug
     * @return array
     */
    public function __sleep(){
        //store strings as base64 so that
        //we do not lose utf8 encoding
        foreach($this-&gt;_data as $key =&gt; $value){
            if ( is_string($value) ){
                $this-&gt;_data[$key] = base64_encode($value);
            }
        }
        //get class name and discover class properties
        return array_keys(get_class_vars(get_class($this)));
    }

    /**
     * recreate object after unserialization
     * @author mlaug
     */
    public function __wakeup(){
        //restore strings from base64 encoding
        foreach($this-&gt;_data as $key =&gt; $value){
            if ( is_string($value) ){
                //this is to restore utf8 encoding
                $this-&gt;_data[$key] = base64_decode($value);
            }
            //check for objects and wake it up 🙂
            if ( is_object($value) ){
                $value-&gt;__wakeup();
            }
        }

    }
</pre>