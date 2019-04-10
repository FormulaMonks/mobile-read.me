# Third-party Libraries

Networking: [Alamofire](https://github.com/Alamofire/Alamofire)  
Logging: [CocoaLumberjack](https://github.com/CocoaLumberjack/CocoaLumberjack)  
Functional Reactive Programming: [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa)  
Behavior-driven development testing framework: [Quick](https://github.com/Quick/Quick)

Libraries (dependencies) should be kept at minimum. Only include those that really add value to the project and that are broadly adopted (and maintained) by the community.

When (if) including one of these, always do so under Carthage (preferred, shorted build times) or CocoaPods (OK). This will help you keep track of what's in your project, and update it if necessary.
