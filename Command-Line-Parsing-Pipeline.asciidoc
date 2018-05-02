The process of parsing a command line is as follows:
string --[lex]--> 
     tokens --[parse 1 / structural validation (e.g. argument arity, set inclusion) ]--> 
          symbols --[parse 2 / value validation (e.g. type correctness, validation of strong-typed result) ]--> 
               ParsedSymbols with strongly-typed args

