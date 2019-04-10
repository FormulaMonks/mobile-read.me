# Dependency Managers

### Carthage

Carthage is a dependency manager for Cocoa, available here: https://github.com/Carthage/Carthage

It offers an advantage over CocoaPods where dependencies are included in projects under a compiled form instead of the code being compiled alongside the project code.

##### Note

While working on a client's project, we realized that not including Carthage pre-compiled frameworks in the code repository had many disadvantages:

* When updating a dependency to a new version, each developer has to perform the `carthage update` command on their own machine.
* When updating a dependency to a new version, it's easy to be too general with the `carthage update` command and cause other dependencies to update to newer versions. This is dangerous as it may inadvertently introduce breaking changes, or behaviour changes that are not expected and won't be verified/tested. It may also result in developers not using the same versions of a third party library because the updates were performed at different times.
* Similar to the previous point, the CI/CD server is fetching itself the Carthage third party libraries and may not use the same versions as then ones present on each developer's machine.

We have therefore moved to the inclusion of pre-compiled third party frameworks obtained via Carthage into the code repository, and removed any CircleCI step that involved Carthage. This:

* Guarantees that the developer in charge of updating a third party library is the only one who has to fetch the updated version.
* The CI/CD server compiles exactly what's in the code repository and does not fetch any dependency itself.
* Other developers don't have to spend time running the `carthage update` command on all their workstations.
* The CircleCI workflows run time is reduced a little.