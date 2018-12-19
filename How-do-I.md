# How do I...?

## How do I call a method?

The simplest case for invoking your code, if you have a program so simple that it has no inputs beyond invocation itself, would look like this:

```csharp
static void Main()
{
    var rootCommand = new RootCommand();

    rootCommand.Handler = CommandHandler.Create(() =>
    {
        /* do something */
    });

    rootCommand.InvokeAsync(args).Wait();
}
```

Of course, if your program is so simple that is has no inputs, you probably didn't need a command line parser and you can `/* do something */` directly in the body of `Main`. Nonetheless, this will give you some additional [features](Features-overview).

## How do I pass parameters to a method?

Usually, your `/* do something */` method has parameters and you would like these to be specified using command line options. 

```csharp
public void DoSomething(int anInt, string aString)
{
   /* do something */
}
```

There are currently two models for configuring the `System.CommandLine` parser to pass these parameters.

### Syntax-first

One approach that you can use is to configure the parser directly by adding `Option`s to your `RootCommand`.

```csharp
static void Main()
{
    var rootCommand = new RootCommand();

    rootCommand.Add(new Option("--an-int"));
    rootCommand.Add(new Option("--a-string"));

    rootCommand.Handler = CommandHandler.Create<int, string>(DoSomething);

    rootCommand.InvokeAsync(args).Wait();
}
```

Parameters are matched using a naming convention that converts camel-cased parameters to kebab-cased options. In this example, the option `--an-int` matches parameter `anInt` on the `DoSomething` method.

### Method-first

Another approach is to let `System.CommandLine` configure the parser for you based on your method signature using the `Command.ConfigureFromMethod` extension method. (The  [DragonFruit](Your-first-app-with-System.CommandLine.DragonFruit) app model uses this approach.)


```csharp
static void Main()
{
    var rootCommand = new RootCommand();

    MethodInfo method = typeof(Program).GetMethod(nameof(DoSomething));

    rootCommand.ConfigureFromMethod<int, string>(method);

    rootCommand.InvokeAsync(args).Wait();
}

```

`ConfigureFromMethod` adds options to your command based on the parameters of the specified method. Options are created using a naming convention that converts camel-cased parameters to kebab-cased options. In this example, the parameter `anInt` generates an option with the alias `--an-int`;

## How do I add an alias to an option or command?

Both commands and options support [aliases](Concepts#Aliases). You can add an alias to an option like this:

```csharp
    var option = new Option("--verbose");
    option.AddAlias("-v");
```

Given this alias, the following command lines will be equivalent:

```console
> myapp -v
> myapp --verbose
```

Command aliases work the same way.

```csharp
    var command = new Command("serialize");
    command.AddAlias("serialise");
```

The following command lines will be equivalent:

```console
> myapp serialize
> myapp serialise
```

## How do I add a subcommand?

Commands can have child commands and can nest as many levels as you like. You can add a subcommand like this:

```csharp
var command = new RootCommand("command");
var subcommand = new Command("subcommand");
command.Add(subcommand);
var subsubcommand = new Command("subsubcommand");
subcommand.Add(subsubcommand);

```

The innermost subcommand in this example can be invoked like this:

```console
> command subcommand subsubcommand
```

Collection initializer syntax is supported, so the following is equivalent:

```csharp
var command = new RootCommand("command")
{
    new Command("subcommand")
    {
        new Command("subsubcommand")
    }
};
```
