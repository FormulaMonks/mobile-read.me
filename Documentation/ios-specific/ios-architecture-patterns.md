# iOS Architecture Patterns

iOS's `UIKit` is built for MVC. This pattern has many downsides. The first one being huge `UIViewController`s, state jumping all around and nearly impossible to unit test code.

To address many of these concerns MVVM was introduced (by Microsoft! 😱), but MVC's being such common across domains and platforms, it was [quickly adopted by the iOS community](https://www.objc.io/issues/13-architecture/mvvm/). Combined with FRP, it's super powerful and easy to unit test.

There are [alternatives](https://www.objc.io/issues/13-architecture/viper/) and people are trying to [improve MVC](https://davedelong.com/blog/2017/11/06/a-better-mvc-part-1-the-problems/), but MVVM is right now pretty much the standard go to.
