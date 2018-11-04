# dotnet-suggest

Command line apps built using `System.CommandLine` have built-in support for tab completion. It just has to be hooked up to your shell. 

On the machine where you'd like to enable completion, you'll need to do two things.

1. Install the `dotnet-suggest` global tool by running the following command:

```shell
> dotnet tool install dotnet-suggest -g --add-source https://dotnet.myget.org/F/system-commandline/api/v3/index.json --version 1.*
```

2. Add the appropriate shim script to your shell profile, which redirects shell completion for `System.CommandLine`-based apps to the `dotnet-suggest` tool. 

For bash, add [dotnet-suggest-shim.bash](https://github.com/dotnet/System.CommandLine/blob/master/src/System.CommandLine.Suggest/dotnet-suggest-shim.bash) to `~/.bash_profile`.

For PowerShell, use [dotnet-suggest-shim.ps1](https://github.com/dotnet/System.CommandLine/blob/master/src/System.CommandLine.Suggest/dotnet-suggest-shim.ps1) to  `C:\Users\<USERNAME>\Documents\PowerShell\Microsoft.PowerShell_profile.ps1`.

(For other shells, please look for or open an [issue](https://github.com/dotnet/System.CommandLine/issues).)

