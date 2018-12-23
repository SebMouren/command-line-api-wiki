# History

This effort has its roots in the building of the `dotnet` CLI, which put significant demands on its parser. While there were a number of .NET command line parsing libraries available, including more than a couple built at Microsoft, none of them had the capabilities we needed, and some of these capabilities had architectural implications that would have made  

With the arrival global tools, we anticipated people writing more console applications. We decided to write a new console library that built on some of the ideas in the `dotnet` CLI's parser and added a number of new features.
