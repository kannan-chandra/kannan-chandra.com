---
layout: post
title: Creating Xamarin.iOS bindings for a native library
---

This past week I've been checking out [Appsee](http://www.appsee.com/), a cool app user analytics tool. The app I'm integrating this into is built using [Xamarin.iOS](http://xamarin.com/ios), which Appsee doesn't support out of the box. This means creating a C# binding for the Appsee static library. I got it working after some Googling, and I thought I'd put together a post documenting the process.

First head over to the Appsee site and grab the latest UIKit SDK. 

![](/images/xamarin-binding-appsee-1.png)

Unzip it and you should see `Appsee.framework`. This is just a folder that contains the static library and required headers and files in a specific folder structure, making it easy to add to an Xcode project. Appsee.framework contains the static library, called `Appsee`, and the header file, called `Appsee.h`.

![](/images/xamarin-binding-appsee-2.png)

First, we need to add the `.a` extension to the library to indicate that it is a static library.

![](/images/xamarin-binding-appsee-3.png)

Next, we need to generate the API definitions. This is the part where we define the C# methods and properties that are going to be available to you. You can do this manually if you want, but a really quick way to do it is to use a tool from Xamarin called [Objective Sharpie](http://docs.xamarin.com/guides/ios/advanced_topics/binding_objective-c/objective_sharpie/).

Download it and run it. Select the target version as appropriate. On the next screen, add the header file we saw earlier.

![](/images/xamarin-binding-appsee-4.png)

Enter a namespace that makes sense for you; I'm using `Appsee` here.

![](/images/xamarin-binding-appsee-5.png)

For the output file, I'm using the name `ApiDefinitions.cs`. You can use any name *except* `Appsee`. If the file and the interface have the same name, you're going to encounter errors down the line. The contents of the file look like this :

{% highlight c# %}

using System.Drawing;
using System;

namespace Appsee {

	[BaseType (typeof (NSObject))]
	public partial interface Appsee {

		[Static, Export ("start:")]
		void Start (string apiKey);

		[Static, Export ("stop")]
		void Stop ();

		...

	}
}

{% endhighlight %}

It's a normal C# class, and the bits in square brackets point to the Objective-C class/method/property you're wrapping with the following C#.

Objective Sharpie does a pretty good job, but there's some cleanup left to do. There are a couple of properties with a `Verify` attribute above them. 

{% highlight c# %}

[Static, Export ("debugToNSLog"), Verify ("ObjC method massaged into setter property", "/Users/kannan/Downloads/Appsee/Appsee.framework/Versions/A/Headers/Appsee.h", Line = 49)]
bool DebugToNSLog { set; }

{% endhighlight %}

`Verify` is an invalid attribute and will not compile. Objective Sharpie puts it there to draw your attention. In this case, it's because Obj-C setter methods were transformed into C# properties. This is alright, so go ahead and remove the `Verify`.

{% highlight c# %}
[Static, Export ("debugToNSLog")]
bool DebugToNSLog { set; }
{% endhighlight %}

There are also a few missing `using` statements. Add the following:

{% highlight c# %}
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
{% endhighlight %}

If you have a look at Appsee's page about the SDK, it lists a few builtin frameworks that Appsee uses.

![](/images/xamarin-binding-appsee-6.png)

We need to add `using` statements for these as well, so go ahead and do that :

{% highlight c# %}
using MonoTouch.AVFoundation;
using MonoTouch.CoreGraphics;
using MonoTouch.CoreMedia;
using MonoTouch.CoreVideo;
using MonoTouch.SystemConfiguration;
using MonoTouch.CoreAnimation;
{% endhighlight %}

The final `ApiDefinitions.cs` file looks like this :

{% highlight c# linenos %}
using System.Drawing;
using System;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
using MonoTouch.AVFoundation;
using MonoTouch.CoreGraphics;
using MonoTouch.CoreMedia;
using MonoTouch.CoreVideo;
using MonoTouch.SystemConfiguration;
using MonoTouch.CoreAnimation;

namespace Appsee {

	[BaseType (typeof (NSObject))]
	public partial interface Appsee {

		[Static, Export ("start:")]
		void Start (string apiKey);

		[Static, Export ("stop")]
		void Stop ();

		[Static, Export ("stopAndUpload")]
		void StopAndUpload ();

		[Static, Export ("pause")]
		void Pause ();

		[Static, Export ("resume")]
		void Resume ();

		[Static, Export ("debugToNSLog")]
		bool DebugToNSLog { set; }

		[Static, Export ("addEvent:")]
		void AddEvent (string eventName);

		[Static, Export ("addEvent:withProperties:")]
		void AddEvent (string eventName, NSDictionary properties);

		[Static, Export ("startScreen:")]
		void StartScreen (string screenName);

		[Static, Export ("overlayImage:inRect:")]
		void OverlayImage (UIImage image, RectangleF rect);

		[Static, Export ("userID")]
		string UserID { set; }

		[Static, Export ("setLocation:longitude:horizontalAccuracy:verticalAccuracy:")]
		void SetLocation (double latitude, double longitude, float horizontalAccuracy, float verticalAccuracy);

		[Static, Export ("locationDescription")]
		string LocationDescription { set; }

		[Static, Export ("markViewAsSensitive:")]
		void MarkViewAsSensitive (UIView view);
	}
}

{% endhighlight %}

We need to prepare another file that defines how linking should happen. I'm going to call this file `linkwith.cs`.

{% highlight c# %}
using MonoTouch.ObjCRuntime;

[assembly: LinkWith ("Appsee.a", LinkTarget.Simulator | LinkTarget.ArmV7 | LinkTarget.ArmV7s, ForceLoad = true,
	Frameworks = "AVFoundation CoreGraphics CoreMedia CoreVideo QuartzCore SystemConfiguration")]
{% endhighlight %}

The bit that says `Frameworks = "..."` is important. It contains the list of native libraries we need to link against. Without it, the creation of the binding will go alright, but there will be compilation errors when the resulting DLL is added to your project.

Let's get the three files, `Appsee.a`, `ApiDefinitions.cs`, and `linkwith.cs` into one folder. I'm calling this Appsee.

![](/images/xamarin-binding-appsee-7.png)

Open up your terminal and navigate over to the folder.

{% highlight console %}
$ cd /Users/kannan/Documents/Appsee
{% endhighlight %}

We use a tool called `btouch`, provided by Xamarin, to create the binding.

{% highlight console %}
$ /Developer/MonoTouch/usr/bin/btouch -unsafe --outdir=tmp -out:Appsee.dll ApiDefinitions.cs -x=linkwith.cs --link-with=Appsee.a,Appsee.a
{% endhighlight %}


The location of the btouch executable might vary depending on your installation. The `out` argument defines the name of the output DLL as `Appsee.dll`.

Running the above in your terminal will create the DLL file.

![](/images/xamarin-binding-appsee-8.png)

In your mobile project, right-click `References` and `Edit References...`. Navigate to the location of your DLL and add it to your project.

![](/images/xamarin-binding-appsee-9.png)

Add a `using` statement for the namespace you defined earlier, and you're good to go!
