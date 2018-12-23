`System.CommandLine` provides a set of default features for both people using it to develop apps and for the users of those apps. This is a quick overview of some of those features. 

# Suggestions

Programs written using `System.CommandLine` have built-in support for tab completion. 

![t-rex-suggestions](https://user-images.githubusercontent.com/547415/50387753-ef4c1280-06b8-11e9-90c8-89466d0bb406.gif)

To enable it, the end user has to take a few steps once per shell, outlined [here](dotnet-suggest). Once this is done, completions will work for all apps written using `System.CommandLine`.

# Help

Command line applications typically have a way to get help, in order to understand the available options and their usage.

```console
> myapp -h
Usage:
  myapp [options]

Options:
  --int-option     An option whose argument is parsed as an int
  --bool-option    An option whose argument is parsed as a bool
  --file-option    An option whose argument is parsed as a FileInfo
  --version        Display version information
```

Users might be accustomed to different prefixes in different ecosystems, especially if your app targets .NET Core and is used cross-platform, so apps built on `System.CommandLine` understand many different ways of requesting help. The following are all equivalent:

```console
> myapp -h
> myapp /h
> myapp --help
> myapp -?
> myapp /?
```

# Version option

Providing a way to check the version of your app is helpful to your users.

`System.CommandLine` provides this by default. In the [help](Features-overview#Help) example you might have noticed an option, `--version`, that was not explicitly configured in the sample code. When you run your program with this option, you'll see something like this:

```console
> myapp --version
1.0.0
```

# Parse preview

Both users and developers often find it useful to see how an app will interpret a given input. One of the default features of a `System.CommandLine` app is the `[parse]` directive, which lets you preview a parse:

```console
> myapp [parse] --int-option not-an-int --file-option file.txt
[ myapp ![ --int-option <not-an-int> ] [ --file-option <file.txt> ] *[ --bool-option <False> ] ]
```

The `[parse]` directive tells the parser to parse the input and return a diagram of the result. Some things worth noting in the above example:

* Commands (`myapp`), their child options, and the arguments to those options are grouped using square brackets. 
* For the option result `![ --int-option <not-an-int> ]`, the `!` indicates a parsing error. `not-an-int` could not be parsed to the expected type.
* For the option result `*[ --bool-option <False> ]`, the `*` indicates that a value was not specified on the command line, so the parser's configured default was used. `False` is the effective value for this option.

# Debugging

When you're developing your app and you find that the parse preview isn't enough to explain something that's happening internally, the `[debug]` directive might help you. Set a breakpoint inside your code, prepend your command line input with `"[debug]"`, and hit `enter`:

```console
myapp [debug] --file-option does-not-exist.txt
Attach your debugger to process 14616 and then press any key.
```

Once you've attached your debugger to the specified process, press any key and execution will proceed to your breakpoint.

