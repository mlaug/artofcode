---
id: 992
title: What could possibly go wrong
date: 2014-12-10T15:56:31+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=992
permalink: /2014/12/10/what-could-possibly-go-wrong/
categories:
  - Allgemein
  - Server
  - Teamarbeit
---
[<img class="alignright wp-image-1000 size-full" src="http://www.artofcode.de/wp-content/uploads/2014/12/Download.gif" alt="so close, yet so far :(" width="500" height="375" />](http://www.artofcode.de/wp-content/uploads/2014/12/Download.gif)

**Disclaimer**: _I am the IT Lead of Takeaway responsible for lieferando, but those thoughts and fails are my own!_

To sum it up &#8211; we screwed up and we were so confident to succeed.

But let&#8217;s start from the beginning. We planed a big marketing campaign at [lieferando](http://www.lieferando.de/gratis-schlemmer-tag#!){.broken_link} that created a big buzz in all the social media platforms and we were aware of a big impact on our servers. Since our system is designed this way to [handle quite a big load](http://www.artofcode.de/2013/05/16/software-in-schichten-die-neue-seite-von-lieferando/ "Software in Schichten – die neue Seite von Lieferando") with far less servers than our competitors we were confident we will handle the load and looked forward to prove everybody wrong about their premature statements about us getting run over.

But we weren&#8217;t ignorant, we planned load testing, prepared backup plans, tested fallbacks and simulated server outages. We thought we were ready, but apparently we were not. So what did go wrong?

A tale of  naive assumptions and not so loosely dependencies&#8230;

<!--more-->

### **Naive load testing**

Our load testing was carried out by JMeter and hit our API responsible for handling incoming orders. We achieved 20 orders per second on a single node, where we had 4 of.

> <p class="p1">
>   <span class="s1">summary +    210 in    11s =   19.4/s Avg:   306 Min:   228 Max:   461 Err:     0 (0.00%) Active: 9 Started: 12 Finished: 3</span>
> </p>
> 
> <p class="p1">
>   <span class="s1">summary +    390 in    17s =   23.1/s Avg:   302 Min:   217 Max:   466 Err:     0 (0.00%) Active: 0 Started: 20 Finished: 20</span>
> </p>
> 
> <p class="p1">
>   <span class="s1">summary =    600 in  27.5s =   21.9/s Avg:   303 Min:   217 Max:   466 Err:     0 (0.00%)</span>
> </p>

<p class="p1">
   So this sounds kind of sophisticated, since we could do 288K Orders in 1 hours and have advertised for 50K. We were so wrong with that assumption and recognized this on 5pm on the 6th of December. Everybody had prepared their order and hit us instantly at 5pm to get their share. We peaked to more than 800 orders/s at which moment our API refused to handle more and sadly refused up to 30% of customer requests. So our tests have been proven worthless, since we just tested the average rather than the peak.
</p>

<p class="p1">
  Another issue has been, that we actually never hit the limits of the APIs, but rather the limits of the available tcp connections of the client. This never exceeded a limit in the API, that has been caused by a misconfiguration
</p>

> <p class="p1">
>   <property name=&#8220;minPoolSize&#8220; value=&#8220;25&#8243; /><br /> <property name=&#8220;maxPoolSize&#8220; value=&#8220;75&#8243; /><br /> <property name=&#8220;maxIdleTime&#8220; value=&#8220;14400&#8243; /><br /> <property name=&#8220;maxConnectionAge&#8220; value=&#8220;14400&#8243; />
> </p>

<p class="p1">
  The max allowed connections are far too low, since we can handle more than 6.000 simultaneous connections and <em>maxConnectionAge</em> as well for a load balanced environments. So we configured a soft limit ourself while the connections have not been evenly distributed through our cluster. Luckily that has been discovered early, so we had this configuration fixed and deployed by 5:20 pm, which recovered most of the situation. Since our DB servers haven&#8217;t been exhausted at all and didn&#8217;t even peak above our biggest business day, Sunday. Looking at those stats afterwards made me really mad, since we could have made it!
</p>

[<img class="aligncenter wp-image-997" src="http://www.artofcode.de/wp-content/uploads/2014/12/Bildschirmfoto-2014-12-08-um-14.22.10-300x150.png" alt="Bildschirmfoto 2014-12-08 um 14.22.10" width="418" height="209" srcset="http://www.artofcode.de/wp-content/uploads/2014/12/Bildschirmfoto-2014-12-08-um-14.22.10-300x150.png 300w, http://www.artofcode.de/wp-content/uploads/2014/12/Bildschirmfoto-2014-12-08-um-14.22.10.png 454w" sizes="(max-width: 418px) 100vw, 418px" />](http://www.artofcode.de/wp-content/uploads/2014/12/Bildschirmfoto-2014-12-08-um-14.22.10.png)

### **Deployments took to long** {.p1}

<p class="p1">
  As I just stated the configuration has been fixed and deployed by 5:20 pm, which is a dramatically long time for a not working service during the peak of the campaign. Since it was just a configuration change this should be possible to ship faster. The deployment can be done in one step and without downtime, but no hot deploy has been possible. The .war file had to be created and the jetty contexts rebooted, which consumed most of time and even though 4 servers we present no parallel deployment was implemented, rather serial.
</p>

### **Services didn&#8217;t gracefully degrade** {.p1}

<p class="p1">
  Our application is clearly separated to shut down services if one fails and recover gracefully. Problem is, that clients needed to be implemented the same way otherwise all good separations in the backend are worthless. Our mobile apps failed here which resulted in empty list. How come, since our restaurant list is statically available in a CDN and does not bother at all about the API dropping out? Easy answer, the apps were calling for available stampcards and restaurant vouchers for this list. This is to be received from the API and wasn&#8217;t reacting all the time. But instead of just showing the list without vouchers, no list was shown at all. The apps didn&#8217;t gracefully degrade it&#8217;s functionality, rather dropping out completely as well.
</p>

<p class="p1">
  This behavior will be changed in one of our next releases to avoid malfunctions because of a service failing, that is not needed to successfully complete an order.
</p>

<p class="p1">
  The same thing applies to our backend component. Sadly we didn&#8217;t check for a database connection to be available before confirming and checking out the PayPal payment. This resulted in unfinished order, because of a transaction rollback in the database, while the payment was kept intact. That actually caused the most harm for our customers and we are currently in the process of refunding all those successful payment which never reached the restaurants.
</p>

### **Overview of the applications** {.p1}

We are using LogStash and Kibana for centralized logging, but we don&#8217;t do it for all applications yet. So discovering errors isn&#8217;t always that easy, since you need to dig through some logs before you make sense of the things that are happening. Since we are moving ourself into a micro service environment we are having more and more the need for such a scenario. It already helped us discover that our service component will not scale with all the Adjust calls we got from the mobile apps about tracking event like &#8222;first installation&#8220;. This has been discovered before 5pm and was blocked as well so it dropped before it had an impact. We just logged the requests to replay those afterwards to not lose any data.

[<img class="aligncenter wp-image-1015 " src="http://www.artofcode.de/wp-content/uploads/2014/12/Bildschirmfoto-2014-12-09-um-15.45.04-1024x375.png" alt="Bildschirmfoto 2014-12-09 um 15.45.04" width="868" height="318" srcset="http://www.artofcode.de/wp-content/uploads/2014/12/Bildschirmfoto-2014-12-09-um-15.45.04-1024x375.png 1024w, http://www.artofcode.de/wp-content/uploads/2014/12/Bildschirmfoto-2014-12-09-um-15.45.04-300x110.png 300w, http://www.artofcode.de/wp-content/uploads/2014/12/Bildschirmfoto-2014-12-09-um-15.45.04-900x330.png 900w" sizes="(max-width: 868px) 100vw, 868px" />](http://www.artofcode.de/wp-content/uploads/2014/12/Bildschirmfoto-2014-12-09-um-15.45.04.png)

### **Legacy code has not been put under consideration** {.p1}

<p class="p1">
  What can I say, legacy is everywhere as soon as an application get&#8217;s used frequently and is evolving over time. In general legacy isn&#8217;t a bad thing if it is documented and maintained well. Actually there are two kinds of legacy. One, as derived from the words actual meaning, is something old but probably good, that is handed over to you. The other one is more like zombie baby you are forced to raise, but will bite you any time if you are not paying attentions 24/7.
</p>

<p class="p1">
  We found a biter in our middle, which has not been considered during a load test. In our case this have been the iPad apps, which are kind of old and still using the old PHP API, which isn&#8217;t designed to scale at all and transformed our web servers into a zombie itself, thriving to consume resources to survive.
</p>

> <p class="p1">
>   <span class="s1">[06-Dec-2014 17:16:53 UTC] PHP Fatal error:<span class="Apple-converted-space">  </span>Allowed memory size of 536870912 bytes exhausted (tried to allocate 512 bytes) in /home/xxx/live/lieferando.de/22/library/Zend/Db/Statement/Pdo.php on line 290</span>
> </p>
> 
> <p class="p1">
>   <span class="s1">[06-Dec-2014 17:16:54 UTC] PHP Fatal error:<span class="Apple-converted-space">  </span>Allowed memory size of 536870912 bytes exhausted (tried to allocate 512 bytes) in /home/xxx/live/lieferando.de/22/library/Zend/Db/Statement/Pdo.php on line 290</span>
> </p>
> 
> <p class="p1">
>   <span class="s1">[06-Dec-2014 17:16:55 UTC] PHP Fatal error:<span class="Apple-converted-space">  </span>Allowed memory size of 536870912 bytes exhausted (tried to allocate 81 bytes) in /home/xxx/live/lieferando.de/22/library/Zend/Db/Statement/Pdo.php on line 290</span>
> </p>

<p class="p1">
  Still it is available and therefore legit to use them for this campaign. We blocked those request using Nginx as our reverse proxy and responded with empty content right away.
</p>

> location /get_service {
  
> default_type text/html;
  
> content\_by\_lua &#8217;ngx.say(&#8222;&#8220;)&#8216;;
  
> }
> 
> location /get_customer {
  
> default_type text/html;
  
> content\_by\_lua &#8217;ngx.say(&#8222;&#8220;)&#8216;;
  
> }
> 
> location /get_meal {
  
> default_type text/html;
  
> content\_by\_lua &#8217;ngx.say(&#8222;&#8220;)&#8216;;
  
> }
> 
> location /get_order {
  
> default_type text/html;
  
> content\_by\_lua &#8217;ngx.say(&#8222;&#8220;)&#8216;;
  
> }

The load dropped instantly and allowed our backend to operate perfectly again. Also long running queries, that caused locks on important tables vanished.

### **Cleanup**

My father always told me, never leave a place messier than you found it. What are the odds he taught me something that has proven itself helpful for IT? If you do changes in short time, make sure you are cleaning up after yourself and while you are doing those changes them document them, those seconds don&#8217;t hurt and they are worth every dime once you need to recover and make sure that everything is back to where it belongs before you leave the office. We didn&#8217;t do that everywhere and crippled our landing pages, since we started the wrong version of html workers, generating our static html pages. Luckily this has been discovered early and fixed soon after.

### **[<img class="alignleft  wp-image-1021" src="http://www.artofcode.de/wp-content/uploads/2014/12/56970496-240x300.jpg" alt="56970496" width="413" height="516" srcset="http://www.artofcode.de/wp-content/uploads/2014/12/56970496-240x300.jpg 240w, http://www.artofcode.de/wp-content/uploads/2014/12/56970496.jpg 500w" sizes="(max-width: 413px) 100vw, 413px" />](http://www.artofcode.de/wp-content/uploads/2014/12/56970496.jpg)Learnings** {.p1}

<p class="p1">
  The time between 5 &#8211; 6pm on last Saturday wasn&#8217;t any good for our IT, but as usual we got our learnings and aiming to not fail a second time. We will improve our dependency coupling to <strong>avoid having cascading effects</strong> if one service is failing and changing our system to <strong>gracefully degrade</strong> if any of those remote services fail. We will improve our logging and we will not fail you another time.
</p>

<p class="p1">
  Also we are planing our load testing more reasonably, taking higher peaks into account and not just trying to build up assumption based on average numbers. Customers never evenly distribute over time, something we are familiar with, but sadly didn&#8217;t take into account.
</p>

<p class="p1">
  We will do such a campaign again, but we won&#8217;t disappoint you next time! We will be 200 OK
</p>

<p class="p1">
  <p class="p1">
    <strong>Update 05.01.2015: </strong>New Years is always our biggest day and we actually achieved to make more orders than on the free pizza day. With all the changes based on the here documented learnings we had no problems at all performance wise.
  </p>
</p>