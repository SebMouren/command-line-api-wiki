# Welcome to the System.CommandLine wiki!

## Our goal

_Give your users a great experience with your .NET console applications while letting you focus on the application you're writing._

`System.CommandLine` is a set of libraries for building command line applications, including parsing, invocation, and rendering.

The new `System.CommandLine` sets out to:

* Reduce the time programmmers spend providing a command line API (CLI)
* Improve the experience of the users of CLI programs

You can read more about it here:

* [History](History)
* [Functional goals](Functional-goals)
* [Technical motivations](Technical-motivations)

## Try it out

[![Build Status](https://dev.azure.com/dnceng/public/_apis/build/status/dotnet/command-line-api/command-line-api?branchName=master)](https://dev.azure.com/dnceng/public/_build/latest?definitionId=337&branchName=master)

The simplest way to create your parser is with the experimental [DragonFruit app model](DragonFruit-overview). This works well if you have a single layer of commands (no subcommands).

[Your first app with System.CommandLine.DragonFruit](Your-first-app-with-System.CommandLine.DragonFruit)

For more complex scenarios, you can use the core APIs directly.

[Your first app with System.CommandLine](Your-first-app-with-System.CommandLine)

## Contribute

There's still a good deal of work to be done to make `System.CommandLine` great. If you want to help, please comment on or open an [issue](https://github.com/dotnet/command-line-api/issues), or [contribute](https://github.com/dotnet/command-line-api/blob/master/CONTRIBUTING.md).

