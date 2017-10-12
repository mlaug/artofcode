---
id: 1052
title: 'Events &#8211; they are not the future'
date: 2015-03-26T12:00:49+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=1052
permalink: /2015/03/26/events-they-are-not-the-future/
categories:
  - Allgemein
---
With the strive for changing software architectures towards MicroServices events step even more into the focus of attention. Some developers have already been familiar with Event Driven Programming but not so with Event Driven Architectures like distributed systems of MicroServices in particular. Though logically both should be kind of the same approach, I experienced developers implemented things differently.

If we look on an Event Model in any monolith, we do things like notifying other parts of our system about things that just happened.

```java
public void login(){
    Publisher publisher = new Publisher();
    publisher.emit(new LoginEvent());
} 
```

While in a MicroService environment, where events become the essence of the entire system, they are degraded to commands instead of actual events (once again the Oxford Dictionary defines an event as: _A thing that happens or takes place, especially one of importanc_e) . I also have seen those kind of things in systems where only part of the application have been decoupled and communication is done via any kind of Bus or Queue system.

```java
public void sendEmailToCustomer(){
    Queue queue = new AmqpQueue();
    queue.setQueue("email_queue");  
    queue.send("maybe some json with the task to send out an email");
}
```

So what do we have here? We have a glitch in the matrix &#8230; while before we did tell the system what just happened, we now inform someone what has to be done. So we actually switched from Event Driven to Command Pattern. I assume this lies in the nature, we do not trust that others react upon our notes so we rather tell people to do something. Command Pattern is a bossy pattern and is as ineffective as micro management for distributed systems.

There is so much wrong with this&#8230;

  1. First of all MicroServices ain&#8217;t MicroServices if they are not loosely decoupled. In this sense Command Pattern makes no sense at all, since one service needs to know about what the other needs. That is **not** loosely decoupled.
  2. Events should be about what just happened not about what is to be done. **Events represent the present not the future**.
  3. Every time someone needs to inform other developers to change their events if they have changed or extended their implementation

[<img class="  wp-image-1055 alignright" src="http://www.artofcode.de/wp-content/uploads/2015/03/Command-Pattern-1-209x300.png" alt="Command Pattern (1)" width="264" height="379" srcset="http://www.artofcode.de/wp-content/uploads/2015/03/Command-Pattern-1-209x300.png 209w, http://www.artofcode.de/wp-content/uploads/2015/03/Command-Pattern-1.png 282w" sizes="(max-width: 264px) 100vw, 264px" />](http://www.artofcode.de/wp-content/uploads/2015/03/Command-Pattern-1.png)So let&#8217;s have a look at this picture. In the above example we have three programs (P1, P2, P3), which are sending requests to the fourth program (P4) to react upon and fulfil a task. Every time P4 changes it&#8217;s implementation we need to notify the developers of P1, P2, P3 to change their request if the requirement for the event has changed, e.g. additional data needs to be provided.

In the next example we still have three programs, but they all emitting different events (e1, e2, e3) and inform the system about **what just happened on their side**. P4 has attached to all those events, since those are events he wants to react upon. Now if P4 is changing its implementation it only has an impact on P4 because he has control.

Sure P1, P2 and P3 sometimes change their events as well, but I do belief that the Open/Closed principal doesn&#8217;t apply only to classes but also to the definition of events (no matter if json, protobuf or anything else). You can extend it any time, but you should never change what is already present. This way P4 will always work with the events from P1, P2 and P3 even if they have extended their events dramatically. P4 just doesn&#8217;t care, since the event, which was used during the implementation phase, is still a subset of the current event therefore still valid for P4.

So, yes, events are not the future, **they define the present**!