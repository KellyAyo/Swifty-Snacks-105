<h2>Swifty Snacks 105: Ch-ch-ch-ch-changes</h2>

Snack boi’s & Snack gal’s… buckle-up, 105 is here. Shizzle just got real.

All popular social app’s require users to complete a registration process. Registration is required, so as to allocate each user with a unique identifier. This identifier is used to identify data (e.g. messages) associated to the user, stored in the cloud.

Let’s get hypothetical. The cool kid on my block, just put me on to Snackergram, the hottest new way to photograph snacks. On opening, Snackergram presents a ‘splash screen,’ which acts as cover behind which the app cranks into life. After a few seconds, I am presented with a registration/login screen, since the app knows that I am a new user. Registration completes. The main user interface is displayed. The snacks on Snackergram are moorish, so much so that I have to log out to avoid tempation. On logging out, the registration/login screen reappears.

Previously, I have implemented the above workflow, by modally presenting registration/login view controllers over the app’s main navigation stack. This approach is not ideal, as it keeps the main navigation stack’s view controllers in memory and is less than fluid, in terms of UX.

These days, I follow the architecture described by Stan Ostrovskiy in the article linked below: -

https://medium.com/@stasost/ios-root-controller-navigation-3625eedbbff

Stan’s approach is to build separate UINavigation stacks for each of the app’s key processes: registration/login workflow & main app functionality. A ‘quarterback’ (root view controller) is established to present the appropriate siloed stack, dependant on the status of the user at the given time.

Time to build out an illustrative example. Start a single view project, then delete that storyboard business (see Swifty Snacks 101). Rename the ‘boilerplate’ ViewController.swift file to RootController.swift, then in the AppDelegate’s didFinishLaunchingWithOptions set the app window’s rootViewController as RootController.

<img src="">
