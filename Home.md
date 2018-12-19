# Welcome to the System.CommandLine wiki!

## Our goal

_Give your users a great experience with your .NET command line applications while letting you focus on the great application you're writing._

To do this, `System.CommandLine` contains three pillars of functionality:

1. _Parsing._ Parse command line input, validate it, and bind it to types beyond just strings.
2. _Middleware._ A composable pipeline to provide features like suggestions, help, exception handling, and parse debugging.
3. _Rendering._ Write code once that can render to multiple outputs (virtual terminal, Windows Console, and files or other redirected streams).

## Parsing

`System.CommandLine` contains the base API which can support various application models to simplify working with the parser. You can also work directly with the the `System.CommandLine` API. It contains the core functionality for parsing, binding input to your model, invoking your application logic, and providing shell completions based on your application's syntax.

![layers](https://user-images.githubusercontent.com/547415/50188724-5fcceb00-02d7-11e9-9c33-4b91d70f963e.png)

The simplest way to create your parser is with the [DragonFruit app model](DragonFruit-overview). This works well if you have a single layer of commands (no subcommands).

