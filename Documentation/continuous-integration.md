# Continuous integration

The idea of CI and CD (Continuous Delivery) is making our lives easier by automating the deployment process. Sending builds to QA, running UT against our code, running lint validations, deploying to the Playstore/App Store, etc.

CI and CD are an essential part of successful mobile application development: there are too many moving parts to expect a human to reliably perform all those tasks, all the time.

## Branches

We have different branches, defined in our [branching strategy](branching-strategy.md) document. When building some of these branches we should trigger different actions in our CI platform. Our main branches are `main`, `release`, `development` and the `feature` branches.

## Fastlane and CI

We need to talk about two things before moving forward; the **CI platform** and **Fastlane**. 

**Fastlane** is an **essential** technology that works independently of the CI platform that we choose. The Fastlane script is the one that is going to take care of building the App, running the UT, running the validations, deploying the the App Store/Playstore, pushing a new build for QA into Fabric or whatever. These different actions are called **lanes**. A lane gets trigger when building an specific branch. We need to move as much work as posible into Fastlane, this way if we need to switch from Jenkins to CircleCI or any other CI platform, we can do it without problem.

The **CI platform** that we choose to use doesn't really matter, the only thing that the CI platforms needs to do is trigger the different lanes depending on the branch that we are building. If we are building a `feature` branch we just want to build the App, run the UT and run the lint validations. If we are building a `development` branch, we want to build the App and send a build to QA through Fabric/HokeyApp or whatever tool we are using for distribution.

A key element to take away here is that this CI/CD pipeline is highlighy dependant on having access to several cloud-hosted elements, whether to obtain third party frameworks, run unit tests against Firebase, ... The retained suite of tools must be able to access those elements.

## Workflows

The CircleCi platform works with workflows as most of the CI platforms do. A workflow is just a job that is going to get executed when building different branches. These are the basic rules that we are going to be using for the different branches:

- Building on a `feature` branch:
  - Android:
    - Fetch code from the repository
    - Retrieve dependencies from various sources using Gradle
    - Update Fastlane if required
    - Build the project so we know that isn't broken
    - Run lint validations
    - Run all tests
    - We don't need to send the build anywhere since it's not really useful for QA testing on specific features
  - iOS:
    - Fetch code from the repository
    - Update Fastlane if required
    - Retrieve the distribution certificates and provisioning profile from developer.apple.com
    - Build the project so we know that isn't broken
    - Run lint validations with the swift lint option
    - Run all tests
    - Execute additional lint validations, leveraging the log file generated as a result of compiling the code before executing the tests
- Build on `development` branch:
  - Android:
    - Build the project so we know hat isn't broken.
    - Deploy the build to Fabric under the `android-qa` testing group. This testing group is only for QA and developers. The version name should be set to `v#{VERSION}-#{BUILD_NUMBER}` so QA can easily link the builds with the deploys on Fabric's Beta App. The changelog should contain the latest commits. The App name should contain the suffix "QA" at the end and use a different icon so its easier for QA to identify the App.
We don't need to run the lint validations or UT at this point.
    - Remember that this build must have all QA related options enabled, Charles proxy, if we have something like an environment switcher to switch between different APIs, all that must be available for QA in these builds.
    - Send a message to the Slack channel #android-qa letting the QAs know that a new version is available for testing.
  - iOS:
    - Perform all steps under `Building on a feature` branch for iOS, incrementing the build number and performing an ad-hoc build using the QA bundle identifier and provisioning profile.
    - Deploy the binary to Fabric under the `ios-qa` testing group. This testing group is only for QA and developers. The version name should be set to `v#{VERSION}(#{BUILD_NUMBER})` so QA can easily link the binary with the deploys on Fabric's Beta App. The changelog should contain the latest commits.  The App name should contain the suffix "QA" at the end and use a different icon so its easier for QA to identify the App.
    - This binary must have all QA related options enabled: environment switcher to switch between different APIs, exposing the Firebase Cloud Messaging Token, ....
    - Send a message to the Slack channel #ios-qa letting the QAs know that a new version is available for testing.
- Build on a `release` branch:
  - Android:
    - Build the project so we know that isn't broken.
    - Deploy the build to Fabric under the `android-milestone` testing group. This testing group contains everyone QA, developers, PMs, etc.  The version name should be set to `v#{VERSION}-#{BUILD_NUMBER}`. Changelog should be set to the latests commits. 
    - The deploy should be exactly the same one that we are going to push into the Playstore later. No QA related options should be available here.
    - Send a message to the Slack channel #android-qa letting the QAs know that a new version is available for testing.
  - iOS:
    - Perform all steps under `Building on a feature branch` for iOS, incrementing the build number and performing an ad-hoc build using the Production bundle identifier and provisioning profile.
    - Deploy the binary to Fabric under the `ios-milestone` testing group. This testing group contains everyone QA, developers, PMs, etc.  The version name should be set to `v#{VERSION}(#{BUILD_NUMBER})`. Changelog should be set to the latests commits. 
    - The binary should be identical to the one that we are going to push to the App Store later. No QA related options should be available here.
    - Send a message to the Slack channel #ios-qa letting the QAs know that a new version is available for testing.
- Build on `main` branch:
  - Android:
    - Build the project and send the deploy to the Google Playstore's Beta track. Once the deploy in the Beta track gets greenlighted by the stakeholders/client we can promote it to release.
  - iOS:
    - Perform all steps under `Building on a feature branch` for iOS, incrementing the build number relative to the previous TestFlight build and performing regular build using the Production bundle identifier and provisioning profile.
    - Deploy the binary to Test Flight.
    - Tag the code used to generate the binary using the name `release_#{VERSION}-#{BUILD_NUMBER}`
    - Once the binary gets greenlighted by the stakeholders/client we can publish it to the App Store.





