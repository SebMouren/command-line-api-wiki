# System.CommandLine Concepts

## Command

A command is a token on the command line that corresponds to an action that the app will perform. The simplest command line applications have only a root command. The app only does one thing. The way in which it does that one thing might vary, for example based on options and arguments that are passed, but it's basically the same thing. 

Other command line apps support subcommands, meaning they can do more than one thing. `dotnet build` and `dotnet restore` are examples of different subcommands.

When an app has subcommands, then the commands above them in the hierarchy typically do nothing by themselves. `dotnet add` is not a complete command because it has the subcommands `dotnet add package` and `dotnet add reference`.

In `System.CommandLine`, 

### Root Command

The root command in `System.CommandLine` corresponds to your app itself. In the following example, the executable name is `myapp` and the room command name is `myapp`.

```shell
> myapp [parse]
[ myapp *[ --int-option <42> ] *[ --bool-option <False> ] *[ --file-option ] ]

```

## Option

An option is a named parameter that can be passed to a command. Options usually follow one of a couple of syntax patterns. In POSIX command lines, you'll often see options represented like this:

```
> myapp --int-option 123
```

In Windows command lines, you'll often see options represented like this:

```
> myapp /int-option 123
```

While `System.CommandLine` can be configured to use these and other prefixes, the default is the POSIX style.

### Option aliases

In both POSIX and Windows command lines, it's common for some options to have aliases, which are usually short forms that are quicker to type. 

## Argument

An argument is a value passed to an option.

Arguments can have default values, expected types, and configurable arity. `System.CommandLine` will reject arguments that don't match these expectations. 

```shell
> myapp --int-option not-an-int
Cannot parse argument 'not-an-int' as System.Int32.

Usage:
  myapp [options]

Options:
  --int-option     An option whose argument will bind to an int
  --bool-option    An option whose argument will bind to a bool
  --file-option    An option whose argument will bind to a FileInfo

> myapp --int-option 1 --int-option 2
Option '--int-option' expects a single argument but 2 were provided.

Usage:
  myapp [options]

Options:
  --int-option     An option whose argument will bind to an int
  --bool-option    An option whose argument will bind to a bool
  --file-option    An option whose argument will bind to a FileInfo
```

In the first of these examples, a parse error resulted because the input "not-an-int" could not be converted to an `int`. In the second example, too many arguments were passed to `--int-option`. This is an example of an arity error. `--int-option` has an arity of exactly one (`ArgumentArity.ExactlyOne`), meaning that if the option is specified, a single argument must also be provided.

Boolean options, sometimes called "flags", have an arity of `ArgumentArity.ZeroOrOne`. This is because all of the following are valid ways to specify a `bool` option:

```shell
> myapp --bool-option
The value of intOption is: 42
The value of boolOption is: True
The value of fileOption is: null

> myapp --bool-option true
The value of intOption is: 42
The value of boolOption is: True
The value of fileOption is: null

> myapp --bool-option false
The value of intOption is: 42
The value of boolOption is: False
The value of fileOption is: null
```

`System.CommandLine` also knows how to bind other argument types. For example, enums and file system objects such as `FileInfo` and `DirectoryInfo` can be bound, and there is support for custom bindings so that you can define your own types. Any type that has a constructor taking a single string argument can be bound without having to write any custom code.


## Directives

In order to provide certain capabilities out of the box, so that you don't have to implement them yourself, `System.CommandLine` has a syntactic concept called "directives". A directive must conform to the following rules: 

* It is a token on the command line coming after your app's name but before any subcommands or options, and
* It is enclosed in square brackets.

The `[suggest]` directive is one example. This directive tells your app to parse the input but act on it by providing suggestions rather than executing a command handler. 

```shell
> .\myapp [suggest] f
--file-option
```

You may not use this directive directly yourself, but [`dotnet-suggest`](dotnet-suggest.md) uses it to provide shell completions.

As you're learning how to configure the parser, one directive in particular can be very useful: the `[parse]` directive. This one can also be useful to the user of your app when they're trying to learn how to use it. Here's an example:

```shell
> myapp [parse] --int-option not-an-int --file-option file.txt
[ myapp ![ --int-option <not-an-int> ] [ --file-option <file.txt> ] *[ --bool-option <False> ] ]

```

The `[parse]` directive tells the parser to parse the input and return a diagram (`ParseResult.Diagram()`) of the result. Some things worth noting in the above example:

* Commands (`myapp`), their child options, and the arguments to those options are grouped using square brackets. 
* For the option result `![ --int-option <not-an-int> ]`, the `!` indicates a parsing error. `not-an-int` could not be parsed to the expectd type.
* For the option result `*[ --bool-option <False> ]`, the `*` indicates that a value was not specified on the command line but the parser's configured default was used. `False` is the effective value for this option.

## Middleware Pipeline

While each command has a handler which `System.CommandLine` will route to based on input, there is also has a mechanism for taking some other action based on that input. The directives above short circuit the call to a command handler and take some other action. Other examples include the `--version` option that can be included in your app by calling `CommandLineBuilder.AddVersionOption()`, or the help support provided by calling `CommandLineBuilder.UseHelp()`.

The mechanism that provides this is `System.CommandLine`'s middleware pipeline. It allows you to configure a chain of calls that can take action based on a `ParseResult` and return early, or call the next item in the chain. The last call in the chain is the handler for the specified command.

You can add a call to this pipeline by calling `CommandLineBuilder.UseMiddleware.` Here's an example that enables a custom directive:

```csharp
commandLineBuilder.UseMiddleware(async (context, next) => {
    if (context.ParseResult.Directives.Contains("just-say-hi"))
    {
        context.Console.Out.WriteLine("Hi!");
    } else
    {
        await next(context);
    }
});
```
```shell
> myapp [just-say-hi]
Hi!
```

## Renderer