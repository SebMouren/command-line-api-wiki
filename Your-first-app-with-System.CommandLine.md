# Building your first app with System.CommandLine

This walkthrough will show you how to get started using System.CommandLine to build a command line application.

## Create a new console app

Open a new console and run the following commands:

```console
> dotnet new console -o myApp
> cd myApp
```

## Install the System.CommandLine.Experimental package

```console
> dotnet add package --source https://dotnet.myget.org/F/system-commandline/api/v3/index.json System.CommandLine.Experimental -v 0.1.0-alpha-63529-01
```

## Add some code

Open `Program.cs`. At the top, add a `using` directive:

```csharp
using System.CommandLine;
```

Your `Main` method looks like this:

```csharp
static void Main(string[] args)
{
    Console.WriteLine("Hello World!");
}
```

Now, let's add a parser. 

You'll need a few more `using` directives:

```csharp
using System.CommandLine;
using System.CommandLine.Invocation;
using System.IO;
using System.Threading.Tasks;
```

Now change your `Main` method to this:

```csharp
static int Main(string[] args)
{
    // Create some options and a parser
    Option intOption = new Option(
        "--int-option",
        "An option whose argument is parsed as an int",
        new Argument<int>(defaultValue: 42));
    Option boolOption = new Option(
        "--bool-option",
        "An option whose argument is parsed as a bool",
        new Argument<bool>());
    Option fileOption = new Option(
        "--file-option",
        "An option whose argument is parsed as a FileInfo",
        new Argument<FileInfo>());

    // Add them to the root command
    var rootCommand = new RootCommand();
    rootCommand.AddOption(intOption);
    rootCommand.AddOption(boolOption);
    rootCommand.AddOption(fileOption);

    rootCommand.Handler = CommandHandler.Create<int, bool, FileInfo>((i, b, f) =>
    {
        Console.WriteLine($"The value for --int-option is: {i}");
        Console.WriteLine($"The value for --bool-option is: {b}");
        Console.WriteLine($"The value for --file-option is: {f?.FullName ?? "null"}");
    });

    // Parse the incoming args and invoke the handler
    return rootCommand.InvokeAsync(args).Result;
}
```

You're ready to run your program.

```console
> dotnet run -- --int-option 123
The value for --int-option is: 0
The value for --bool-option is: False
The value for --file-option is: null
```

This program is equivalent to the one demonstrated in [Your first app with System.CommandLine.DragonFruit](Your-first-app-with-System.CommandLine.DragonFruit).

To explore its features, take a look at [Features: overview](Features-overview)
