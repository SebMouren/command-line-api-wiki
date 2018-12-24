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

# Adaptive rendering

ANSI terminals support a variety of features by including ANSI escape sequences in standard input and output. These sequences can control the cursor, set text attributes and colors, and more. Windows [recently joined](https://blogs.msdn.microsoft.com/commandline/2018/06/27/windows-command-line-the-evolution-of-the-windows-command-line/) Linux and Mac in supporting these features, but it's not turned on by default. It can be enabled programmatically.

`System.Console.Rendering` adds support for detecting and setting terminal settings. It also provides an API that can write output that looks correct based on those settings as well as when output is redirected, as is commonly the case on a build server or when your command line app is called by another command line app.

The following are examples of output rendered by the same view code in these three different contexts.

In PowerShell on Windows with virtual terminal mode enabled:

![ansi](https://user-images.githubusercontent.com/547415/50388667-575b2280-06d2-11e9-91ae-36e8ffabbf8a.png)

In PowerShell with virtual terminal mode disabled:

![non-ansi](https://user-images.githubusercontent.com/547415/50388673-85d8fd80-06d2-11e9-844b-4690e4b4ab5a.png)

Redirected to a text file:

```
Directory: C:\dev\command-line-api\build

Name               Created             Modified          
build.ps1          10/6/2018 10:56 AM  11/4/2018 7:10 PM 
build.sh           10/6/2018 10:56 AM  11/4/2018 7:10 PM 
cibuild.cmd        10/6/2018 10:56 AM  10/6/2018 10:56 AM
cibuild.sh         10/6/2018 10:56 AM  10/6/2018 10:56 AM
NuGet.props        10/6/2018 10:56 AM  10/6/2018 10:56 AM
SignToolData.json  10/6/2018 10:56 AM  11/19/2018 1:56 PM
Toolset.proj       10/6/2018 10:56 AM  10/6/2018 10:56 AM
Versions.props     10/6/2018 10:56 AM  11/19/2018 1:56 PM

```

The raw text written to standard out in the first example is this:

```
[1;1H[39m[49m[2;1HDirectory: [38;2;235;30;180mC:\dev\command-line-api\build[39m[39m[49m[3;1H[39m[49m[4;1H[4mName[24m[39m[49m               [4;20H[4mCreated[24m[39m[49m             [4;40H[4mModified[24m[39m[49m          [5;1H[37mbuild.ps1[39m[39m[49m          [5;20H10/6/2018 [90m10:56 AM[39m[49m  [5;40H11/4/2018 [90m7:10 PM[39m[49m [6;1H[37mbuild.sh[39m[39m[49m           [6;20H10/6/2018 [90m10:56 AM[39m[49m  [6;40H11/4/2018 [90m7:10 PM[39m[49m [7;1H[37mcibuild.cmd[39m[39m[49m        [7;20H10/6/2018 [90m10:56 AM[39m[49m  [7;40H10/6/2018 [90m10:56 AM[39m[49m[8;1H[37mcibuild.sh[39m[39m[49m         [8;20H10/6/2018 [90m10:56 AM[39m[49m  [8;40H10/6/2018 [90m10:56 AM[39m[49m[9;1H[37mNuGet.props[39m[39m[49m        [9;20H10/6/2018 [90m10:56 AM[39m[49m  [9;40H10/6/2018 [90m10:56 AM[39m[49m[10;1H[37mSignToolData.json[39m[39m[49m  [10;20H10/6/2018 [90m10:56 AM[39m[49m  [10;40H11/19/2018 [90m1:56 PM[39m[49m[11;1H[37mToolset.proj[39m[39m[49m       [11;20H10/6/2018 [90m10:56 AM[39m[49m  [11;40H10/6/2018 [90m10:56 AM[39m[49m[12;1H[37mVersions.props[39m[39m[49m     [12;20H10/6/2018 [90m10:56 AM[39m[49m  [12;40H11/19/2018 [90m1:56 PM[39m[49m

```

 In virtual terminal mode, the Windows console interprets these escape sequences into cursor movements and colors. As you can see in the first example above, virtual terminal mode enables the display of RGB colors and underlining that are not supported otherwise on Windows. Most Linux and Mac terminals support this form of rendering by default. 
 
 Both of these examples build the table structure by positioning the cursor for each cell and then writing the content. In a virtual terminal, this is done using ANSI escape sequences such as `[1;1H`. The equivalent `System.Console` call, which is needed in non-virtual terminals, looks like this: `Console.SetCursorPosition(0, 0)`. Meanwhile, the third example renders the layout using spaces and newlines, since there is no cursor when output is redirected.

 Providing a common API across these very different modes so that you don't have to write the code three times is a major goal of `System.CommandLine.Rendering`. The API is still very rough but you can explore these capabilities in the `RenderingPlayground` [sample](https://github.com/dotnet/command-line-api/tree/master/samples/RenderingPlayground).