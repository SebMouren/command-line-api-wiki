# Welcome to the System.CommandLine wiki!

## Our goal

_Give your users a great experience with your .NET console applications while letting you focus on the application you're writing._

.NET has ways to interact with the console via `System.Console` and `Main` command arguments, but it does not provide an easy route to building command line applications that meet today's expectations. This is particularly true in the Linux and Mac ecosystem where users are accustomed to ANSI terminals and rich shell completions.

The new `System.CommandLine` sets out to:

* Reduce the time programmmers spend providing a command line API (CLI)
* Improve the experience of the users of CLI programs

## More information

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

