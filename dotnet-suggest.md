# dotnet-suggest

Command line apps built using `System.CommandLine` have built-in support for tab completion. 

![suggest](https://user-images.githubusercontent.com/547415/48972576-1217d800-efe2-11e8-8653-8063ddc6864f.gif)

On the machine where you'd like to enable completion, you'll need to do two things.

1. Install the `dotnet-suggest` global tool by running the following command:

```console
> dotnet tool install dotnet-suggest -g --add-source https://dotnet.myget.org/F/system-commandline/api/v3/index.json --version 1.*
```

2. Add the appropriate shim script to your shell profile, which sends completion requests from your shell to the `dotnet-suggest` tool, which delegates to the appropriate `System.CommandLine`-based app.

For bash, add [dotnet-suggest-shim.bash](https://github.com/dotnet/System.CommandLine/blob/master/src/System.CommandLine.Suggest/dotnet-suggest-shim.bash) to `~/.bash_profile`.

For PowerShell, add [dotnet-suggest-shim.ps1](https://github.com/dotnet/System.CommandLine/blob/master/src/System.CommandLine.Suggest/dotnet-suggest-shim.ps1) to  `C:\Users\<USERNAME>\Documents\PowerShell\Microsoft.PowerShell_profile.ps1`.

(For other shells, please [look for](https://github.com/dotnet/System.CommandLine/issues?q=is%3Aissue+is%3Aopen+label%3A%22shell+suggestion%22) or open an [issue](https://github.com/dotnet/System.CommandLine/issues).)
