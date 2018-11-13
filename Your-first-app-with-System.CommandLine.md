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
> dotnet add package --source https://dotnet.myget.org/F/system-commandline/api/v3/index.json System.CommandLine.DragonFruit -v 0.1.0-alpha-63513-01
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
using System.Threading.Tasks;
```

Now change your `Main` method to this:

```csharp
static async Task Main(string[] args)
{
    // Create some options and a parser
    Option intOption = new Option(
        "--an-int", 
        "An option that accepts an int as an argument",
        new Argument<int>());

    var rootCommand = new RootCommand();
    rootCommand.AddOption(intOption);
    rootCommand.Handler = CommandHandler.Create<int>(anInt => Console.WriteLine($"Parsed: {anInt}"));

    // Parse the incoming args and invoke the handler
    await rootCommand.InvokeAsync(args);
}
```

You're ready to run your program.

```console
> dotnet run -- --an-int 123
Parsed: 123
```

