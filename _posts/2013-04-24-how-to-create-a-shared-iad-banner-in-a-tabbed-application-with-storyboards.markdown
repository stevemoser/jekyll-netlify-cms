---
layout: post
title: "How to Create a Shared iAd banner in a Tabbed Application with Storyboards"
date: 2013-04-24 20:45
tags: [iAd,UITabBarController,Storyboards,UIViewControllerContainment]
---

While looking at Apple's iAd Suite sample code I noticed that the TabbedBanner example was the perfect use case for iOS 6's new *Container View* Storyboard object. It allows a view controller to be placed inside another view controller which is exactly how we want our banner view controllers setup. No code. Actually it gets better, to get Apple's example to work with container views we mostly remove code! Check out my example [github project][1] or follow below to see how.

To get started it's easiest to start a new project based on Xcode's *Tabbed Application* template while making sure that the *Use Storyboards* option is checked. Then download [Apple's iAd Suite sample code][2] and copy the BannerViewController .h and .m files into your project (for extra fun you copy the TextViewController classes as well to show off the timer reacting to ads appearing).

Next open up the main storyboard and remove the relationship segue to the first and second view controller and move them to the right to make room for the container view controllers. Then drag out two view controllers and two container view and place each container view inside their respective view controllers. Open the Identity Inspector and select each new view controller and change the class to BannerViewController. The Banner View Controllers now need to be connected to the tab bar so ctrl drag from the Tab Bar Controller scene to each Banner View Controller and select a *relationship* segue.

Now we need to make sure that the container view is the root view of each Banner View Controller. Unless someone knows an easier way I've found it best to drag the container view above the default view in Interface Builder's Document Outline. Finally we can connect each Banner View Controller's container view to their respective first and second view controllers from the original template by ctrl dragging from each container view to each first or second view controller and selecting the *embed* segue. We are all done with storyboard though you can add a long text view to each first and second view controller so that you can test the banner view controller to make sure it doesn't occlude it's child view controller.

Open up BannerViewController.h and remove the old init method. We don't need this anymore since we are creating the banner view controllers in the storyboard.

	- (instancetype)initWithContentViewController:(UIViewController *)contentController;

Move on to BannerViewController.m and remove the implementation of the previous method. We are almost done but we still how to set the `_contentController` instance variable and add the banner view controller to the BannerViewManager's shared instance since we are not overriding the init method anymore. Instead of implementing a custom init method we have to wait for the storyboard to setup the view controller and then let us finish the initialization by overriding `initWithCoder:` and adding the banner view controller to the BannerViewManager's shared instance.

	- (id)initWithCoder:(NSCoder *)aDecoder
	{
	    self = [super initWithCoder:aDecoder];
	    if (self) {
	        [[BannerViewManager sharedInstance] addBannerViewController:self];
	    }
	    return self;
	}

But what about the `_contentController` you may ask. At this point we haven't loaded BannerViewController's view and it's subviews thus the container view isn't loaded. Normally if we were creating this view controller programmatically we could create the view like in `loadView` but since it is created in the storyboard we can remove this method as well.  After BannerViewController's view is loaded it is still missing the contents of it's container view so it invokes `prepareForSegue:` to retrieve the child view controllers from the storyboard. It is at this point we can set our `_contentController` to the segue's destination view controller.

	- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
	{
	    _contentController = segue.destinationViewController;
	}

And we're done! Build and run to see that the banner view controller doesn't occlude it's child view controller when the ad pops up and the same ad is show across the view controllers. Now you can easily incorporate iAds into your Tabbed Application that uses Storyboards.

[1]: https://github.com/stevemoser/TabbedBanner 
[2]: http://developer.apple.com/library/ios/#samplecode/iAdSuite/Introduction/Intro.html