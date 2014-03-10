---
layout: post
title: Creating cross-platform apps for mobile
---

There's been a lot written about which mobile platforms one should target when putting out a new app, and it comes down to either iOS or Android. Some apps, like single-player games or utility apps, can get away with launching on just one of these. But for a lot of other apps, launching on both platforms is crucial to gain traction quickly.

This is especially true of the app we're working on at [Ambi](http://www.ambiclimate.com). Ambi Climate is a device that sits at home and controls your air conditioner, and we provide a mobile app for you to interact with it. Being available on just a single platform will be a dealbreaker for most customers, because everyone in the household will have to be on the same platform. It's crucial that we cover as much of the market as possible at launch.

We evaluated several options early on. We could of course just bite the bullet and build two apps natively. This would mean using the development kits from Apple and Google and writing the apps in Objective-C and Java respectively. This approach gives us full access to every feature available on each platform, and the ability to build a native and responsive user interface. The downside, of course, is having to build two entire apps. Two codebases to maintain, twice as many bugs, and we're going to have to solve the same problems twice.

On the other end of the scale are web apps. We could write a single application using web technologies like HTML and Javascript, and users can access the app using their browser. There are also tools such as [PhoneGap](http://phonegap.com/) that allow you to wrap up the web app into a native app that can be installed on the phone, and even access some hardware functions such as the camera. Upside: we can target any platform with a browser, and roll out native apps for the large number of platforms PhoneGap supports. The downside is a degraded user experience, because none of the apps will take advantage of the visual conventions of each platform. For Ambi Climate, specifically, we require access to the Wifi network list to do device setup, which is not accessible with a web app.

What we're using at Ambi is [Xamarin](https://xamarin.com/). Using [Mono](http://www.mono-project.com/), an open source version of Microsoft's .NET framework, allows you to code in C# for any platform. Xamarin provides wrappers around both the [iOS](http://xamarin.com/ios) and [Android](http://xamarin.com/android) platforms, allowing you to access all the native APIs of each platform entirely using C#. This means you can build fully native UIs, since you have access to the standard Views on Android and all the UIKit classes on iOS.

The thing to understand is that Xamarin is not a cross platform tool. It's a tool that allows you to write Android apps in C#, as well as write iOS apps in C#. You still write two individual apps. Writing them in the same language, though, allows you to do some interesting things.

First, you get to share a lot of common code. If you separate the UI layer from the code that handles the application logic, models, networking and data storage, you can reuse all of those classes in both projects. When you're done with one platform, all you need to do for the other is to write the application layer. Any changes you make to any other layer can stay synced across both projects.

Secondly, you get to leverage the .NET platform and community. The C# language and .NET libraries have a lot of great features that can make development a lot smoother. I'm personally in love with [`async-await`](http://msdn.microsoft.com/en-us/library/hh191443.aspx). There's also a huge number of great [third party .NET libraries](http://blogs.msdn.com/b/dotnet/archive/2013/11/13/pcl-and-net-nuget-libraries-are-now-enabled-for-xamarin.aspx) you can use in your project. What's better is that Xamarin gives you an easy way to also use any third party [iOS](http://docs.xamarin.com/guides/ios/advanced_topics/binding_objective-c/) or [Android](http://docs.xamarin.com/guides/android/advanced_topics/java_integration_overview/binding_a_java_library_(.jar)/) library in your project. We recently used this to [integrate Appsee's analytics platform](http://kannan-chandra.com/posts/xamarin-binding-appsee/) into our app.



