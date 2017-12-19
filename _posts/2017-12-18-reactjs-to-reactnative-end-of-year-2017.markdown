---
layout: post
title: "From ReactJS to React-Native: End of Year 2017."
date: 2017-12-18 11:39:06 -0400
---

# From ReactJS to React-Native: End of Year 2017.
![React](/images/reactjs-to-reactnative/react.png)

As a [ReactJS](https://reactjs.org/) developer that has dabbled in [ReactVR](https://facebook.github.io/react-vr/), I was excited build my first [React-Native](https://facebook.github.io/react-native/) app.  I've been developing a cross platform React-Native app that supports a smart ball toy (a ball that Counts it's bounces) and allows the players to compete against each other and track their scores.  

Having never worked with `React-Native` before, I smacked into wall after wall until I finally got a solid footing on how `React-Native` was different from `ReactJS`.  Beyond that, a lot of the design patterns are just plain `React`. 

Below I've curated a list of things I had to figure out this year in order to jump from `ReactJS` to `React-Native` as a developer.  To those at the beginning, I hope this can help you along your path to mobile development.

# Apps don't have a browser window or DOM

[![reactnative-vs-reactjs](/images/reactjs-to-reactnative/react-dom.png)](https://medium.com/@jiyinyiyong/virtual-dom-is-the-new-ir-67839bcb5c71)

This may seem obvious, but if you hadn't thought about it before it greatly changes which npms components you'll be able use in your app.  In ReactNative there is no DOM.  Therefore, any component designed in such a way that requires a DOM will not work at all in React-Native.  

This means that when you're searching for a doohicky that does something, make sure you are googling `react-native-DOOHICKY` and not just `react-DOOHICKY`. 

Save yourself some pain.

# Apps don't have exposed routes

This is another item that may seem obvious. Even if you're retrieving data from a Rails or Node.js backend, you're not building something with publicly accessible web routes.  There is no DOM, no Browser, and thus there is no address bar to use to navigate around. In a lot of ways this makes things easier, you can use a variety of different packages to handle routing, but I chose to use [`react-native-router-flux`](https://github.com/aksonov/react-native-router-flux)

In my project I created a file called `AppRouter.js` that I render after my Redux store in `Root.js`. Inside my app router I simply import like so:

```js
import { Scene, Router } from "react-native-router-flux";
```

Here's an example of how I used react-native-router-flux:

```js
const AppRouter = () =>
    (<Router sceneStyle={{ backgroundColor: transparent }}>
        <Scene key="root">
            <Scene
                key="splash"
                component={Splash}
                animation="fade"
                duration="2000"
                hideNavBar
                initial
            />
            <Scene
                component={WithHeadingHOC(Home)}
                headerText="plug in"
                hideNavBar={1}
                key="home"
                previous={() => Actions.splash(reset)}
            />
            <Scene
                component={WithHeadingHOC(ResetPassword)}
                headerText="reset pw"
                hideNavBar={1}
                key="resetPassword"
                previous={() => Actions.login(reset)}
            />
            <Scene
                key="main"
                component={WithoutHeadingHOC(Main)}
                isMain
                showBallDisconnectedModal
                hideNavBar={1}
            />
            <Scene
                component={WithoutHeadingHOC(Badges)}
                headerText="badges"
                hideNavBar={1}
                isMain={false}
                key="badges"
                previous="main"
            />
        </Scene>
    </Router>);
```

You can see that I wrap a Router component around a series of scene components.  The key inside the Scene is what I'll use later to trigger a route. Here's an example:

```js
Actions.main({ ballStatus: "count", previous: "sign up" });
```

You can see that above I have a scene with the key of "main." All I have to do to trigger the component listed in that scene is call Actions.main. I wanted to cut down on the number of routes I was using and refactored the basic page template to change based on values I'd pass in when I called the route.  In this way you can pass data like props when calling a route.

# Higher order components are awesome

You'll notice that in the router example above I'm wrapping the components in another component. This is called a higher order component.  This allows me to reuse a great deal of code and as you can see above it's really easy to use. By extending `WrappedComponent`, the higher order component has access to the incoming component's props.

```js
export default function WithHeadingHOC(WrappedComponent) {
    return class ScreenWithHeading extends WrappedComponent {
        render() {
            return (
                <ImageBackground
                    key="imageBackground"
                    source={crossPlatformDevice(
                        require("../../img/MasterBG.png"),
                        require("../../img/MasterBGTabletMiniAir.png"),
                        require("../../img/MasterBG.png"),
                        require("../../img/MasterBGTabletMiniAir.png")
                    )}
                    resizeMode="cover"
                    style={styles.container}
                >
                    <BallHeadingNav previous={this.props.previous} />
                    <YellowBallHeading
                        key="YellowBallHeading"
                        headerText={this.props.headerText}
                        previous={this.props.previous}
                    />
                    <WrappedComponent key="wrappedComponent" {...this.props} />
                    <GeneralErrorModal />
                </ImageBackground>);
            }
        };
}
```

# You will need to read/write Java and Objective-C sometimes.

![prototype](/images/reactjs-to-reactnative/prototype.png)

In this project my client wanted to use the headphone jack like a modem to send data from the ball to the phone.  ReactNative can do a lot, but at the moment of writing this it can't access the headphone jack. 

I started out with these prototypes to simulate the smart ball.

![smart-ball](/images/reactjs-to-reactnative/ball.png)

Except for this one element, ReactNative was a great choice for building this project.  The client wants 2 apps and for the most part I only need to build 1.

In ReactJS `create-react-app` is becoming quite the standard with Dan Abramoff voicing strong support for the way it's simplifying the intensely layered process of stacking a ReactApp. There is a `create-react-native-app` which is also great, unfortuantely though, if you need to do ANYTHING with the native code you need to eject.

When you eject instead of all the `Android`, `IOS`, `Android Studio` and `XCode` project files being hidden, they become exposed and can be edited in the same way an `IOS` or `Android` developer would work on the app. There are a few differences however. 

`React-Native` should be handling the `views`. IOS loads `appdelegate.m`, written in `objective-c` and then launches `react-native's views after loading itself.  

You can load an IOS view as a separate page, but that's a special case scenario I didn't need to do.

# You might need to create a Bridge between IOS or Android and React-Native

If you need to do something with native like receiving headphone data to be decoded into a count, you're going to need to send that data to `React-Native`.  

If the data needs to continually update you'll need to setup an event. If you only need the data on load you can pass constant values through as well. 

Here's an example of some of the `objective-c` used to build a custom `bridge` from native `IOS` into `React-Native`:

```objective-c
#import "ReactNativeEventEmitter.h"

@implementation ReactNativeEventEmitter

RCT_EXPORT_MODULE();

- (NSArray<NSString *> *)supportedEvents {
    return @[
        @"BounceCountEmitter", @"AudioInputChangedEmitter",
        @"isTryMeModeEmitter"
    ];
}

- (void)startObserving {
    [[NSNotificationCenter defaultCenter]
        addObserver:self
           selector:@selector(emitBounceCountEvent:)
               name:@"bounceCountEventNotification"
             object:nil];

    [[NSNotificationCenter defaultCenter]
        addObserver:self
           selector:@selector(emitIsTryMeModeEvent:)
               name:@"isTryMeModeEventNotification"
             object:nil];

    [[NSNotificationCenter defaultCenter]
        addObserver:self
           selector:@selector(emitAudioRouteChangeEvent:)
               name:AVAudioSessionRouteChangeNotification
             object:nil];
}

- (void)stopObserving {
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}

- (void)emitBounceCountEvent:(NSNotification *)notification {
    NSString *count = notification.userInfo[@"data"];
    [self sendEventWithName:@"BounceCountEmitter" body:count];
}

- (void)emitIsTryMeModeEvent:(NSNotification *)notification {
    NSString *isTryMeMode = notification.userInfo[@"data"];
    [self sendEventWithName:@"isTryMeModeEmitter" body:isTryMeMode];
}

- (void)emitAudioRouteChangeEvent:(NSNotification *)notification {
    [self sendEventWithName:@"AudioInputChangedEmitter"
                       body:[ReactNativeEventEmitter isHeadsetPluggedIn]
                                ? @"true"
                                : @"false"];
}

+ (BOOL)isHeadsetPluggedIn {
    UInt32 routeSize = sizeof(CFStringRef);
    CFStringRef route = nil;

    AudioSessionInitialize(NULL, NULL, NULL, NULL);
    OSStatus error = AudioSessionGetProperty(kAudioSessionProperty_AudioRoute,
                                             &routeSize, &route);

    if (error)
        printf("ERROR GETTING INPUT AVAILABILITY! %ld\n", error);
    if ((route == NULL) || (CFStringGetLength(route) == 0)) {
        NSLog(@"AudioRoute: SILENT");
    } else {
        NSString *routeStr = (__bridge NSString *)route;
        NSLog(@"routeStr=%@", routeStr);
        if ([routeStr rangeOfString:@"Head"].location != NSNotFound ||
            [routeStr rangeOfString:@"MicrophoneWired"].location !=
                NSNotFound) {
            return YES;
        }
    }
    return NO;
}

@end
```

This was a huge pain in the ass to say the least so take this into consideration when you're planning your project.  This bridging needs to be done in both IOS and Android (Java), so this obviously reduces the overall time saving benefit of a shared codebase.

Using `Objective-C`, `C`, `Swyft`, or `Java` to create your own `bridge` is pretty complex if you're a typical React developer without a background in mobile.  Avoid ejecting for complex stuff like that if at all possible. I didn't have a choice. If you want to install anything that requires interfacing with the native layer you're going to have to interface in those native languages.

A great example is [react-native-firebase](https://github.com/invertase/react-native-firebase) which requires a lot of native setup, but allows you to multi-thread Firebase methods vs. running Firebase on just the Javascript thread. 

Like [react-native-device-info](https://www.npmjs.com/package/react-native-device-info) which can be used to get data about the user's device or [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons) which provides customizable Icons for React Native with support for NavBar/TabBar/ToolbarAndroid, image source and full styling

# Styling IOS vs. Android

![IOS vs Android](/images/reactjs-to-reactnative/ios_vs_android.jpg)

To start you don't really use separate stylesheet files, the idea is that you're going to be breaking up all your code into tiny pieces, where everything about that piece is located in the same file, the component. 

If you've factored your code properly this structure really simplifies styling as it's no longer a wild goose chase to find the specific property you need to adjust or where to adjust it.  

```js
import { StyleSheet } from "react-native";
```

You can pass a `StyleSheet` or plain object filled with `camelCased CSS styles` into the `style={styles.background}` prop on most components and should expect to use inline styles from now on.

`StyleSheet.create({})` loads your styles into memory at the moment your app launches.  StyleSheet objects usually don't work if you need to set `dynamic values` in the style prop like for an animation. In this case you can use a plain `Javascript object` to build your `JSS`, but it's preferable to use `StyleSheet` from `react-native`.

```js
const styles = StyleSheet.create({
    background: {
        bottom: crossResponsiveHeight(null, null, 3, 3),
        height: responsiveHeight(100),
        position: "absolute",
        resizeMode: "cover",
        top: crossResponsiveHeight(-3, -3, null, null),
        width: responsiveWidth(100),
        zIndex: -1
    }
});
```

I really like `JSS` because as you see above I'm able to utilize `Javascript` methods right in the `StyleSheet`. 

Incredibly powerful!

For some reason `IOS` and `Android` don't always respond the same way to the `JSS`.  In `IOS` there is a preference for setting position using `top` and using negative numbers when necessary to position elements on the screen.  `Android` for some reason doesn't like negative numbers, so you'll need to use `bottom` instead.

But wait, aren't we deviating from the single codebase rule if we need a separate stylesheet for Android? Not exactly.  Back to those `ternaries`... 

If you import `Platform`:

```js 
import { Platform } from "react-native"
``` 
`React-Native` will give you the `platform` of the device it's running on.  Therefore you can make statements like this in your `JSS`:

```css
formInput: {
    zIndex: 1,
    fontFamily: Platform.OS === "ios" ? "Myriad Pro" : "myriadpro_regular",
    fontSize: responsiveFontSize(2),
    backgroundColor: "#17b2ee",
    color: "white",
    textAlign: "center",
    borderRadius: 20,
    marginTop: Platform.OS === "ios" ? responsiveHeight(3) : Math.ceil(responsiveHeight(1.3)),
    marginLeft: responsiveWidth(12),
    marginRight: responsiveWidth(12),
    paddingTop: Platform.OS === "ios" ? responsiveHeight(2) : Math.ceil(responsiveHeight(1)),
    paddingBottom: Platform.OS === "ios" ? responsiveHeight(2) : Math.ceil(responsiveHeight(1))
}
```

# React-Native-Cross-Platform-Responsive-Dimensions
I eventually created and open sourced my own component: [react-native-cross-platform-responsive-dimensions](https://github.com/drumnation/react-native-cross-platform-responsive-dimensions) that provides an `API` of methods to create values based on `device dimensions`, as well as methods that allow you to target specific `operating systems`, types of `devices`, and even specific `devices` all within your JSS stylesheet.  

# Support for Hot-Reloading on Multiple Devices

![Cross-Platform Device Styling](/images/reactjs-to-reactnative/MaM.jpg)

The ability to hot reload multiple devices at the same time became available around the time I began writing and using my component for styling. It occurred to me that with such fine control over every operating system and device type, I could save time by previewing on as many devices as I could plug into my computer, styling everything simultaneously.

# Custom Fonts

Installing custom fonts were difficult at first. I haven't researched how to do this without ejecting out of [create-react-native-app](https://github.com/react-community/create-react-native-app), ejecting is the same as initializing a new react-native project with `react-native init`.  This exposes all the native code wheras it is hidden within a create-react-native-app project.

There are a few things to know. First off, IOS and Android look for the same font but by expect them to be named different things.

This means that if you call your custom font in your stylesheet, unless you do the following, the custom font will only ever work on one platform or the other.

I handle this with the `crossPlatformOS(ios, android)` method that's part of my [react-native-cross-platform-dimensions]((https://github.com/drumnation/react-native-cross-platform-responsive-dimensions)) component.

```css
modalBadgeNameText: {
    color: white,
    fontWeight: "400",
    fontFamily: crossPlatformOS("Myriad Pro", "myriadpro_regular"),
    fontSize: responsiveFontSize(2),
    marginTop: responsiveHeight(2)
}
```

You'll also need to copy the fonts to a folder in your project, as well as make sure they are linked to the the react-native project.  Make sure you restart your server and recompile whenever you add a custom font.

Here's a guide on setting up fonts => [React Native Custom Fonts](https://medium.com/react-native-training/react-native-custom-fonts-ccc9aacf9e5e)

# ReactNative Elements

![react-native-elements](/images/reactjs-to-reactnative/elements.png)

Making a touchable button is a little bit more complex on mobile than it is on the web.  On the web you make a button and an onClick event handler and that's it, but on mobile you have to specify things related to touch and there are some additional options to choose.  

Libraries like [React-Native Elements](https://github.com/react-native-training/react-native-elements) strive to make this much easier by constructing useful design elements into easy to use components.  That said, when these things fail me for some reason I always fall back to the long way and use the ReactNative commands instead.

Nader Dabbit, the developer behind React Native Elements, has a great podcast called [React-Native Radio](https://devchat.tv/react-native-radio). 

[![Radio](/images/reactjs-to-reactnative/radio.png)](https://devchat.tv/react-native-radio)

I've been listening for a while now. He brings a really great range of guests from the React-Native world to talk about what they're doing. 

# When To Use Redux

![Dan Abramov](/images/reactjs-to-reactnative/dan-abramov.png)

In my personal opinion any application more complex than a brochure should use [Redux](https://redux.js.org/). Hear me out... React's power is in it's ability to reuse components and simplify things with containers.  The more you refactor your code into smaller components, the more children you'll need to pass props through to get data where it's needed.  I've tried several times to avoid Redux as long as possible and found adding it to be inevitable. 

From a DRY perspective having to pass maybe 3, 4, 5 props down 3 or 4 levels for the data to reach where it's needed, adds an enormous amount of bloat to every child down the line.  With Redux you can refactor your components to your heart's content, keep everything you need to access from random components in the store and only use local state for controlled components and the like.

# Testing Your App

To test your apps you need to install `Xcode` and `Android Studio`. You will need to install emulated OSes within each respectively that will be booted in a simulator to test your apps.  It's also useful to use a real `iPhone` or `Android` phone by registering them within `XCode` and `Android Studio` for use.  Plug them into your computer and enable developer mode.

```bash
react-native run-ios 
```

Running the above command will trigger Xcode to compile, start bundling the JS, and then start the packager and testing server, which will allow you to hot-reload while making changes. Horray!

```bash
react-native run-android
```

This command will do the same, and will launch whatever simulator you've configured within Android studio.

# Compiling your App for Clients

This was a huge pain.  In order to compile your IOS app as a standalone app to send to clients or to publish to the AppStore you'll need a valid developer license with Apple. After "Archiving" which creates an IPA file, send it to some kind of app distribution or testing service. 

These services function much like alternative app stores, making it easy for your client to download the app, and easy for you to update the app for your client.

+ Expo
    + React Native with Expo is a joyride -> [Link](https://medium.com/@DanielKrejci/react-native-with-expo-is-a-joyride-671d706b7a19)

+ Microsoft App Center
    + Up & Running with React Native + Visual Studio Mobile Center -> [Link](https://medium.com/react-native-training/up-running-with-react-native-visual-studio-mobile-center-e3c95adbf650)

+ [HockeyApp](https://hockeyapp.net/)

+ [TestFairy](https://testfairy.com/)

+ [TestFlight](https://developer.apple.com/testflight/)

At Gramercy Tech the developers use a combination of all of these. I'm rooting for Microsoft App Center, but I've yet to get my project to successfully build on their remote server. At the moment I am using TestFairy.

In order to save time you should be able to distribute your app with one command.  As you'll soon see each build could take you upwards of 15-20 minutes as there are multiple steps that can take a while to do manually. 

For example, if I build both Android and IOS apps and upload them to TestFairy it takes about 30-45 minutes. It will involve a lot of sitting around because you have to be there to initiate each additional step. You won't be able to go to lunch while the computer is crunching it's numbers.

The holy grail of local compiling right now can be achieved with [Fastlane](https://fastlane.tools/). Fastlane is the tool to release your iOS and Android app 🚀

It handles all tedious tasks, like generating screenshots, dealing with code signing, and releasing your application.

# End to End Testing and Mobile App Automation with Detox

From the creators of [Detox](https://github.com/wix/detox):

>High velocity native mobile development requires us to adopt continuous integration workflows, which means our reliance on manual QA has to drop significantly. Detox tests your mobile app while it's running in a real device/simulator, interacting with it just like a real user.

>The most difficult part of automated testing on mobile is the tip of the testing pyramid - E2E. The core problem with E2E tests is flakiness - tests are usually not deterministic. We believe the only way to tackle flakiness head on is by moving from black box testing to gray box testing. That's where Detox comes into play.

I created an end to end test that would test my crud actions and auth flow. It starts by signing up and creating a new user, it then logs out, logs back in as that user, then navigates to the edit user page to completely change all of it's properties to something new.

I found that good examples were really hard to come by so here's the full test file:

## Example E2E Test with Detox
```js
var faker = require("faker");
var { allCountryNames } = require("../src/components/auth/CountryCodesAndStates");
var randomNumber = Math.floor(Math.random() * 248)
var newRandomNumber = Math.floor(Math.random() * 248)
var staticEmail = "Darius_Breitenberg48@yahoo.com";

// Sign up and Login Info
var punctuationless = faker.internet.userName().replace(/[.,\/#!$%\^&\*;:{}=\-_`~()]/g,"");
var username = punctuationless.replace(/\s{2,}/g," ");
var country = allCountryNames[randomNumber];
var state = faker.address.state();
var email = faker.internet.email();
var password = "111111111"
// Edit User Info
var newPunctuationless = faker.internet.userName().replace(/[.,\/#!$%\^&\*;:{}=\-_`~()]/g,"");
var newUsername = newPunctuationless.replace(/\s{2,}/g," ");
var newEmail = faker.internet.email();
var newCountry = allCountryNames[newRandomNumber];
var newState = faker.address.state();
var newPassword = "222222222";

sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

describe("<< FORM TESTS >>", () => {
    describe("SECTION 1 - User should be able to Signup.", async () => {
        describe("USERNAME TESTS", async () => {
            it("STEP 1: Signup Button Exists", async () => {
                const signup = await element(by.id("signupButtonHome"));
                await expect(signup).toExist();
                await signup.tap();
            }),
            it("STEP 2: Signup Button Tapped", async () => {
                const usernameAccordianVisible = await element(
                by.id("usernameAccordian")
                );
                await usernameAccordianVisible.tap();
            }),
            it("STEP 3: Username Accordian Visible Tapped", async () => {
                const usernameSignupInput = await element(
                by.id("usernameSignupInput")
                );
                await usernameSignupInput.tap();
            }),
            it("STEP 4: Type Username", async () => {
                const usernameSignupInput = await element(
                by.id("usernameSignupInput")
                );
                await usernameSignupInput.typeText(username);
            }),
            it("STEP 5: Type Username", async () => {
                const usernameAccordianHide = await element(
                by.id("usernameAccordianHide")
                );
                await usernameAccordianHide.tap();
            }),
            it("STEP 6: Hide Username Accordian", async () => {
                const usernameAccordianHide = await element(
                by.id("usernameAccordianHide")
                );
                await usernameAccordianHide.tap();
            });
        }),
        describe("EMAIL TESTS", async () => {
            it("STEP 7: Email Accordian Visible Tapped", async () => {
                const emailAccordianVisible = await element(
                by.id("emailAccordian")
                );
                await emailAccordianVisible.tap();
            }),
            it("STEP 8: Email Accordian Hidden Tapped", async () => {
                const emailSignupInput = await element(by.id("emailSignupInput"));
                await emailSignupInput.tap();
            }),
            it("STEP 9: Email Typed", async () => {
                const emailSignupInput = await element(by.id("emailSignupInput"));
                await emailSignupInput.typeText(email);
            }),
            it("STEP 10: Email Accordian Hidden", async () => {
                const emailAccordianHide = await element(
                by.id("emailAccordianHide")
                );
                await emailAccordianHide.tap();
                await emailAccordianHide.tap();
            });
        }),
        describe("COUNTRY TESTS", async () => {
            it("STEP 11: Country Accordian Visible Tapped", async () => {
                const countryAccordianVisible = await element(
                by.id("countryAccordian")
                );
                await countryAccordianVisible.tap();
            }),
            it("STEP 12: Country Picker Tapped", async () => {
                const countryPicker = await element(by.id("countryPicker"));
                await countryPicker.tap();
            }),
            it("STEP 13: CountryFilter Tapped", async () => {
                const countryFilter = await element(by.text("Filter"));
                await countryFilter.tap();
            }),
            it("STEP 14: Type Country Name Into Filter", async () => {
                const countryFilter = await element(by.text("Filter"));
                await countryFilter.typeText(country);
            }),
            it("STEP 15: Pick Country", async () => {
                const pickCountry = await element(by.text(country)).atIndex(1);
                await pickCountry.tap();
            }),
            it("STEP 16: Hide Keyboard", async () => {
                const pickCountry = await element(by.text(country)).atIndex(1);
                await pickCountry.tap();
            }),
            it("STEP 17: Country Accordian Hide Tapped", async () => {
                const countryAccordianHide = await element(
                by.id("countryAccordianHide")
                );
                await countryAccordianHide.tap();
            });
        }),
        describe("STATE TESTS", async () => {
            it("STEP 18: State if United States", async () => {
                if (country === "United States") {
                it("STEP 18.5: StatePicker Tapped", async () => {
                    const statePickerVisible = await element(by.id("statePicker"));
                    await statePickerVisible.tap();
                }),
                it("STEP 18.8: Select US State", async () => {
                    const state = await faker.address.state();
                    console.log("US STATE: ", state);
                    const pickState = await element(by.text(state));
                    await pickState.tap();
                });
                }
            });
        }),
        describe("PASSWORD TESTS", async () => {
            it("STEP 19: Password Accordian Visible Tapped", async () => {
                const passwordAccordianVisible = await element(
                by.id("passwordAccordian")
                );
                await passwordAccordianVisible.tap();
            }),
            it("STEP 19: Password Accordian Visible Tapped", async () => {
                const passwordSignupInput = await element(
                by.id("passwordSignupInput")
                );
                await passwordSignupInput.tap();
            }),
            it("STEP 20: Password Typed", async () => {
                const passwordSignupInput = await element(
                by.id("passwordSignupInput")
                );
                await passwordSignupInput.typeText(password);
            });
            it("STEP 21: Hide Password Accordian", async () => {
                const passwordAccordianHide = await element(
                by.id("passwordAccordianHide")
                );
                await passwordAccordianHide.tap();
                await passwordAccordianHide.tap();
            })
        }),
        describe("PASSWORD CONFIRMATION TESTS", async () => {
            it("STEP 22: PasswordConf Input Textfield Tapped", async () => {
                const passwordConfSignupInput = await element(
                by.id("passwordConfSignupInput")
                );
                await passwordConfSignupInput.tap();
            }),
            it("STEP 23: PasswordConf Typed", async () => {
                const passwordConfSignupInput = await element(
                by.id("passwordConfSignupInput")
                );
                await passwordConfSignupInput.typeText(password);
            });
        }),
        describe("FINAL VALIDATION TESTS", async () => {
            it("STEP 24: Signup Button Tapped", async () => {
                const signupButton = await element(by.id("signupButton"));
                await signupButton.tap();
                await signupButton.tap();
            }),
            it("STEP 25: Main Menu Container Exists", async () => {
                const mainMenuContainer = await element(
                    by.id("mainMenuContainer")
                );
                await expect(mainMenuContainer).toExist();
            }),
            it("STEP 26: Tap Ball Plugged In Modal", async () => {
                const modal = await element(by.id("modal"));
                await expect(modal).toExist();
                await modal.tap();
            }),
            it("Step 27: Log Out", async () => {
                const logOutButton = await element(by.id("logOutButton"));
                await expect(logOutButton).toExist();
                await logOutButton.tap();
            })
        }),
    describe("Section 2 - User should be able to Login.", async () => {
        it("STEP 1: Login Button Exists", async () => {
            const login = await element(by.id("loginButtonHome"));
            await expect(login).toExist();
        }),
        it("STEP 2: Login Button Tapped", async () => {
            const login = await element(by.id("loginButtonHome"));
            await login.tap();
        }),
        it("STEP 3: Email Button Tapped", async () => {
            const usernameField = await element(by.id("usernameField"));
            await usernameField.tap();
        }),
        it("STEP 4: Typed Email", async () => {
            const usernameField = await element(by.id("usernameField"));
            await usernameField.typeText(username);
        }),
        it("STEP 5: Tapped Password", async () => {
            const passwordField = await element(by.id("passwordField"));
            await passwordField.tap();
        }),
        it("STEP 6: Password Typed", async () => {
            const passwordField = await element(by.id("passwordField"));
            await passwordField.typeText(password);
        }),
        it("STEP 7: Login Button Tapped", async () => {
            const loginButton = await element(by.id("loginButton"));
            await loginButton.tap();
        }),
        it("STEP 8: Main Menu Container Exists.", async () => {
            const mainMenuContainer = await element(by.id("mainMenuContainer"));
            await expect(mainMenuContainer).toExist();
        })
    }),
    describe("Section 3 - User should be able to Edit Their Profile.", async () => {
        describe("NAVIGATE TO EDITUSER SCREEN", async () => {
            it("STEP 1: Tap Ball Plugged In Modal", async () => {
                const modal = await element(by.id("modal"));
                await expect(modal).toExist();
                await modal.tap();
            }),
            it("STEP 1: Tap Settings", async () => {
                const settings = await element(by.id("settings"));
                await settings.tap();
            }),
            it("STEP 2: Tap User", async () => {
                const user = await element(by.id("user"));
                await expect(user).toExist();
                await user.tap();
            }),
            it("STEP 3: Tap Edit User", async () => {
                const editUser = await element(by.id("editUser"));
                await expect(editUser).toExist();
                await editUser.tap();
            })
        }),
        describe("USERNAME INFO TESTS", async () => {
            it("STEP 4: Tap + Edit User", async () => {
                const editUsername = await element(by.id("editUsername"));
                await expect(editUsername).toExist();
                await editUsername.tap();
            }),
            it("STEP 5: Change Username", async () => {
                const editUsername = await element(by.id("editUsername"));
                await editUsername.typeText(newUsername);
            }),
            it("STEP 6: Tap + Edit Email", async () => {
                const editEmail = await element(by.id("editEmail"));
                await editEmail.typeText(newEmail);
            })
        }),
        describe("COUNTRY TESTS", async () => {
            it("STEP 7: Country Picker Tapped", async () => {
                const countryPicker = await element(by.id("countryPicker"));
                await countryPicker.tap();
            }),
            it("STEP 8: CountryFilter Tapped", async () => {
                const countryFilter = await element(by.text("Filter"));
                await countryFilter.tap();
            }),
            it("STEP 9: Type Country Name Into Filter", async () => {
                const countryFilter = await element(by.text("Filter"));
                await countryFilter.typeText(newCountry);
            }),
            it("STEP 10: Pick Country", async () => {
                const pickCountry = await element(by.text(newCountry)).atIndex(1);
                await pickCountry.tap();
            }),
            it("STEP 11: Hide Keyboard", async () => {
                const pickCountry = await element(by.text(newCountry)).atIndex(1);
                await pickCountry.tap();
            })
        }),
        describe("STATE TESTS", async () => {
        it("STEP 12: State if United States", async () => {
            if (newCountry === "United States") {
                it("STEP 12.5: StatePicker Tapped", async () => {
                    const statePickerVisible = await element(by.id("statePicker"));
                    await statePickerVisible.tap();
                }),
                it("STEP 12.8: Select US State", async () => {
                    const pickState = await element(by.text(newState));
                    await pickState.tap();
                });
            }
        });
        }),
        describe("PASSWORD TESTS", async () => {
            it("STEP 13: Password Accordian Visible Tapped", async () => {
                const passwordSignupInput = await element(
                by.id("passwordSignupInput")
                );
                await passwordSignupInput.tap();
            }),
            it("STEP 14: Password Typed", async () => {
                const passwordSignupInput = await element(
                by.id("passwordSignupInput")
                );
                await passwordSignupInput.typeText(newPassword);
            });
        }),
        describe("PASSWORD CONFIRMATION TESTS", async () => {
            it("STEP 15: PasswordConf Input Textfield Tapped", async () => {
                const passwordConfSignupInput = await element(
                by.id("passwordConfSignupInput")
                );
                await passwordConfSignupInput.tap();
            }),
            it("STEP 16: PasswordConf Typed", async () => {
                const passwordConfSignupInput = await element(
                by.id("passwordConfSignupInput")
                );
                await passwordConfSignupInput.typeText(newPassword);
            });
        }),
        describe("FINAL VALIDATION TESTS", async () => {
            it("STEP 17: Confirm Changes Button Tapped", async () => {
                const confirmChanges = await element(by.id("confirmChanges"));
                await confirmChanges.tap();
            }),
            it("STEP 25: Redirected to User Screen", async () => {
                const editButtonOnUserScreen = await element(
                    by.id("editUser")
                );
                await expect(editButtonOnUserScreen).toExist();
            })
        })
    })
    })
});
```
## Video of the automated test flow

I know that was kind of long, but here's the video of this automation going through it's sequence.

[![Detox Test](/images/reactjs-to-reactnative/detox.png)](https://youtu.be/lJHzOcIT5os)

# Bash Commands I use all the time

Cleans node_modules folder, reset caches, and re-installs node_modules folder using NPM:

```bash
rm -rf node_modules && npm install && watchman watch-del-all && rm -fr $TMPDIR/react-*
```

Cleans node_modules folder, reset caches, and re-installs node_modules folder using YARN:
```bash
watchman watch-del-all && rm -rf node_modules/ && yarn cache clean && yarn install && yarn start -- --reset-cache
```

Resets the React packager cache:
```bash
npm start -- --reset-cache
```

alternatively,

```bash
node_modules/react-native/scripts/packager.sh --reset-cache
```

## Android Bash Commands

Create installable build:
```bash
react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res/
```

Generate new Android Keystore:
```bash
keytool -genkey -v -keystore my-app-key.keystore -alias my-app-alias -keyalg RSA -keysize 2048 -validity 10000
```

Compile release build and run Android simulator:
```bash
react-native run-android --variant=release
```

Copy compiled apk directly to device that is connected to the computer:
```bash
adb install -r ./app/build/outputs/apk/app-release-unsigned.apk
```

Clean Android build folder (from android directory):
```bash
./gradlew clean
```

Clean and build Android Release (from android directory):
```bash
echo "DID YOU INCREMENT THE BUILD NUMBER?" && ./gradlew clean && ./gradlew assembleRelease
```

## IOS Bash Commands

Record a video of the IOS simulator for clients:
```bash
xcrun simctl io booted recordVideo appvideo.mov
```

List IOS devices:
```bash
xcrun simctl list
```

Run IOS Device + Release configuration:xcrun simctl listxcrun simctl list
```bash
react-native run-ios --device 'YOUR-DEVICE-NAME' --configuration Release
```

Reset Everything and Update Pods
(From the CocoaPods directory)
```bash
watchman watch-del-all && 
rm -rf node_modules && 
rm -rf package-lock.json && 
rm -rf ios/build && 
rm -rf $TMPDIR/react-* && 
npm cache clear --force -s && 
npm cache verify && 
npm install && 
cd ios && 
sudo gem install cocoapods-deintegrate && 
sudo gem install cocoapods-clean && 
pod deintegrate && 
pod clean && 
rm -f Podfile.lock && 
pod install && 
pod update &&
cd .. && 
npm start -- --reset-cache
```

Update CocoaPods (from IOS directory)
```bash
pod update
pod update repo
pod install --repo-update
```

# Other must read React-Native guides

Before you start building your React-Native project make sure you setup ESLint + Prettier.  It will make your code super clean and save you a ton of time.

+ Configure ESLint, Prettier, and Flow in VS Code for React Development -> [Link](https://hackernoon.com/configure-eslint-prettier-and-flow-in-vs-code-for-react-development-c9d95db07213)
+ Virtual DOM is the new IR -> [Link](https://medium.com/@jiyinyiyong/virtual-dom-is-the-new-ir-67839bcb5c71)
+ How to make an ARKit app in 5 minutes using React Native -> [Link](https://medium.com/@HippoAR/how-to-make-your-own-arkit-app-in-5-minutes-using-react-native-9d7ce109a4c2)
+ Alphabetize your CSS properties, for crying out loud -> [Link](https://medium.com/@jerrylowm/alphabetize-your-css-properties-for-crying-out-loud-780eb1852153)
+ Typed Redux -> [Link](https://blog.callstack.io/typed-redux-2aa8bff926ff)
+ Inline React Styles Using the JSX Spread Operator -> [Link](http://osxi.github.io/react/2015/10/18/inline-react-styles-using-the-jsx-spread-operator.html)
+ The State of Javascript 2017 -> [Link](https://stateofjs.com/2017/introduction/)

# Conclusion

There are a number of differences between building a web application with `ReactJS` and building a Native app with `React-Native`, but at the end of the day you build pages with `JSX`, `JSS`, `JS`, reuse components, and structure things like a one page application in the same way.  

I'm sure as time progresses and `React-Native` hits 1.0, more of these issues will be ironed out, but as for the others, you just need to know that it's different and act accordingly.

