# Welcome to the System.CommandLine wiki!

## Our goal

_Give your users a great experience with your .NET command line applications while letting you focus on the great application you're writing._

or more simply

_Leave the plumbing to us._

To do this, System.CommandLine contains three pillars of functionality:

1. _Parsing._ Parse command line input, validate it, and bind it to types beyond just strings.
2. _Middleware._ A composable pipeline to provide features like suggestions, help, exception handling, and parse debugging.
3. _Rendering._ Write code once that can render to multiple outputs (virtual terminal, Windows Console, and files)

## Parsing

System.CommandLine is the base API which can support multiple application models to simplify working with the parser. You can also work directly with the the System.CommandLine API. 

![appmodels](https://user-images.githubusercontent.com/5844318/47970907-aed10080-e048-11e8-83dd-7e9dcd01c885.png)

The simplest way to create your parser is with the [DragonFruit app model](DragonFruit-overview). This works well if you have a single layer of commands (no subcommands). This approach adds a target to the build process. 

If you have a complex API, or you want to work with the API directly to better understand it, you can access the [System.CommandLine API](Using-The-API) directly. 

We plan to add application models to fit niches between DragonFruit and direct API access in the future.
