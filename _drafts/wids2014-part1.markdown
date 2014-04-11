---
layout: post
title: WIDS 2014 - Part 1
---

I can mark "attending a conference" off my todo list now. There are an increasing number of monthly dev meetups in Hong Kong, but WIDS (the World Internet Developer Summit) is the only conference. Sure, the name's a bit bombastic and it sounds like a Justice League of developers coming together to save the Internet, but it's a proper conference! Speakers from big name companies! Multiple tracks! A name tag!

IMAGE
Okay, they just put my name card into a card-holder, but it counts.

The first day was at Cyberport. The previous time I was here was for the Angelhack hackathon, and it's a really beautiful tech startup campus hub thing. When I joined the line, I was pretty sure we were waiting to enter the auditorium where the talks happen. Turns out, the arena we were lining up at **is** the venue.

IMAGE
"Are you not entertained???"

I've noticed that Hong Kong events at Cyberport and Science park tend to be preceded by a bit of a report card on how Hong Kong is doing with the startup thing. Speakers from the various organizers came up to remind us about the awesome internet speed here, and the increasing number of startups getting funding from the States.

The first talk was from Microsoft about Azure services. Not that much of interest happened, really. There were brief attempts to link what was being presented to general cloud concepts, but it was pretty much just an hour-long ad. There was a weird five minutes where I think they were just showing us animated graphs on Excel. They completely flew past the only interesting part of the demo, which was when they showed a natural language query interface that allowed you to draw arbitrary analytics by just phrasing it in plain language. It was like Youtube where you have to wait for the sponsored ad to end before getting to the good stuff.

IMAGE
Not pictured: Adblock.

The next talk was just a complete switch to full on tech mode. Asya Kamsky spoke about replication in MongoDB. I had a vague understanding of replication before, but her talk went into some practical implementation details. A replica set is a collection of servers (usually three) that store separate copies of the same data. One is a primary, and is the one used for both reads and writes in general. The others are secondaries that try to keep up with the state held at the primary. In the event of a failure at the primary, the remaining nodes hold an election and promote one of them to be the new primary. One thing I found cool is that when the old primary does come back, there are mechanisms to allow it to join the cluster and turn into a secondary. The same mechanism allows for downtime-less maintenance. Something I had not thought about before is that replica sets can be useful for running other background tasks such as backups and analytics on one of the secondaries. Overall a really good talk. The one question I had was that these concepts do seem fairly general. What makes MongoDB more suited to this than a relational DB?

Maxime Bélanger's talk was one of my favourite from the conference. The talk was primarily about how Dropbox hacked the OSX Finder to display the classic syncing overlays on top of file icons. The level of modification is pretty insane, and I was quite surprised the extent to which Dropbox messes with the Finder. The core idea is to write low level Darwin code that gets a foreign process (the Finder process) to run code from a custom memory location you have allocated. This allows you to run a bit of bootstrap code which you can use to link against an actual dynamic library. At this point, you can finally start working with the Objective-C runtime, and swap out the implementation of Finder methods with your own method. Dropbox uses this trick to add the syncing state to the file data, as well as swap the built-in icon rendering method with their own.

The whole idea is both exciting and really worrying. This is the level of control you are handing over to any application which asks for your administrator password when being installed. Not only does the app have full access to the entire file system (which you'd expect), it can also get other processes to change their built-in behaviour. The second half of the talk was a summary of some general lessons from this exercise. First, doing something this crazy gives you an inherent competitive edge. Not everybody is this crazy, and your competitors probably won't go this far. Second, anything is possible. Literally anything. If you want you can make a computer do anything you want it to do, whether or not the framework, operating system, programming language allow you to do it. You just need to go a level lower and bend the machine to your will.

"Everything is just C code."

Working at such a low level comes with some baggage. You need to minimize your dependence on reverse engineered libraries as much as possible. This will be the first thing to break when the underlying platform changes, because none of the functionality you are depending one was meant to be outward facing. Counter intuitively (to me, anyway), it can be better to trust low level code more than high level. Kernel level changes are much rarer than high level changes. As an example, the Finder implementation has been revamped 3-4 times during Dropbox's existence, but the underlying Darwin implementation only changed once.

The talk after lunch was by Josh Aberant, postmaster at Twitter. Yes, apparently that's a thing. This session was probably the least technical talk from both days. That's not a bad thing, though. It turned out to be a really interesting one. The main thrust of the talk was "growth hacking".

IMAGE
"Growth Hacking", the artist formerly known as "Marketing"

The three primary channels he covered were email, push, and SMS. I'd never thought of push notifications as a marketing channel, but the key takeaway from this session was keeping the customer's entire lifecycle in mind, from the first time they hear about the product to when they stop using it. Every stage of that journey is an opportunity to lead the customers to different objectives, and each channel excels at different stages of that journey. The stages, broadly, are acquisition, activation, engagement, virality and resurrection. The non-obvious ones to me were activation and resurrection. Activation is turning a customer you just acquired (registration, download of a free trial, etc) into an active user. An example Josh cited was that Facebook found that once you hit a certain number of friends, you wouldn't churn. So Facebook's objective at that point is to get you to add connections and hit that point of no return, where you are guaranteed to be an active user. Identifying the causes and effects within your customers' lifecycle can help you figure out the transition from one stage to another. The other stage, resurrection, is bringing back a user who has gone inactive. Again, being able to anticipate that situation gives you a chance to reactivate the user before they even leave.

For me, the big takeaway is to think of marketing in terms of the customer's lifecycle. It's natural to strategize in terms of where the company is in its own lifecycle ("Right now, what we need is to generate awareness among early adopters", "We need to broaden the appeal of the product to allow the non-tech crowd to start using it", etc). But you get from hundred users to a thousand one at a time. Nine hundred separate people went from being ignorant of your product to becoming an active user (or a paying customer). None of them care about where the company as a whole is. Keeping each individual customer's journey in mind is a prerequisite to growth.

Up next was the panel, about Big Data and the Cloud. Big Data, if I understand correctly, is when the default font size of PHPMyAdmin is set too high. This can cause many problems including not being able to see all of it at once. This can Cloud your understanding of what it all means. In 2013, Social Media would have solved that, but teenagers don't use that anymore. It's a pity they didn't talk about Bitcoin as well; I was this close to beating my Buzzword Bingo highscore.

Day two of the conference was held at the other big startup/tech campus, the Science Park. Getting there was three trains and a bus away, so that was a bit of a bummer. The "Getting Here" section of their site says they are TODO FILLED IN INCOMPLETE NEED TO FIND THE QUOTE. No, Science Park. You are not in the center. Yes, if we dug up all the soil in Hong Kong you'd be near the center of gravity, but the *people* live over *there*. That's like TODOFILLIN, Nebraska billing itself as "Strategically equidistant from both New York and San Francisco".

I spotted a funky looking giant golden rugby ball thing on the way in and took a photo of it. Asking for directions, I was told that *that was the venue*.

IMAGE
#selfie #nofilter #nomakeup #groupphotoofeveryonewhoattendedifyouthinkaboutit

The first session was by Markku Lepisto from AWS. Broadly, it was a slightly more interesting version of the Azure talk that started off the first day. Atleast there was an attempt to link the Amazon services being shown off to broader cloud concepts. The demo at the end of the session was pretty neat. It was a live demo of the self healing ability of their services. The demo website was being served from both Tokyo and Singapore. When the primary server in Singapore was killed, the automatic health check failed and redirected all traffic to Tokyo within a few seconds of downtime. At the same time, the Singapore server automatically rebooted and took over the traffic once it passed the health checks. Pretty neat.

Next was TODOJeremy from Evernote. The talk covered how Evernote used localization to grow internationally (they have more users outside the States than inside). The key takeaway for me from this one was using your API as a localization strategy. A growing startup may be able to get the UI translated to a bunch of languages. But localization goes beyond that, as the app will encounter new use cases that really only arise in that country. Having an API and promoting your platform devs allows them to fill those gaps and bring your platform to a completely new set of users. In Japan, people even wrote loads of guidebooks about using Evernote when they localized there.




