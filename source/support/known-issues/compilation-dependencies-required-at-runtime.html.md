---
title: Extension compilation system dependencies are required at runtime
---

## Affected components

- AppSignal for Elixir package versions: `v1.8.0` and later.
- AppSignal for Ruby package versions: `v2.7.0` and later, but less likely to occur. Read more in the [description](#description).
- Systems: Alpine Linux only.

## Description

After installing AppSignal for Elixir 1.8.0 or higher, or AppSignal for Ruby 2.7.0 or higher, no more data is being received by AppSignal. During the start of the app that has AppSignal installed an error message is printed. The app itself is otherwise unaffected.

A new build of the AppSignal extension is requiring compilation dependencies to be installed on the machine the parent app is executed. This change in behavior, this new requirement, was unintentionally introduced in these AppSignal releases. This new requirement is only present for the "musl"-build, which is used for Alpine Linux hosts and very old "libc" installations. For more information on which build is used, see the [Supported Operating Systems](/support/operating-systems.html#linux) page.

This issue is most commonly seen with Elixir apps that have a two stage release process. On one machine the app is compiled and packaged into a release with a tool such as [Distillery](https://github.com/bitwalker/distillery). The first stage has all the [required compilation dependencies](/support/operating-systems.html), but the second stage does not.

In these release something goes wrong that dynamically links the extension to a system library on the compile stage machine, one that isn't present on the second stage machine. The missing package causes the AppSignal extension to be unable to start and deactivates itself.

## Symptoms

- No data is being received by AppSignal from your app.
- When your application starts AppSignal for Elixir prints the following message:

    ```
    [error] Failed to start AppSignal. Please run the diagnose task (https://docs.appsignal.com/elixir/command-line/diagnose.html) to debug your installation.
    ```
- The `install.log` file in the AppSignal package contains a platform specific error message.

## Workaround

1. Install the system dependencies for your Operating System as listed on the [Supported Operating Systems page](/support/operating-systems.html) on the host your app is running.
2. Downgrade to AppSignal for Elixir version 1.7.2 or AppSignal for Ruby version 2.6.1.

## Solution

None available at this time. We want to fix this in a future release.

Please let us know at support@appsignal.com if you currently have this issue. It would help us a lot if you provide the following information:

- AppSignal diagnose ([Ruby](/ruby/command-line/diagnose.html) / [Elixir](/elixir/command-line/diagnose.html)) report token. (Send the report to us when prompted.)
- A description of your app's build/compile/release process.