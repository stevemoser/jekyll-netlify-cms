---
layout: post
title: Designing and Developing for iPhone X
categories: iPhone
---

![Levvel Branded iPhone](https://cdn.levvel.io/blog_content/Designing%20and%20Developing%20for%20iPhone%20X/Designing%20and%20Developing%20for%20iPhone%20X-c5SzfSJ4vf4uNKJjvDl2mhyK1fMlYpKu.jpeg)

# Designing and Developing for iPhone X

_Note: this post orginally appeared on [Levvel's blog](https://www.levvel.io/our-ideas/Designing-and-Developing-for-iPhone-X)_

The iPhone X is a large departure from previous forms of iPhone devices—one that will be a disruptive force in the digital device marketplace in the months to come\. In order to keep up with the changing space, developers and designers creating 3rd\-party applications for the iPhone X must be able to adapt their applications to the new version’s APIs while still supporting older devices\. One of the best ways to prepare for this is to become well acquainted with the X, including the changes in its screen, gestures, and buttons, and with the updated [iPhone User Guide](http://help.apple.com/iphone/11/#/) to find out where functionality has moved in the new version\. 

App creators should also check out how Apple’s native apps and leading 3rd\-party apps have been updated for the new version\. The following section offers some highlights on the new changes in the iPhone X, and what designers and developers should keep in mind as they create applications for this device\.

## Screen

![iPhone X Corner](https://cdn.levvel.io/blog_content/Designing%20and%20Developing%20for%20iPhone%20X/Designing%20and%20Developing%20for%20iPhone%20X-tTBb1vc4Zb7aBaOrm8ZmdBwZkgCcxHRo.png)

The new edge\-to\-edge screen is the headlining feature of iPhone X\. You should get acquainted with the resulting safe areas because of the new rounded corners and the sensor housing\. You should also become familiar with the new aspect ratio, OLED pixels, and true 3x resolution\.

## Safe Areas

The all new Super Retina display doesn’t have a single right angle on it, including around the notch at the top, which Apple refers to as the sensor housing\. The hardware seems to melt into the software, especially at the bottom where the curve of the phone’s corners, display corners, dock corners, and app corners all match\.

To extend this rich aesthetic to your app, Apple recommends a 16pt radius to match the main corners\. Also, use the safe area and layout margins provided by UIKit to make sure content and controls are not occluded by the corners or the sensor housing\. When your background content does reach the corners you don’t have to worry about pixelated edges since Apple handles the anti\-aliasing for you\. The safe area layout guide also provides backward compatibility for your apps running on older devices by reducing their size to zero \(since older devices have no unsafe areas to avoid\)\. 

If your app already uses UIKit controls and bars along the top and bottom, you will have a much easier time updating your app for iPhone X\. In fact, if you are using storyboards, it can be as easy as checking the ‘Enable Safe Area Layout Guide’ checkbox to perform an automatic migration of your constraints to use the safe area layout guide\. Be sure to fix any constraints to use the superview instead of the safe area layout guide or vice versa\. Typically if a view has edge\-to\-edge background content without controls, like a header parallax image, then you will want to use a superview in a constraint\. Otherwise, it is best to stick with the safe area layout guide\. Of course, any top or bottom layout defined in code will have to be updated, as no automatic code migration is provided for the safe area layout guide\. For more information about the safe area layout guide checkout [Use Your Loaf’s post](https://useyourloaf.com/blog/safe-area-layout-guide/)\.

## Keyboard

The new screen also changes the keyboard height\. With the edge\-to\-edge display, the keyboard was made much taller in order to keep the keys in an ergonomic position, so be on the lookout for code that assumes the height of the keyboard\. Even if your code doesn’t make assumptions about the height of the keyboard you will probably have to update your UITextField and UITextField’s inputAccessoryView so that content appears within the safe area\. You’ll also have to update it so that your bar doesn’t run into the indicator when an external keyboard is connected, since in that case it would be at the bottom of the screen\. If you’re using the popular IQKeyboardManager library you’ll want to update to the latest version, as it fixes many of these issues\.

![iPhone X keyboard bar error](https://cdn.levvel.io/blog_content/Designing%20and%20Developing%20for%20iPhone%20X/Designing%20and%20Developing%20for%20iPhone%20X-ueHdDpIkfUf1rAdOpYgLYDAHgJQppYjC.png)

## Aspect Ratio

The new display also brings a new aspect ratio \(19\.5:9\) to iOS devices \(this also means there is no Display Zoom support because Display Zoom requires taking advantage of developers supporting the same aspect ratio on smaller devices and there is no smaller 19\.5:9 device\)\. 

The new aspect ratio shouldn’t matter too much if you’re correctly resizing your app for every screen size\. However, it might impact you if you are making assumptions about the aspect ratio for scrolling content or have a full\-screen image that is relying on a certain aspect ratio\. For example, with iOS 10 only iPhone devices with 16:9 displays were supported\. 

I came across one app that changed the translucency of the navigation bar over a header image based on what percentage of the screen was scrolled\. With the new ratio of iPhone X, that calculation has to incorporate the aspect ratio of the screen as well\.

Also, be sure that either an aspect fill of your existing image doesn’t cut off any important content, or provide a new asset\. The latter is prefered since iPhone X has the first true 3x display of any iOS device\. The plus\-size phones render a 3x Retina image scaled down by 1\.15\. In any case any non full\-screen 3x assets used for the plus\-size devices will work on iPhone X\.

## OLED

If your app provides a dark theme with a dark gray background, consider adding another dark theme with true black\. True black looks much better on the OLED Super Retina display because the contrast is much higher for this color theme\. Also consider a true white background if your app has an off\-white tint theme\. This is because users that opt to use the new Smart Invert feature will see true black if they have that feature enabled\.

## Smart Invert

Smart Invert inverts the colors on all views except those marked by a developer or UIKit as “not needing inversion”\. For example, a UISwitch would turn from green to purple with Classic Invert, but with Smart Invert it stays green, which ends up looking like the Clock app\. Also, with Smart Invert, if a screen is dark by default like the Clock app or the movies tab in the iTunes app, developers can make it stay dark\.To support Smart Invert, developers should check out the [accessibilityIgnoresInvertColors property on UIView](https://developer.apple.com/documentation/uikit/uiview/2865843-accessibilityignoresinvertcolors)\.

## Top Notch

One word on the ‘Embrace the Notch’ debate\. I agree with Apple that the notch should be embraced for two reasons\. First, because it reduces the wear and burn\-in on the Super Retina display and second because the rest of iOS embraces the notch\. Any 3rd\-party app that doesn’t embrace the notch will look out of place, as it gives the device a non\-symmetrical look that isn’t seen anywhere else in iOS\.

![From Apple’s ‘Designing for iPhone X’ video](https://cdn.levvel.io/blog_content/Designing%20and%20Developing%20for%20iPhone%20X/Designing%20and%20Developing%20for%20iPhone%20X-eoU4aDJEhXNYf3JO9Y9D5EbU34HW73pX.jpeg)

From Apple’s ‘Designing for iPhone X’ video

## Gestures and Buttons

In the iPhone X, the Home button is gone and its duties have been divided between what Apple calls the indicator \(the long pill shape at the bottom of the screen\) and the renamed side button\. Previously Apple called this side button the Wake/Lock button\. It seems likely that they are taking a cue from the Apple Watch since image push notifications and a double click of the side button to confirm Apply Pay eventually made their way to iPhone\.

In any case, designers and developers will have to be mindful of the indicator at the bottom of the screen\. Although Apple allows developers to hide it for viewing full\-screen content like photos and videos, the indicator should rarely be hidden for at least two reasons\. 

First, it is a buffer and a reminder for designers and developers to not fill the bottom of the screen with controls since the area around the indicator can we used to swipe left and right for multitasking\. Second, it shows the orientation of the device, which is especially important if no text is on screen to hint at the orientation\. The orientation is important because swiping from the bottom of the current orientation of the screen is the only way to go to the Home screen\. 

In [Casey Neistat’s review of iPhone X](https://youtu.be/-7dTzc8kTOY?t=2m21s) he stated that the DJI app froze on him and because iPhone X has no hardware Home button he wasn’t able to exit the app\. I believe what happened is that the DJI app hid the indicator while in landscape mode\. When Casey was swiping from the bottom of the phone instead of the bottom of the screen orientation \(which is the left side of the screen here\) the indicator became unhidden on the left side of the screen \(which you can see below\)\. Swiping from the left or right edge of the device according to the screen orientation does nothing and thus Casey thought that app was frozen and couldn’t get to the Home screen\. I verified that blocking the main thread doesn’t prevent the swipe\-up\-to\-go\-Home gesture from working by creating a test app\.

![Casey Neistat’s review of iPhone X](https://cdn.levvel.io/blog_content/Designing%20and%20Developing%20for%20iPhone%20X/Designing%20and%20Developing%20for%20iPhone%20X-jLEBAmtSSLZ7uBcYwpbskg2vkO0cVK7i.png)

## Face ID

Finally, three quick notes about Face ID\. First, if you are linking on iOS 11 or later, you will have to add the NSFaceIDUsageDescription usage description key with a description to your Info\.plist if you want to use Face ID\. This is used in place of Touch ID’s localizedReason string\. 

The text that goes with it says, “Do you want to allow ‘App Name’ to use Face ID?”\. This alert isn’t so much for privacy’s sake, like access to your photo library, as it is for user preference, like the notifications permission dialog\. Second, you’ll want to test when the user declines or enables this through the Settings app under the Face ID & Passcode section\. 

Keep in mind that you’ll have to test this on an actual device as this setting isn’t available on the simulator\. Third, Face ID behaves a little differently than Touch ID\. For whatever reason when Face ID scans a user’s face your app briefly enters the background and applicationDidEnterBackground is called\. When this finishes, your app becomes active and applicationDidBecomeActive is called\. This can lead to an authentication dialog loop as seen in the Barclay app if an app brings up a biometric authentication dialog during applicationDidBecomeActive\.

## In Conclusion

These are just a handful of tips for developing and designing for iPhone X\. For more information check out Apple’s updated [Human Interface Guidelines section for iPhone X](https://developer.apple.com/ios/human-interface-guidelines/overview/iphone-x/) and [Designing for iPhone X video](https://developer.apple.com/videos/play/fall2017/801/).


