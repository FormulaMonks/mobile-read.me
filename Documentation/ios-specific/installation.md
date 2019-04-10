# Installation
## Xcode
Xcode is the default IDE for iOS and macOS platforms.
- **Install Xcode** from _macOS AppStore_ or [Apple's Xcode website](https://developer.apple.com/xcode/).

## Optional: RVM
Since _Cocoapods_ is a Ruby gem, we suggest that you install [rvm](https://rvm.io) and use the Ruby version you want instead of using system's Ruby.
This will prevent some future issues related with system permissions and if your current ruby setup gets broken you can just uninstall it and start over again easily.

> RVM is a command-line tool which allows you to easily install, manage, and work with multiple ruby environments from interpreters to sets of gems.

## Cocoapods
> CocoaPods is a dependency manager for Swift and Objective-C Cocoa projects.

Check if your project has a `Gemfile` file on the root and follow the steps:

### if the project has a `Gemfile`:
- `gem install bundler` (this will install [Bundler](https://bundler.io))
- `bundle install`

### if the project has NOT a `Gemfile`:
- `gem install cocoapods`
- `pod install`

### Note to developers about `Gemfile`s:
[Bundler](https://bundler.io) for Ruby is what [Cocoapods](https://cocoapods.org) is for iOS. Therefore, **Gemfile** for Ruby is what **Podfile** is for iOS.
This will avoid teammates have different tooling versions such as Cocoapods, Fastlane, Danger, etc.
_Provide Gemfile and use Bundler whenever is possible_. 

# Run
- Open `<project_name_workspace>` on Xcode.
- Tap the ▶️ button on the toolbar.
