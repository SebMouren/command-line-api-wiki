# Welcome to the System.CommandLine wiki!

## Our goal

_Give your users a great experience with your .NET console applications while letting you focus on the application you're writing._

## Why another command line library?

Since the beginning, .NET has provided the `System.Console` API, which provides ways to read, write, and redirect standard input and output from the console, reposition the cursor, and change colors. 

With .NET Core, .NET programs increasingly run on Linux and MacOS, where users have long been accustomed to ANSI terminals and rich shell completions. Providing that kind of experience using .NET has been a lot of work for developers. The new `System.CommandLine` is an attempt to make some of those things easier.

* [History](History)
* [Functional goals](Functional-goals)
* [Technical motivations](Technical-motivations)

## Try it out

The simplest way to create your parser is with the experimental [DragonFruit app model](DragonFruit-overview) app model. This works well if you have a single layer of commands (no subcommands).

[Your first app with System.CommandLine.DragonFruit](Your-first-app-with-System.CommandLine.DragonFruit)

For more complex scenarios, you can use the core APIs directly.

[Your first app with System.CommandLine](Your-first-app-with-System.CommandLine)

## Contribute

There's still a good deal of work to be done to make `System.CommandLine` great. If you want to help, please comment on or open an [issue](https://github.com/dotnet/command-line-api/issues), or [contribute](https://github.com/dotnet/command-line-api/blob/master/CONTRIBUTING.md).

