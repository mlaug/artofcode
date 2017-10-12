---
id: 1036
title: 'Software &#8211; they ain&#8217;t gonna use it as expected'
date: 2015-03-03T08:35:38+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=1036
permalink: /2015/03/03/software-they-aint-gonna-use-it-as-expected/
thesis_thumb_width:
  - "66"
thesis_thumb_height:
  - "66"
categories:
  - Software
---
<img class="alignleft" src="http://i.imgur.com/oCuT0vu.jpg" alt="" width="343" height="294" />It is like a rule, it always happens, you can&#8217;t hide from it, it will find you&#8230;

No matter how smart you are designing your service or the UI of your application. The user will end up with weird ways of using it, I guarantee you! Apparently it is the way the human brain is defined, otherwise I cannot make sense out of it, since it doesn&#8217;t happen only to software. Even in the wild people do see intend in things where others don&#8217;t.

Maybe here is the reasoning for IT designing weird error messages, since they just don&#8217;t get how the customer might react. It is like the US law forcing companies to come up with messed up rules on their products to avoid complete misuse. Ever heard about the &#8222;cat microwave lawsuit&#8220;? No, than you belong to the 26% of people that don&#8217;t know about it, but it&#8217;s fine, it is an [urban myth](http://www.certifiedcoolness.com/coolnessinstitute/global-surveys/95-76-believe-in-the-microwaved-pet-lawsuit-in-the-usa.html){.broken_link}, while still a good example for weird &#8222;please don&#8217;t use it that way&#8220; sentences in some instructions.

So what it is with instructions in the first place. A common rule in UI design is: **&#8222;if you need to explain it you done it wrong&#8220;**. But this rule has been most of the time sarcastically interpreted as: **&#8222;you need to dumb it down for the user&#8220;**. This assumption &#8211; since as we know assumption are the root of all evil &#8211; leads directly into the user experience hell.

**First** of all the user isn&#8217;t dumb, he or she just tends to misinterpret things that are provided by the developers in the first place. So _dumbing things down_ should actually resolves to _removing things that confuse or distract_. The devil lies in the detail therefore simplicity isn&#8217;t something that can be simply achieved. Jeff Atwood describes it very nicely in his blog post about &#8222;[the god login](http://blog.codinghorror.com/the-god-login/)&#8222;, a login box and how much effort his team has put into that to make it easy to use and understandable. At the end it is just a login box, those nasty things that a developer needs to implement multiple times in his career. But it causes agony to the users if done wrong!

**Second**, the misusage of software might actually give a hint about the absence of certain functionalities. Happened to my team, when we introduced the possibility to assign restaurants to a franchise. It didn&#8217;t take long and franchises like &#8222;normal&#8220; and &#8222;no contract&#8220; or &#8222;testing phase&#8220; have been introduced. While someone else needed this feature to group chains together to create one invoice others saw their chance of finally grouping restaurants to query them more easily for statistical purposes.

**Third**, never assume, test! _Assumption is the root of all evil_ and you should never expect that a customer has no other choice but using the software this way. Test the shit out of your platform and you will be wining about that customer tend to like the pink buttons more than the green ones, even though the latter ones have been suggested by a UX guru. Don&#8217;t worry about it, behavior isn&#8217;t rational most of the time and so you are incapable by definition to release something which you have scientifically proven to be understandable and working fluently. No chance! Do A/B testing and react upon the results.

**Fourth**, don&#8217;t belief your customers &#8211; Ok &#8230; ehm what? In the last paragraph I stated clearly to go out and react upon customer behavior. And now I am urging you to **not** trust them in the first place? Sure &#8230; no really this makes perfect sense, because you should never be biased or blinded because of numbers. Those numbers might lie and give you a wrong feedback, so always revalidate and take a look on the long term impact once you have changed your buttons to shiny pink because of the success of the last A/B test on mothers day.

Last but not least, don&#8217;t be frustrated about that situation, think about it from another perspective: Users **are** using your software, that is something good. Just accept and deal with the weird user behavior as valued user feedback, even though you need to interpret it. Listen to the noise of the crowd!