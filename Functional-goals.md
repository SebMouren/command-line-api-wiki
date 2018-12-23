# Functional goals

1. _Parsing._ The parse operation in `System.CommandLine` produces a `ParseResult` that can be used independently of any invocation. This was important for being able to test your parsing in isolation. We also provide a tokenizer to mimic what happens before `Main(string[])` is called, so you can test using a `string` rather than a `string[]` as parser input. The parse model also supports altering and re-parsing input. This enabled features such as [suggestions](Features-overview#Suggestions) and [directives](Command-line-syntax.md#directives).

2. _Middleware._ We wanted to provide features such as suggestions, help, parse analysis, exception handling, and parse debugging, and we wanted these features to be extensible while remaining separate from your app's core logic. To enable these things, `System.CommandLine` uses a composable chain of responsibility in between the parsing operation and the invocation. 

3. _Tab completion._ Tab completion is usually implemented using shell scripts that are crafted per app and per shell. We wanted a model that allowed your app, rather than a script, to be the source of truth, for suggestions to be extensible, and for adding support for additional shells to be easy.

4. _Rendering._ Rendering output such as table layouts and animations is complicated, especially if you intend for that output to look correct regardless of whether you're writing to a non-ANSI terminal, an ANSI terminal, or when output is redirected. (This is the newest part of the project. It will be split into a separate assembly and the API is still very unstable.)


