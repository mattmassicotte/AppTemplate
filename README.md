# AppTemplate
A template for building apps with Xcode 

Getting set up to build apps in Xcode might seem easy, since it comes with templates. However, Xcode is complex and those templates don't always set things up as I'd like. So, I thought I'd share a slightly opinionated approach to structuring an Xcode project. This follows a similar pattern I've used for [packages](https://github.com/mattmassicotte/PackageTemplate).

By no means is this a one-size-fits-all kind of problem. Projects can be tremendously complex, with all kinds of different needs. But, I still think what's here is at least a good starting point. If you have ideas for ways to improve this, please open an issue or PR!

## Overall Project Organization

This Xcode project makes use of [xcconfig](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project) files. I've been using these for a while, and I find it difficult to control my target settings without these. I don't always migrate every setting in, but I always use them when I need to change something.

`User.xcconfig`: a special config file that holds per-user settings. This comes in really handy for open source apps, but is unnecessary for any app that shares a single development team.
`Project.xcconfig`: global settings that apply to all targets within the project. Occationally it does make sense to override these defaults, but that's rare. `User.xcconfig` is included here.

This was all set up with Xcode 15.1.

## Building

This project seperates out signing and bundle identifier details. This makes sense for a project that may have collabrators that do not share code signing information.

- clone the repo
- `cp User.xcconfig.template User.xcconfig`
- update `User.xcconfig` with your personal information
- build/run with Xcode

## Targets

Because there can be such a variety of targets, I thought it would be useful to create a bunch. Your project almost certainly doesn't need them all.

### MultiPlatformApp

This is a standard application target that builds for all Apple platforms: macOS, iOS, tvOS, visionOS, watchOS, and catalyst. Multi-platform targets are kind of amazing, and used to be extremely difficult to pull off. I **absolutely** love supporting multiple platforms, even if you don't intend on shipping to all. Especially macOS, since it doesn't require a simulator, and can build/test with much less overhead.

This app also includes two related test targets: `MultiPlatformAppTests` and `MultiPlatformAppUITests`.

Note that supporting watchOS is a little special, since there are multiple types of watchOS apps. This is a true standalone app, with no iOS companion. I have a feeling this isn't what most people woudl want, but its possible so I included it. It's also a little strange to support both macOS and catalyst, but again, possible so why not.

### SharedFramework

This is a shared framework target, also built for all platforms. This is a really useful structure for storing common code between an app and other bundled executables, like a widget extension. I've chosen to use a dynamic library here. This is a trade-off between app launch and app size. A dynamic library will require a little more work at launch time. But in exchange, there's only one copy of the shared code within the app.

A static library, which is the default for SPM packages, will improve launch times. But, you'll have to pay a size penalty for each excutable that needs the shared code. Inceased binary sizes can also hurt launch times, so this isn't a trival thing to compare.

I wouldn't overthink this too much. Especially since a framework also is a logical organizational structure.

### ShareExtension

This is an extension target. It depends on and links against `SharedFramework`, and is embedded within `MultiPlatformApp`. However! macOS and watchOS do not support share extensions, so the embedding build phase has to be filtered by platform. This is a very powerful Xcode feature, and definitely something worth knowing about when building for multiple platforms.

Also, note that this target has to customize its bundle identifier because extension bundle ids have to be prefixed with their container app.

## Contributing and Collaboration

I'd love to hear from you! Get in touch via [mastodon](https://mastodon.social/@mattiem), an issue, or a pull request.

I prefer collaboration, and would love to find ways to work together if you have a similar project.

I prefer indentation with tabs for improved accessibility. But, I'd rather you use the system you want and make a PR than hesitate because of whitespace.

By participating in this project you agree to abide by the [Contributor Code of Conduct](CODE_OF_CONDUCT.md).
