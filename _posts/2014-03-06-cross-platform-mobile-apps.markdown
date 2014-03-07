---
layout: post
title: Creating cross-platform apps for mobile
---

There's been a lot written about which mobile platforms one should target when putting out a new app, and it comes down to either iOS or Android. Some apps, like single-player games or utility apps, can get away with launching on just one of these. But for a lot of other apps, launching on both platforms is crucial to gain traction quickly.

This is especially true of the app we're working on at [Ambi](http://www.ambiclimate.com). Ambi Climate is a device that sits at home and controls your air conditioner, and we provide a mobile app for you to interact with it. Being available on just a single platform will be a dealbreaker for most customers, because everyone in the household will have to be on the same platform. It's crucial that we cover as much of the market as possible at launch.

We evaluated several options early on. We could of course just bite the bullet and build 

Web apps 


- Importance of targetting both
- Web
- Native
- Xamarin
- Technically not a cross platform tool
- Share code
- One lang (don't have to think about two different memory management models)
- Able to leverage .NET libraries and community
http://blogs.msdn.com/b/dotnet/archive/2013/11/13/pcl-and-net-nuget-libraries-are-now-enabled-for-xamarin.aspx

Future post
- Sharing code
- Outlets and Actions to decouple view logic
- Native libraries
- .NET libraries