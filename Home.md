# Welcome to the System.CommandLine wiki!

## Our goal

_Give your users a great experience with your .NET command line applications while letting you focus on the application you're writing._

## Why another command line parser?

Since the beginning, .NET has provided the `System.Console` API, which provides ways to read, write, and redirect standard input and output from the console,reposition the cursor, and change colors. Parsing command line input was left for the developer to implement. Tab completion was available only in PowerShell.

With .NET Core, .NET programs increasingly run on Linux and MacOS, where users have long been accustomed to ANSI terminals and rich shell completions. Providing that kind of experience using .NET has been a lot of work for developers. The new `System.CommandLine` is an attempt to make some of those things easier.

### History

This effort has its roots in the building of the `dotnet` CLI, which put significant demands on its parser. While there were a number of .NET command line parsing libraries available, including more than a couple built at Microsoft, none of them quite had the set of capabilities we needed. With the arrival global tools, we wanted to improve upon and share some of that work.

1. _Parsing._ The parse operation in `System.CommandLine` produces a `ParseResult` that can be used independently of any invocation. This was important for being able to test your parsing in isolation. We also provide a tokenizer to mimic what happens before `Main(string[])` is called, so you can test using a `string` rather than a `string[]` as parser input.

The parse model also supports altering and re-parsing input. This enabled features such as [suggestions](Features-overview#Suggestions) and [directives](Command-line-syntax.md#directives).

2. _Middleware._ We wanted to provide features such as suggestions, help, parse analysis, exception handling, and parse debugging, and we wanted these features to be extensible while remaining separate from your app's core logic. To enable these things, `System.CommandLine` provides a composable chain of responsibility in between the parsing operation and the invocation.

3. _Tab completion._ Tab completion is implemented using shell scripts that are crafted per app and per shell. We wanted a model that allowed your app to be the source of truth, for suggestions to be extensible, and for the work to add support for additional shells to be minimal.

4. _Rendering._ Rendering output such as table layouts and animations is a lot of work, especially if you intend for that output to look correct regardless of whether you're writing to a non-ANSI terminal, an ANSI terminal, or when output is redirected. (This is the newest part of the project. It will be split into a separate assembly and the API is still very unstable.)

## Try it out

The simplest way to create your parser is with the experimental [DragonFruit app model](DragonFruit-overview) app model. This works well if you have a single layer of commands (no subcommands).

[Your first app with System.CommandLine.DragonFruit](Your-first-app-with-System.CommandLine.DragonFruit)

For more complex scenarios, you can use the core APIs directly.

[Your first app with System.CommandLine](Your-first-app-with-System.CommandLine)

## Contribute

There's still a good deal of work to be done to make `System.CommandLine` great. If you want to help, please open an [issue](https://github.com/dotnet/command-line-api/issues) or [contribute](https://github.com/dotnet/command-line-api/blob/master/CONTRIBUTING.md).

![layers](https://user-images.githubusercontent.com/547415/50188724-5fcceb00-02d7-11e9-9c33-4b91d70f963e.png)
