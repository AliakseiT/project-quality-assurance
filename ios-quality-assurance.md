# iOS/Swift project Quality Assurance

The idea behind this document is to have a reference of how to create, develop and maintain an iOS project in [Karumi](https://karumi.com).

These are no hard rules, but we encourage them as we wrote this document based on our experience doing consultancy projects and in-house project. This document complements the [Project Quality Assurance](https://github.com/Karumi/project-quality-assurance) with all the specifics we follow for iOS projects written in Swift.

Although this document is written having in mind that the reader will be using Swift, most of the advice can be easily adapted to Obj-C.

## Feature kickstart

We all love to start new features. These are a few questions you should ask to prevent future delays.

* [Is the design prototype ready](./design-collaboration-guidelines.md)?
  - [ ] Does it include a style guide?
  - [ ] Is it prepared for devices with different sizes?
  - [ ] Does it have components that repeat themselves through various screens, if yes does it have a component guide?
  - [ ] Does it include any navigation prototype or guide?
  - [ ] Does it include empty states for screens with dynamic content?
  - [ ] Did we provide a list the interactions that will trigger a loading state?
  - [ ] Does it include a loading animations?
  - [ ] Are all the assets exported included placeholders?
  - [ ] Does it include examples of user-generated content (long names or numbers, etc.)?

* Localization
  - [ ] Are the sentences for this feature defined? If no, require them; if they are, consider if there is enough room to display them, especially the dynamic ones (the ones which will be displaying usernames, amount of money, etc.)

* Are the error scenarios well defined?
  - [ ] There is no internet connection, what this feature would do?
  - [ ] Our server is down, how the app should behave?
  - [ ] Are all the business error handled? If not, should they be?

* Server-side integration.
  - [ ] Is the API defined/available? If not, try to do it, at least explain how the model would look.

## Pull requests

* Guide the reviewers on how to test the new feature, the screens that are affected, how to access them. If there is any context requirement (user logged in, malfunctioning API, etc.) specify how did you test it.
* All pull requests must have screenshots of the new screens or UI changes if any. It is recommended to show changes on different devices and resolutions (small phones, tablets).
* If there are animations involved, record them and upload them to the PR.
* Specify if there are changes in any DB schema and how the evolution is being performed.
* If there is any change in how the app communicates with the server, link to the documentation of the endpoint being used.
* Include new permissions being requested to users.

## Issues

* Always include visuals on how the screen should look like (wireframes, mocks, etc).
* Add or link all your new assets to the issue.
* Contemplate common error scenarios such as no internet access, no permissions, etc.
* Include all the translation keys that are needed to implement the new features. Always include the English copy.
* List all the actions that have to be tracked in the new feature if applicable.
* Recommended tags for your issues besides the common ones: _UI_, _DB_, _API_, _STATS_.

## Fixing an issue

* Try to reproduce it. If it’s fixed, indicate when it was resolved. If not, ask for more information on the ticket.
* Attach media resources (images videos) showing how it looked like after your fix.
* Briefly explain what was happening.
* Add tests to assure that the scenario described above shouldn’t happen again.
* Keep track of this issue in the following release to check if it’s fixed if not, update information and reschedule this issue fix.

## Asking for review

Don’t forget you are not your code, the point of establishing a PR process is gathering feedback about how to improve the product you are working.

* Try to fix just one thing at a time.
* Use the template you can find in this repo where you’ll have to introduce the issue you are fixing/developing; what was needed and why the implementation is the way it is.
* Indicate how it can be tested, ideally it should be in an automatic way, but if not, explain why and the tests the reviewer will have to perform to check it’s working as expected.
* Indicate how it should be tested by hand. You might have forgotten some scenarios, and the only way to find them out is using exercising that code.
* Be respectful to your colleagues and answer or at least ACK all their comments.

## Reviewing

Think that nobody does their job poorly on purpose (or at least not the most of us). If something is not looking good, try to figure out why almost always there is a reason.

* The best way to have all the context is not the web tool used for PR, checkout the branch and open it.
* Try to understand why it’s done the way it is before suggesting changes.
* Don’t forget that this is about code quality, no about being right. Your comments or suggestion can be declined if there are reasons for so.
* If you can, provide snippets of code showing your idea/suggestion, it’s usually better to discuss it.
* Try to review the PR you have been asked to, your opinion may be relevant, that every developer wants to see their job reflected in the product. The longer a PR is open, the higher the possibility to get conflicts at merge time.

## Continuous integration

* Besides what's mandatory for every other project, in iOS it's also required to:
  * Verify your UI is being rendered correctly.
  * Check your API integration.

* You can use _swiftlint_ linting purposes.
* UI tests can be written in _KIF_ (functional approach), decorated with _Barista_ and _Snap_ (Snapshot based).
* Use _OHHTTPStubs_ to check your API integration.
* The CI environment should be used to upload new beta versions to your preferred service (Test Flight, Fabric, etc.).

## CVS

* Include a linting configuration when possible.
* Ideally, your project should contain an _EditorConfig_ file so that it's easier to work with other developers around different environments.
* Try to use a feature/fix branch approach, no one should code directly on master/develop.
* Create a branch for release tentatives, and those branches will be alive while the release is being checked, any change related to that release should be done following the same feature branch approach, but having that release branch as reference branch.
* Do not track more code than needed.
* Setup a proper ignore file, a good example for Swift can be found [here](https://github.com/github/gitignore/blob/master/Swift.gitignore).

## Project tooling

### Formatting/Linting

Be sure that the code styles is consistent across the whole project. You have two options, you can use:

* [SwiftLint](https://github.com/realm/SwiftLint)
* [SwiftFormat](https://github.com/nicklockwood/SwiftFormat)

### Tests

It’s important to verify that our app is doing what is suppose to do for that we can use:

* Verification: [Nimble](https://github.com/Quick/Nimble), [SwiftCheck](https://github.com/typelift/SwiftCheck), [ios-snapshot-test-case](https://github.com/uber/ios-snapshot-test-case/)
* HttpStubs: [OHHttpStub](https://github.com/AliSoftware/OHHTTPStubs)
* UI Testing: [KIF](https://github.com/kif-framework/KIF)

### Code generation

If some parts of the code can be generated automatically, let’s do that.

* Miscellanea: [Sourcery](https://github.com/krzysztofzablocki/Sourcery)
* API integration: [Swagger](https://github.com/swagger-api/swagger-codegen)

### CI/Integration

As said before, CI environment is mandatory for every project.

* Tool: [Fastlane](https://fastlane.tools)
* CI platform: [Bitrise](https://www.bitrise.io/) seems to be working fine for iOS and Android.

### Crashes

Keep track of how the app behaves in production.

* [Fabric](https://get.fabric.io) is the tool we’ve been using the most.

### Distribution

Distribute often; it’s an excellent way to gather feedback. For instance, we aim to deploy a weekly beta to our clients. This has wider implications that you could expect:

- Issues need to be defined to be fitted in one week.
- Feedback cycle is high enough to detect issues.
- Bugs are discovered early.

* [Testflight](https://developer.apple.com/testflight/) works perfectly and can be integrated with Fastlane

## Project

* Use one of the many tools to keep track of crashes and errors like _Crashlytics_.
* Create different flavors to point to different environments in your API calls.
* Configure your debug and release build types. If you need a different signing config for your beta distributions, create a different build type.
* Always disable stats tracking while running tests or using your app from a development environment. The only safe way to do it is to specifically block tracking from your production code. In order to detect a test environment, you can test if there is any test class loaded.
