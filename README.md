# Helpme

[![Build Status](https://travis-ci.org/snotskie/Helpme.jl.svg)](https://travis-ci.org/snotskie/Helpme.jl)

Frustrating error messages getting you down? Wrap the code giving you problems in a call to the `@helpme` macro like so:
```julia
julia> @helpme begin
       merge(["foo"=>"Hello World!"], ["bar"=>1337])
       end
INFO: Julia attempts to choose the proper type for Dicts when [brackets] are used,
and merge can be fussy when types don't match up. To force Julia to assign the type
Dict{Any,Any}, use {braces} in your variable definitions for Dicts.
ERROR: no method convert(Type{ASCIIString}, Int64)
 in setindex! at dict.jl:521
 in merge! at dict.jl:78
 in merge at dict.jl:83

julia> merge({"foo"=>"Hello World!"}, {"bar"=>1337})
       {"foo"=>"Hello World!", "bar"=>1337}
```

Helpme will attempt to find a suggestion based on the exception raised and the code passed. If one is found, it will print a friendly message using to INFO.

## Todo

In no particular order:

* Examples, Examples, Examples. To be any helpful, Helpme will need a database of all the mistakes Julia-coders tend to run into. At present, it only has thirteen examples to take suggestions from.
* Better Distance Calculations. To determine the "closest" example error and code to what's passed to it, `@helpme` needs to know what determines "close"--that is, it needs a distance function. At present, because I already had the code laying around, it uses a Levenshtein distance on their string representations. I hope to use something a bit more accurate in the future.