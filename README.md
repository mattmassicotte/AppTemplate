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

This is a standard application target that builds for all Apple platforms that support standalone apps. That means it supports macOS, iOS, tvOS, and visionOS. Multi-platform targets are kind of amazing, and used to be extremely difficult to pull off. I **absolutely** love supporting multiple platforms, even if you don't intend on shipping to all. Especially macOS, since it doesn't require a simulator, and can build/test with much less overhead.

This app also includes to related test targets: `MultiPlatformAppTests` and `MultiPlatformAppUITests`.

## Contributing and Collaboration

I'd love to hear from you! Get in touch via [mastodon](https://mastodon.social/@mattiem), an issue, or a pull request.

I prefer collaboration, and would love to find ways to work together if you have a similar project.

I prefer indentation with tabs for improved accessibility. But, I'd rather you use the system you want and make a PR than hesitate because of whitespace.

By participating in this project you agree to abide by the [Contributor Code of Conduct](CODE_OF_CONDUCT.md).
