<h2>Swifty Snacks 105: Ch-ch-ch-ch-changes</h2>

Snack boi’s & Snack gal’s… buckle-up, 105 is here. Shizzle just got real.

All popular social app’s require users to complete a registration process. Registration is required, so as to allocate each user with a unique identifier. This identifier is used to identify data (e.g. messages) associated to the user, stored in the cloud.

Let’s get hypothetical. The cool kid on my block, just put me on to Snackergram, the hottest new way to photograph snacks. On opening, Snackergram presents a ‘splash screen,’ which acts as cover behind which the app cranks into life. After a few seconds, I am presented with a registration/login screen, since the app knows that I am a new user. Registration completes. The main user interface is displayed. The snacks on Snackergram are moorish, so much so that I have to log out to avoid tempation. On logging out, the registration/login screen reappears.

Previously, I have implemented the above workflow, by modally presenting registration/login view controllers over the app’s main navigation stack. This approach is not ideal, as it keeps the main navigation stack’s view controllers in memory and is less than fluid, in terms of UX.

These days, I follow the architecture described by Stan Ostrovskiy in the article linked below: -

https://medium.com/@stasost/ios-root-controller-navigation-3625eedbbff

Stan’s approach is to build separate UINavigation stacks for each of the app’s key processes: registration/login workflow & main app functionality. A ‘quarterback’ (root view controller) is established to present the appropriate siloed stack, dependant on the status of the user at the given time.

Time to build out an illustrative example. Start a single view project, then delete that storyboard business (see Swifty Snacks 101). Rename the ‘boilerplate’ ViewController.swift file to RootController.swift, then in the AppDelegate’s didFinishLaunchingWithOptions set the app window’s rootViewController as RootController.

<img src="Swifty Snacks 105/image1.png">

In RootController set the view’s backgroundColor to red. Build and run to check that everything is in order.

<img src="Swifty Snacks 105/image2.png">

RootController will act as our quarterback, let’s give it some teams to play with. Team SplashController will present a splash screen to the user, when the app ‘boots’ into life. Add an activity indicator to it’s view and instruct it to cycle for 3 seconds, to mimic a call to the cloud.

<img src="Swifty Snacks 105/image3.png">

Next, construct team RegistrationLoginController. Add a left bar button, as a navigationItem, in addition to the associated function ‘login.’

<img src="Swifty Snacks 105/image4.png">

Finally, build our app’s MainController team, adding a ‘log out’ button associated to function ‘logout.’

<img src="Swifty Snacks 105/image5.png">

To enable our ‘quarterback’ to lead, we must empower its teammates to reference his leadership when required. We will define this means of reference as an extension to AppDelegate.

<img src="Swifty Snacks 105/image6.png">

We can now call-out to our ‘quarterback’ (aka rootViewController) from anywhere within the app. Great, now return to RootController to give it the tools to manage its newfound responsibilities.

The intention is to set our ‘quarterback’ as a parent view that will add the appropriate child, as instructed. Use a private variable to set a reference to the current child view controller.

<img src="Swifty Snacks 105/image7.png">

Address xCodes concern by adding a initialiser to the class.

<img src="Swifty Snacks 105/image8.png">

Next, allow xCode to add the required init(coder), press into the warning and select ‘fix.’ Within viewDidLoad() we can now set the current view controller, which will start out being an instance of our SplashController class, as RootController’s child. Set the child’s view.frame to equal that of its parent’s view.bounds. We do this so that the child’s frame act in step with its parent view, when UI changes occur e.g. the in-call status bar is invoked. Add the child as a subview then complete the process by calling didMove(toParent: self), which must be called ‘after the view controller is added or removed from a container view controller.’

<img src="Swifty Snacks 105/image9.png">

We can now build out three new functions, within RootController, to be used by our quarterback to change the current viewcontroller on display: namely, showRegistrationLoginScreen() & showMainScreen().

Consider showRegistrationLoginScreen(). In part 1 we add an instance of RegistrationLoginController() as our quarterback’s child. Notice that RegistrationLoginController() is embeded as the root of a new UINavigationController — do not confuse this root with our app’s rootViewController, aka quarterback. Part 2 deals with installing the quarterback’s child as its current view controller i.e. that which it will display to the user — out with the old, in with the new.

<img src="Swifty Snacks 105/image10.png">

Proceed to build out showMainScreen(), in accordance with the structure of showRegistrationLoginScreen().

<img src="Swifty Snacks 105/image11.png">

So far so good. To wrap up, let’s ‘walk through’ the user journey, adding code to inform the quarterback which view controllers should be displayed according to current session status.

Our freshly installed app will tell its quarterback (RootController) to display an instance of SplashController(). SplashController() will fake a call to an external service. Here, an if / else statement, leveraging NSUserDefaults, will determine what course of action is to be taken, based on session status. If the user is ‘LOGGED_IN’ the app’s quarterback will be instructed to show the MainScreen (aka MainController). Otherwise, the quarterback will be tasked to show the RegistrationLoginController. The initial session status is ‘LOGGED_IN’ = false, so the RegistrationLoginController is displayed.

<img src="Swifty Snacks 105/image12.png">

The RegistrationLoginController will assume that registration has succeeded and set the session status to ‘LOGGED_IN’ = true, before instructing the app’s quarterback to show an instance of MainController().

<img src="Swifty Snacks 105/image13.png">
