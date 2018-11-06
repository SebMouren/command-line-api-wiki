# DragonFruit

The entry point for a normal .NET console application looks like this:

```csharp
static void Main(string[] args)
{
    Console.WriteLine("Hello World!");
}
```

Interpreting the `string[]` arguments into behaviors has been left as a task for the developer. Did the user ask for help? Did they pass invalid input? Does the input map correctly to the types that you need if they're not `string`? These problems are not solved for you.

What if you could declare a strongly-typed `Main` method? This was the question that led to the creation of the experimental app model called "DragonFruit", which allows you to create an entry point with arguments of various types and default values, like this:

```csharp
static void Main(int intOption = 42, bool boolOption = false, FileInfo fileOption = null)
{
    Console.WriteLine($"The value of intOption is: {intOption}");
    Console.WriteLine($"The value of boolOption is: {boolOption}");
    Console.WriteLine($"The value of fileOption is: {fileOption?.FullName ?? "null"}");
}
```

DragonFruit handles help requests, parsing errors, and argument binding for you.

```console
> dotnet run
The value of intOption is: 42
The value of boolOption is: False
The value of fileOption is: null 
```

You don't need to write any special code to get help support.

```console
> dotnet run -- -h
Usage:
  myapp [options]

Options:
  --int-option     intOption
  --bool-option    boolOption
  --file-option    fileOption
```

If you want more informative help, you can add it using standard XML comments:

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

The text of those comments will be shown when a user requests help.

```console
> dotnet run -- -h
Usage:
  myapp [options]

Options:
  --int-option     An option whose argument will bind to an int
  --bool-option    An option whose argument will bind to a bool
  --file-option    An option whose argument will bind to a FileInfo
```

You can try out DragonFruit by installing the latest preview package.

```console
> dotnet add package --source https://dotnet.myget.org/F/system-commandline/api/v3/index.json System.CommandLine.DragonFruit -v 0.1.0-alpha-63506-01
```



