# Mobile

**NOTE**: There are multiple approaches to mobile development. This is a living document, intended as a guide and as a conversation starter, not as rules to be followed. Some of the examples here are oriented towards iOS. When we add Android, we'll just fork whatever's specific and leave whatever's generic to both platforms.

## Releasing

Probably the most outstanding difference between mobile and web development is the fact that we don't own the devices users will be running our software on, or even the stores users will be downloading that software from. We don't even own which version of our software they will run. This has a few unavoidable consequences:

* Releasing takes time: *Days*.
* Getting the user to update the app takes time: *Weeks*. And they may even not do it, ever.
* Corrupt, bug-triggering data/state is inaccessible to the developer, unless explicitly forwarded or reported.

This means the release **process** should be stable, repeatable and, if possible, automated and able to measure the success of each version.

Each project should define its release process. The following are broad strokes and capabilities that will ensure successful releases and the ability to fix them if they fail.

Pre-release checklist:
* Review the exact scope of a version (stories, tasks, etc) and document it for later tracing.
* Make sure all checks are running (CI, with automated tests, etc)
* Guarantee a certain level of stability for the whole scope of the version and each individual component (stories, tasks, etc)
* Our [CI/CD pipeline](/Documentation/continuous-integration.md) makes sure that every build gets greenlighted first by QA with the release candidate and then by the stakeholders/PM through the AppStore/Playstore beta channel.
* Continuously make the version available to the product owner(s) so they can try it
* Update store metadata (description, screenshots, etc)
* Plan ahead for a phased release where available (always, if including feature flags)
* Must adhere to the [AppStore](https://developer.apple.com/app-store/review/guidelines/) and [Playstore](https://play.google.com/about/developer-content-policy/) regulations.

Post-release must haves:
* Retrieve that version (and its code) even once it's outdated by newer ones
* Have a clear understanding of the live issues the app is having, by version, including logs to help reproduce and debug

In the case of a critical issue:
* Don't freak out. Even if you could find and fix the issue in 30 seconds it'll probably not reach all users for a few days
* Due to the previous issue, first option should always be fix server-side, even if temporary, while the client rolls out a fix
* Once the required code is PR'd and merged, we must be able to produce quickly a hot-fix for that issue alone
* Depending on the criticality of the issue, request [expedite review](https://developer.apple.com/contact/app-store/?topic=expedite) << **DO NOT ABUSE THE EXPEDITE REQUESTS!!**

Tools involved:
* [Fastlane](https://fastlane.tools) (Allows us to run the building jobs independently from the CI platform through Ruby scripts)
* [CircleCI](https://www.circleci.com/) (CI platform, implements the branching strategy, runs the jobs using Fastlane)
* [Azure DevOps](https://dev.azure.com/) (CI platform, alternative to CircleCI)
* [BuddyBuild](https://www.buddybuild.com) (CI, α testing distribution)
* [Firebase](https://firebase.google.com/) (Push Notifications, feature flag managment, A/B testing)
* [Fabric](https://get.fabric.io) (Crash reporting)
* [TestFlight](https://itunesconnect.apple.com) (Pre-AppStore)


## Project's support scope
This is often overlooked, and when you do it's bad news. Mobile devices vary incredibly in their capacities. In an already restricted world this could mean the difference between something working as expected, lagging and right out crashing. Or something looking great, crammed and right out not fitting the screen.

Be **extremely** careful when a clients ask to support everything. Check our [device support document](/Documentation/device-support-matrix.md) to know the things that you should keep in mind when trying to define the scope for an App.

## Being a _good citizen_
Your software will be run on someone else's device, sometimes it'll be run on millions of someone _elses_' devices. It's their devices, but it's your responsibility to try and keep to the minimum:
* Battery consumption
* Data consumption
* Storage consumption

We can, sometimes, when required, make the tradeoff in the server. We can say, on exceptions, "we'll just buy more servers" or "we'll add RAM to that server" or "We can pay the extra bandwidth". But we can't add disk space to the users phone, we can't recharge their battery, we can't pay their phone bill if we retry an API call indefinitely while on roaming.

The above mentioned consumptions must be kept an eye on. Xcode and Android Studio, they both have tools to measure all these different variables.

## Design
Mobile design differs from web or desktop design in that we are doing so for a device that's intended to be used differently. This applies to product design as well. 20 options on a menu won't do it. Mobile apps are intended for being used quick, on often uncomfortable environments (such as on the bus, on a concert, on very high or very low lighting), with often low connectivity. All that's non essential must be stripped off.

* [iOS Design](/Documentation/ios-specific/ios-design-standard.md)
* [Android Design](/Documentation/android-specific/android-design-standard.md)

## Architecture
We try to stick with the latest technolgies (without falling for untested and greenish technologies) and the guidelines defined by Google and Apple.

* [iOS Architecture Patterns](/Documentation/ios-specific/ios-architecture-patterns.md)
* [Android Architecture Patterns](/Documentation/android-specific/Architecture/android-architecture-patterns.md)

## Coding Style
We try to follow every standard proposed and we enforce this standards by running our code through Lint validators on both platforms. For Kotlin on Android and Swift on iOS.

* [iOS Coding Stlye](/Documentation/ios-specific/ios-coding-style.md)
* [Android Coding Style](/Documentation/android-specific/android-coding-standard.md)

## Broadly adopted libraries
On both platforms we use different third party libraries, here is a list of the technology stack that we currently handle:

* [iOS Libraries](/Documentation/ios-specific/ios-libraries.md)
* [Android Libraries](/Documentation/android-specific/android-libraries.md)
