System.CommandLine provides a set of default features that are useful to people using it to develop apps as well as to the users of those apps. This is a quick overview of those features, using the app we created in 
[Your first app with System.CommandLine](Your-first-app-with-System.CommandLine) or [Your first app with System.CommandLine.DragonFruit](Your-first-app-with-System.CommandLine.DragonFruit) (which are functionally equivalent). 

### Get help

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

Users might be used to different prefixes in different ecosystems, especially if your app targets .NET Core and is used cross-platform, so apps build on System.CommandLine understand many different ways of requesting help. The following are all equivalent:

```console
> myapp -h
> myapp /h
> myapp --help
> myapp -?
> myapp /?
```

### Version option

Providing a way to check the version of your app is helpful to your users.

System.CommandLine provides this by default. In the [help](Features-overview.md#get-help) example you might have noticed an option, `--version`, that was not explicitly configured in the sample code. When you run your program with this option, you'll see something like this:

```console
> myapp --version
1.0.0
```

