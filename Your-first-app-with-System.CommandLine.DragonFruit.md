# Building your first app with System.CommandLine

This walkthrough will show you how to get started using System.CommandLine.DragonFruit to build a command line application.

## Create a new console app

Open a new console and run the following commands:

```console
> dotnet new console -o myApp
> cd myApp
```

## Install the System.CommandLine.DragonFruit package

```console
> dotnet add package --source https://dotnet.myget.org/F/system-commandline/api/v3/index.json System.CommandLine.DragonFruit -v 0.1.0-alpha-63519-01
```

## Add some code

Open `Program.cs`. You'll see that your `Main` method looks like this:

```csharp
static void Main(string[] args)
{
    Console.WriteLine("Hello World!");
}
```

If you run it, you'll see this:

```console
> ./myapp # or: > dotnet run
Hello World!
```

The default main only takes `string` arguments as an array. With DragonFruit, you can accept named arguments of various types and specify default values. Change your `Main` method to this:

```csharp
/// <summary>
/// My example app
/// </summary>
/// <param name="intOption">An option whose argument will bind to an int</param>
/// <param name="boolOption">An option whose argument will bind to a bool</param>
/// <param name="fileOption">An option whose argument will bind to a FileInfo</param>
static void Main(int intOption = 42, bool boolOption = false, FileInfo fileOption = null)
{
    Console.WriteLine($"The value of intOption is: {intOption}");
    Console.WriteLine($"The value of boolOption is: {boolOption}");
    Console.WriteLine($"The value of fileOption is: {fileOption?.FullName ?? "null"}");
}
```













