---
layout: post
title: "Is your App at Risk of Getting Kicked out of the App Store?"
date: 2016-11-02 08:00:00 -0400
tags: [iOS, levvel]
categories: [apple-dev]
---

_Note: this post originally appeared on [Levvel's Blog](https://www.levvel.io/our-ideas/is-your-ios-app-at-risk)_

While developers are thinking about Swift 3, Xcode 8, and support for iOS 10, app owners are considering whether it is the time to update their years-old iOS apps. Apple has just given app owners a great reason to consider updating their apps, [announcing](https://developer.apple.com/support/app-store-improvements/) that it is “cleaning up” the App Store by removing old apps that haven’t been updated in some time and will thus fail to pass the new app review process. If your app becomes non-compliant, Apple is giving you 30 days to update it. This process has the potential to affect a large number of apps, as [roughly half](https://david-smith.org/blog/2016/09/03/a-sizeable-cleanup/) of the apps in the App Store have not been updated in the last year. This post will walk you through the main points to consider when updating your app for iOS 10.

![ipad and iphone](https://cdn.levvel.io/blog_content/iOS10-Update-Blog-Post/ipad-and-iphone.jpeg)

## Swift Update

When you first open your app in Xcode 8, you'll be prompted to update the version of Swift your app uses. For the first time, you have two options: you can either make a smaller update to Swift 2.3 or leap all the way forward to Swift 3.0. By updating to Swift 3.0, you'll not only be more prepared for the next version of Swift but you'll also have access to Xcode 8’s new [thread sanitizer](https://developer.apple.com/videos/play/wwdc2016/412/) and [visual memory debugger](https://developer.apple.com/videos/play/wwdc2016/410/). The thread sanitizer helps surface difficult to find concurrency issues. The visual memory debugger graphs object ownership in your app, which can help you detect memory leaks. However, updating to Swift 3.0 is a more difficult update than moving to 2.0 for many reasons, one of them being the pervasive syntax changes. You'll also have to update your dependencies for Swift 3.0. Hopefully, the Swift version migrator runs flawlessly but, if it doesn’t, you can rerun the migrator. For a sufficiently large Swift project, however, you'll probably have to manually update your code, which is beyond the scope of this post.

## Objective-C Update

Since Objective-C hasn’t had any major changes in the past couple of years, you should be able to build and run your app in Xcode 8 without any errors, for the most part. However, you'll probably see new warnings about nullability annotations and lightweight generics. [Nullability annotations](https://developer.apple.com/swift/blog/?id=25) let the compiler and other developers know which pointers may or may not be null. This also allows your swift code to use optionals for pointer types defined in your Objective-C code that are annotated as nullable. Lightweight generics allows you to specify the type of objects inside of a collection. Adopting both of these changes can be tedious but they should be a priority—not only because they allow your Swift code to better interoperate with your Objective-C code—but because they will also make your code more explicit and safer.

## Update Deployment Target

Your next decision when upgrading your app is which minimum iOS version you want to target. This is different from the base SDK, which should be set to ‘Latest’ and, consequently, will be iOS 10 in Xcode 8. Because iOS users update their devices so quickly, the general consensus and Apple’s recommendation is to target the last minor version of the previous major release. Since iOS 10.1 is out, that means your app’s deployment target should be 9.3. With the new deployment target set, be sure to remove code that references previous iOS versions through system version checks. While we’re on the subject of system version checks, make sure your code doesn’t check for the system version like this:

<pre style="white-space: pre-wrap;  white-space: -moz-pre-wrap; white-space: -pre-wrap; white-space: -o-pre-wrap; word-wrap: break-word;"><code class="hljs undefined">
#define IsIOS7 ([[[[UIDevice currentDevice] systemVersion] substringToIndex:1] intValue]>=7)
</code>
</pre>

This will return false for iOS 9 but true for iOS 10, because it only compares the first character of the returned system version. This is apparently as [widespread](https://github.com/search?q=%255B%255B%255BUIDevice+currentDevice%255D+systemVersion%255D+substringToIndex%253A1%255D&type=Code&utf8=) as the [Windows system version check](http://www.pcworld.com/article/2690724/why-windows-10-isnt-named-9-windows-95-legacy-code.html) looking for Windows 95 or 98.

## Find and Fix New Warnings

When you compile and run your app, you'll most likely see some new warnings about deprecated methods, but hopefully your app runs the first time. If it exits on launch, check the console for a line similar to this:

<pre style="white-space: pre-wrap;  white-space: -moz-pre-wrap; white-space: -pre-wrap; white-space: -o-pre-wrap; word-wrap: break-word;"><code class="hljs undefined">
The app's Info.plist must contain an NSPhotoLibraryUsageDescription key.
</code>
</pre>

Usage description keys have been around awhile but, starting with iOS 10, Apple requires all systems prompts for access to the user’s data to include a usage description. If your app doesn’t include the corresponding description for a system prompt, it will exit on launch. Simply add a usage description key for each framework that accesses user data to your app’s info.plist file. If you already included usage description keys in the past, then be sure to also check out the [new required usage description keys](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html). Note that your app might not exit on launch if a dependency in your library links against one of these frameworks, but you still need to include a usage description; otherwise, your app will not pass validation when uploading to iTunes Connect.

On the topic of dependencies, now would be a good time to update those as well. If you are using CocoaPods, update your Podfile with your deployment target and then run ‘pod update’ to fetch updates for your pods.

Before you start working your way down the list of new warnings, first update to the recommended Xcode project settings by selecting ‘project’ in the sidebar, then in the menu select Editor → Validate Settings…, then Perform Changes. Among other things, this will turn on new compiler checks that will generate new warnings. After you have fixed the resulting warnings, go ahead and run the static analyzer on your project, since it includes new checks in Xcode 8.

In addition to fixing deprecated methods, I ran into some other warnings when updating my iOS 7 app to iOS 10, such as a crash when presenting UIActivityViewController on iPad. If you try to present a UIActivityViewController without specifying a popover presentation controller source view in iOS 8 or newer, your app will crash and produce the following log within the console: 

<pre style="white-space: pre-wrap;  white-space: -moz-pre-wrap; white-space: -pre-wrap; white-space: -o-pre-wrap; word-wrap: break-word;"><code class="hljs undefined">
Your application has presented a UIActivityViewController &lpar;&lt;UIActivityViewController: 0x7ffd88c6c730&gt;&rpar;. In its current trait environment, the modalPresentationStyle of a UIActivityViewController with this style is UIModalPresentationPopover. You must provide location information for this popover through the view controller's popoverPresentationController. You must provide either a sourceView and sourceRect or a barButtonItem.  If this information is not known when you present the view controller, you may provide it in the UIPopoverPresentationControllerDelegate method -prepareForPopoverPresentation.
</code>
</pre>

Fixing this is usually as simple as what the log suggests. In my case, I set my UIActivityViewController popoverPresentationController’s sourceView to the button that calls up the Activity View Controller.

Another issue I encountered related to launch storyboard requirements. iOS 8 introduced launch storyboards, which allow you to specify how your launch image displays across all supported iOS device sizes with a single adaptive storyboard. This will save time in the long run as Apple introduces new device sizes, but if you are not ready to make that investment you can opt out of it on iPad-only apps with the ‘UIRequiresFullScreen’ info.plist key.

Once you’ve updated your deployment target, updated your dependencies, and fixed your warnings, you’re ready to upload your app to iTunes Connect. I would suggest, however, that you take the time to go beyond SDK compatibility and strive for taking advantage of the updated SDK. 

## iOS 8 SDK

The biggest change that the iOS 8 SDK brings to the table is adaptivity. Adaptivity allows developers to specify how their app should best fill a certain screen size through AutoLayout and new adaptivity APIs handle the rest. Developers no longer have to maintain different files for iPhone and iPad layout. While the new adaptivity APIs are powerful, they can take a significant amount of time to adopt—especially if your app doesn’t use AutoLayout yet. Before iOS 8, the only screen size change introduced to iOS came as a result of the extra height of the iPhone 5 and iPhone 5s devices. These were relatively simple to support since most content on iOS scrolls vertically. When the iPhone 6 and 6 Plus devices arrived, they changed not only the height but also the width of the screen. Now developers have to consider how their layout and especially their text layout expands and truncates with the different sizes. Designers also have to get involved by cutting new 3x assets for iPhone devices with 5.5-inch Retina HD Displays. Note that Apple still accepts iPhone 5-only sized apps in the App Store.

If you’re not ready to take on this change, your app will simply expand to fill the screen. However, users will notice that you are not taking full advantage of their screen size especially when they try to type on the blown up keyboard they are unaccustomed to seeing in other appropriately sized apps. Also, no one knows how much longer Apple will allow updates for iPhone 5 only sized apps in the App Store.

## iOS 9 SDK

The iOS 9 SDK continued with the theme of adaptivity from the iOS 8 SDK by bringing support to iPad. Since iOS 9 allows iPad users to use apps in split screen and slide-over modes, developers must allow their app to gracefully fill these new sizes. Like before, Apple will still accept apps that don’t support these new modes, but no one knows for how long.

3D Touch was another enhancement that Apple introduced with iOS 9. It allows for new peek and pop pressure sensitive options. Thankfully, these are relatively easy to support for vanilla UIKit applications.

## iOS 10 SDK

This year’s iOS 10 SDK is relatively easy to support, especially since no new device screen sizes were introduced with it, though you do have to contend with App Transport Security (ATS) starting in the new year. ATS is a security measure by Apple to ensure that your app communicates securely with its back end. In order to meet its security requirements, your app must use HTTPS and TLS version 1.2 with forward security. ATS was easily ignored when it was introduced in iOS 9 by setting the NSAllowsArbitraryLoads key to YES under the NSAppTransportSecurity dictionary key. This prevents the alert below from popping up. However, starting January 1, 2017, all apps submitted to the App Store are required to use ATS. If you foresee this being an issue for your app, you’ll want to get your app update out before this requirement hits.

![error message](https://cdn.levvel.io/blog_content/iOS10-Update-Blog-Post/iOS10-blog-error.png)

## iTunes Connect

Before you submit your update to the App Store via iTunes Connect, you might need to guard against roadblocks, such as code signing. If it has been a year since your last update, then your app signing setup is probably expired. Thankfully, Apple has further improved its ‘Fix Issue’ button by adding Automatic Code Signing with Xcode8. Previously, this button might overwrite a previous provisioning profile that was correctly set up on a coworker’s machine and thus render the coworker’s code signing setup useless. This brought on so much frustration that a developer made an Xcode plugin to disable this button entirely in order to prevent wasted afternoons of resetting of provisioning profiles. With Xcode 8, Automatic Code Signing instead uses a dedicated profile managed by Xcode, which means you can turn on Automatic Code Signing without worrying about messing up your coworker’s signing setup after checking in your project settings file. Just go to your project’s target and tick on the Automatic Code Signing checkbox shown below and then click ‘Enable Automatic’ to accept the changes.

![manage](https://cdn.levvel.io/blog_content/iOS10-Update-Blog-Post/iOS10-blog-manage.png)

![reset](https://cdn.levvel.io/blog_content/iOS10-Update-Blog-Post/iOS10-blog-reset.png)

With code signing fixed, you can now create a new version of your app in iTunes Connect, build an archive, and upload your app. If everything went okay, then you’ll be ready to submit your app for review. If your app uploaded to iTunes Connect but you don’t see the build, be patient. If you still don’t see your build after 30 minutes, check the developer email address associated with your app. When I tried this with my app, I discovered that one of my dependencies had linked against a framework that requires a usage description key for privacy purposes. If you run into issues like this, you’ll need to increase your app’s build number (not its version number) after you fix the issue and upload it again in order for iTunes Connect to see the new version of your app. 

Even now, just as you’re ready to hit the ‘Submit for Review’ button, you might hit one or two more snags. In recent years, Apple has reduced the number of allowed special characters in your app description. Simply remove those restricted characters and move on to the screenshots, if this applies. If you’re submitting an iPad compatible application, iTunes Connect needs an iPad Pro screenshot. This shouldn’t take long to do well, but if you’re in a real hurry, Apple will accept blown up versions of your existing iPad screenshots.


## Conclusion


Getting your app up and running with the iOS 10 SDK might take a little more time and require extra attention, but it’s worth it. Not only will you get to keep your app in the App Store, but users will appreciate the speed and reliability that comes with building your app against the latest SDK. 

