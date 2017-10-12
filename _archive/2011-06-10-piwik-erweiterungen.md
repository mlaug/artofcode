---
id: 507
title: Piwik Erweiterungen
date: 2011-06-10T19:55:20+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=507
permalink: /2011/06/10/piwik-erweiterungen/
categories:
  - Php
  - Webentwicklung
---
Wer es noch nicht kennt sollte es umbedingt einmal ausprobieren. <a href="http://piwik.org/" target="_blank">Piwik ist eine Open Source Alternative zu Google Analytics</a>, ist mittlerweile in der Version 1.4 erschienen und in PHP geschrieben. Dabei liegen die Vorteile in der offenen Datenbankstruktur und der Möglichkeit Plugins selber zu schreiben. Auch die Integration in das eigene System gestaltet sich durch die Widgets sehr einfach. Mächtig wird das ganze jedoch erst duch seine <a href="http://piwik.org/docs/analytics-api/segmentation/" target="_blank">API zur Segmentierung</a>. Um die Integration in das eigene System zu erleichtern, habe ich die PHP API Klasse erweitert und auch um die Möglichkeit erweitert mit Zielmetriken zu arbeiten. Da diese eigentlich immer erst in Piwik erstellt werden müssen habe ich einen etwas anderen Weg genommen. Wenn man ein Ziel tracken möchte und dieses existiert noch nicht wird die Klasse diese erstellen und die nötigen Assoziationen in einer lokalen Tabelle speichern.

<pre class="brush:sql">create table `piwikGoals` (
    `id` INT NOT NULL auto_increment primary key,
    `goalId` INT NOT NULL,
    `goalName` VARCHAR(255) NOT NULL UNIQUE
) ENGINE=InnoDB CHARACTER SET utf8 COLLATE utf8_general_ci</pre>

Anschließend habe ich die vorhanden [Piwik Tracker Klasse](http://demo.piwik.org/index.php?module=SitesManager&action=downloadPiwikTracker&idSite={$IDSITE}&piwikUrl=http://www.example.org/piwik/) erweitert. Das ganze basiert auf Zend &#8230;

<pre class="brush:php">&lt;?php

/**
 * Description of Tracker
 *
 * @author matthiaslaug
 */
class My_Piwik_Tracker extends Piwik_Tracker{

    /**
     *
     * @param string $name
     * @param integer $revenue
     */
    static function trackGoal($name, $revenue = 0){
        //add to your config
        //piwik.id = YOUR_PIWIK_ID
        //piwik.url = URL_TO_YOUR_PIWIK_INSTALLATION
        //piwik.token = YOUR_PIWIK_AUTH_TOKEN
        $config = Zend_Registry::get('configuration');
        try {
            $t = new Yourdelivery_Model_Piwik_Tracker($config-&gt;piwik-&gt;id, $config-&gt;piwik-&gt;url);
            $result = $t-&gt;doTrackGoalAndCreate($name, $revenue);
        } catch (Exception $e) {
        }
    }

    /**
     * track a goal but create relation if not existent
     * @author mlaug
     * @since 08.06.2011
     * @param string $name
     * @param integer $revenue
     * @return boolean
     */
    public function doTrackGoalAndCreate($name, $revenue = 0) {
        $table = new DbTable_Piwik_Goals();
        $row = $table-&gt;fetchRow('goalName="' . $name . '"');
        if ( !$row ){
            $goalId = $this-&gt;createGoal($name);
            if ( $goalId &lt;= 0 ){
                return false;
            }
            $table-&gt;createRow(array(
                'goalId' =&gt; $goalId,
                'goalName' =&gt; $name
            ))-&gt;save();
        }
        else{
            $goalId = (integer) $row-&gt;goalId;
            if ( $goalId &lt;= 0 ){
                return false;
            }
        }
        $this-&gt;doTrackGoal($goalId, $revenue);
        return $goalId;
    }

    /**
     * create a goal
     * @author mlaug
     * @since 08.06.2011
     * @param string $name
     * @param string $pattern
     * @param string $matchAttr
     * @param string $patternType
     * @param boolean $caseSensitive
     * @param integer $revenue
     * @param boolean $allowMultipleConversionsPerVisit
     * @return integer
     */
    public function createGoal($name, $pattern = '/', $matchAttr = 'manually', $patternType = 'contains', $caseSensitive = false, $revenue = 0, $allowMultipleConversionsPerVisit = false){
        $config = Zend_Registry::get('configuration');
        $url  = self::$URL . '?idSite=' . $this-&gt;idSite;
        $url .= '&module=API';
        $url .= '&method=Goals.addGoal';
        $url .= '&matchAttribute=' . $matchAttr;
        $url .= '&patternType=' . $patternType;
        $url .= '&pattern=' . $pattern;
        $url .= '&name=' . $name;
        $url .= '&token_auth=' . $config-&gt;piwik-&gt;auth;
        $ret = $this-&gt;sendRequest($url);
        try{
            $xml = new DOMDocument();
            $xml-&gt;loadXML($ret);
            if ( $xml-&gt;getElementsByTagName('error')-&gt;length &gt; 0 ){
                return 0;
            }
            $goalId = (integer) $xml-&gt;getElementsByTagName('result')-&gt;item(0)-&gt;nodeValue;
            return $goalId;
        }
        catch ( Exception $e ){
            return 0;
        }
    }

    /**
     * @author mlaug
     * @since 08.06.2011
     * @param integer $idGoal
     * @return boolean
     */
    public function deleteGoal($idGoal){
        $config = Zend_Registry::get('configuration');
        $url  = self::$URL . '?idSite=' . $this-&gt;idSite;
        $url .= '&module=API';
        $url .= '&method=Goals.deleteGoal';
        $url .= '&idGoal=' . $idGoal;
        $url .= '&token_auth=' . $config-&gt;piwik-&gt;token;
        $ret = $this-&gt;sendRequest($url);
        try{
            $xml = new DOMDocument();
            $xml-&gt;loadXML($ret);
            if ( $xml-&gt;getElementsByTagName('error')-&gt;length &gt; 0 ){
                return false;
            }
            if ( $xml-&gt;getElementsByTagName('success')-&gt;length &gt; 0 ){
                $table = new Yourdelivery_Model_DbTable_Piwik_Goals();
                $table-&gt;delete('goalId='.$idGoal);
                return true;
            }
            return false;
        }
        catch ( Exception $e ){
            return false;
        }
    }

}

?&gt;</pre>

Und noch die DbTable Klasse

<pre class="brush:php">/**
 * Description of Goals
 *
 * @author matthiaslaug
 */
class DbTable_Piwik_Goals extends Zend_Db_Table {

    /**
     * name of the table
     * @param string
     */
    protected $_name = 'piwikGoals';
    //put your code here
}</pre>

Ich hoffe das nützt jemandem 🙂