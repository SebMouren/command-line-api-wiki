# Using the System.CommandLine API

To illustrate how to use System.CommandLine API, this walkthrough reimplements `dotnet tool` portion of the .NET Core SDK, using the sample app `dotmetal`. A simpler approach is offered by the DragonFruit application model and we hope additional application models will be created to fit other scenarios. You want to use the System.CommandLine API when the application models don't support features you're application needs. This sample is called `dotmetal` because this is how you work "on the metal" directly with System.CommandLine.

## The basic parser

The `Main` method of the application calls a method that defines the parser, builds the parser, and invokes the parser passing the args array passed to Main method.

```c#
using System.CommandLine.Invocation;
using System.Threading.Tasks;

namespace DotMetal
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var parser = CreateCli.GetParserBuilder()
                          .Build();
            await parser.InvokeAsync(args);
        }
    }
}
```

The `GetParserBuilder` method returns a `CommandLineBuilder`. The `CommandLineBuilder` allows you to gather all the details about your syntax and how you want the pipeline to work. `Build` creates the actual `Parser` instance, which is immutable during the evaluation, validation and invocation phases. 

The main method iteself doesn't need to know anything about the syntax of the CLI. 

The `GetParserBuilder` method creates a CommandLineBulider which understands both commands and the middleware pipeline. Since the `tool` command is just one part of the syntax, the `GetParserBuilder` method calls a method for each of the commands that will eventually be supported:

```C#
        public static CommandLineBuilder GetParserBuilder()
        {
            var builder =  new CommandLineBuilder()
                .AddCommand(GetToolCommand());
            // Additional commands would be defined here
            return builder;
        }
```

The `GetToolCommand` method defines the syntax and the code that runs if the command is requested by the user:

```C#
        private static Command GetToolCommand()
        {
            var command = new Command("tool", Strings.toolDescription);
             command.AddCommand(new Command("list", Strings.toolListDescription,
                new Option[] { 
                     new Option(new string[] { "--global", "-g" }, Strings.toolGlobalOptionDescription),
                     new Option("--tool-path", Strings.toolPathOptionDescription,
                                new Argument<DirectoryInfo>().ExistingFilesOnly()) 
                     },
                handler: CommandHandler.Create<bool, DirectoryInfo>(ToolActions.List)));
            // Additional subcommands would be defined here
            return command;
        }
```

Note that the descriptions are constants in a separate file. I find the structure and syntax of the CLI becomes difficult to comprehend if the descriptions are placed in line. In a real application, a method call could provide localized descriptions. 

There are two options, one of which is a boolean option that has an alias, and the other is a option that takes a directory name as an argument. Since `DirectoryInfo` has a constructor that takes a single string, it can be used as the argument type. The application requires an existing directory, and fluent validation specifies this. The `ExistingFilesOnly` validation will be renamed to `ExistingPathsOnly`. These are plural becuase the same validation could be used with mutliple files or paths, just by specifying  `IEnumerable<DirectoryInfo>` as the argument type. 

The `handler` argument to the `Command` constructor indicates what action should be performed when the command is requested. In this case, it is a static method on the ToolActions class. Later in this walkthrough, you'll see this as an isntance of a class implementing an interface, to make mocking your application easier. 

Adding the next subcommand, adds some new options, but also results in a new copy of information for the `--global` and `--tool-path` options. 

```C#
            command.AddCommand(new Command("upgrade", Strings.toolUpgradeDescription,
                new Option[]
                { 
                    new Option(new string[] { "--global", "-g" }, Strings.toolGlobalOptionDescription),
                    new Option("--tool-path", Strings.toolPathOptionDescription,
                            new Argument<DirectoryInfo>().ExistingFilesOnly()),
                    new Option("--configfile", Strings.configFileOptionDescription,
                            new Argument<FileInfo>().ExistingFilesOnly()),
                    new Option("--add-source", Strings.addSourceOptionDescription,
                            new Argument<string>()),
                    new Option("--framework", Strings.frameworkOptionDescription,
                            new Argument<string>())
                },
                handler: CommandHandler.Create<bool, DirectoryInfo, string, FileInfo, string, string, StandardVerbosity>(ToolActions.Install),
                argument: new Argument<string>()));               
```

Repeating the details of the `--global` and `--tool-path` options seems like a terrible idea! Each option must be new - you can't just create one and reuse it. But you can create a method that creates them and call it in each location. Local methods make sense for the options are specific to the `tool` subcommand. There is also a `--verbosity` option that is used consistently in the .NET Core CLI. This is a method on the class.

The full class, with only  `tool` defined is:

```C#
 using System;
using System.CommandLine;
using System.CommandLine.Builder;
using System.CommandLine.Invocation;
using System.IO;

namespace DotMetal
{
    public static class CreateCli
    {

        public static CommandLineBuilder GetParserBuilder()
        {
            var builder =  new CommandLineBuilder()
                .AddCommand(GetToolCommand());
            return builder;
        }

        private static void InfoHandler() => Console.WriteLine("Output some info");

        private static Command GetToolCommand()
        {
            var command = new Command("tool", Strings.toolDescription);
            command.AddCommand(new Command("install", Strings.toolInstallDescription,
                new Option[] { Global(), ToolPath(), Version(), ConfigFile(), AddSource(), Framework(), StandardVerbosity() },
                handler: CommandHandler.Create<bool, DirectoryInfo, string, FileInfo, string, string, StandardVerbosity>(ToolActions.Install),
                argument: new Argument<string>()));
            command.AddCommand(new Command("uninstall", Strings.toolUninstallDescription,
                new Option[] { Global(), ToolPath() },
                handler: CommandHandler.Create<bool, DirectoryInfo>(ToolActions.Uninstall)));
            command.AddCommand(new Command("list", Strings.toolListDescription,
                new Option[] { Global(), ToolPath() },
                handler: CommandHandler.Create<bool, DirectoryInfo>(ToolActions.List)));
            command.AddCommand(new Command("update", Strings.toolUpdateDescription,
                new Option[] { Global(), ToolPath(), ConfigFile(), AddSource(), Framework(), StandardVerbosity() },
                handler: CommandHandler.Create<bool, DirectoryInfo, FileInfo, string, string, StandardVerbosity>(ToolActions.Update),
                argument: new Argument<string>()));
            return command;

            Option Global()
              => new Option(new string[] { "--global", "-g" }, Strings.toolGlobalOptionDescription);
            Option ToolPath()
               => new Option("--tool-path", Strings.toolPathOptionDescription,
                      new Argument<DirectoryInfo>().ExistingFilesOnly());
            Option Version()
               => new Option("--version", Strings.versionOptionDescription,
                      new Argument<string>());
            Option ConfigFile()
               => new Option("--configfile", Strings.configFileOptionDescription,
                      new Argument<FileInfo>().ExistingFilesOnly());
            Option AddSource()
                => new Option("--add-source", Strings.addSourceOptionDescription,
                      new Argument<string>());
            Option Framework()
              => new Option("--framework", Strings.frameworkOptionDescription,
                      new Argument<string>());
        }
        private static Option StandardVerbosity()
             => new Option(new string[] { "--verbosity", "-v" }, Strings.verbosityOptionDescription,
                      new Argument<StandardVerbosity>());
    }
}
```

## Letting the pipeline handle common problems

There are a number of things all (or almost all) console applications should do that can be automated by the System.CommandLine pipeline. These include:

* Displaying help
* Offering suggestions
* Diagramming how the parser understood a particular command, at least for debugging
* Pausing to allow you to attach a debugger
* Reporting parser errors, such as validation failures or unexpected tokens
* Providing graceful exception handling

All of these operations can be added to the pipeline with extension method on the `CommandLineBuilder`


```c#
using System.CommandLine.Invocation;
using System.Threading.Tasks;

namespace DotMetal
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var parser = CreateCli.GetParserBuilder()
                          .UseHelp()
                          .UseSuggestDirective()
                          .UseParseDirective()
                          .UseDebugDirective()
                          .UseParseErrorReporting()
                          .RegisterWithDotnetSuggest()
                          .UseExceptionHandler()
                          .Build();
            await parser.InvokeAsync(args);
        }
    }
}

```

## Alternatives for invocation handler

The invocation is where the important code of your application runs! 

Defining the action to be performed within the structural definition allows the correct values to be automatically bound and passed to the action. The code performing the action can generally be ignorant of all aspects of the syntax used to call it. 

The parser can also parse into a `Result` structure, but it is a bit more difficult to interact with. Currently, you need to use the result directly to access arguments and options on the outer commands of the request. For example, `dotnet add` has an argument for project name which occurs in the syntax prior to the `package` or `reference` subcommand.

The invocation can be any delegate matching the signature. This includes lambda expressions, static methods in a specific class, or methods on an instance that is available. 

## A special case

In the current design, only leaf commands' handlers will be executed. Triggering a handler based on an option on a command that is not a leaf (for example, `dotnet --info`), is not supported. This behavior can be accomplished using middleware. This is how help and `CommandLineBuilder.AddVersionOption` are implemented.

Options on the root command could also be implemented as commands - `dotnet --info` actually does something. This works today, but help is not correct. 
