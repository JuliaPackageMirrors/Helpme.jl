# Helpme

[![Build Status](https://travis-ci.org/snotskie/Helpme.jl.svg)](https://travis-ci.org/snotskie/Helpme.jl)

Frustrating error messages getting you down? Wrap the code giving you problems in a call to the `@helpme` macro like so:
```julia
julia> Pkg.clone("https://github.com/snotskie/Helpme.jl")
julia> using Helpme
julia> @helpme begin
       merge(["foo"=>"Hello World!"], ["bar"=>1337])
       end
INFO: Julia attempts to choose the proper type for Dicts when [brackets] are
used, and merge can be fussy when types don't match up. To force Julia to
assign the type Dict{Any,Any}, use {braces} in your variable definitions for
Dicts.
INFO: Strings are concatenated with the * operator, not the + operator.
INFO: Type parameters in Julia are invariant, meaning although Int <: Number is
true, Array{Int} <: Array{Number} is false. This can be annoying when defining
functions. Instead of "f(Array{Number})=..." try "f{T<:Number}(Array{T})=..."
ERROR: no method convert(Type{ASCIIString}, Int64)
 in setindex! at dict.jl:521
 in merge! at dict.jl:78
 in anonymous at /home/snotskie/.julia/v0.3/Helpme/src/Helpme.jl:2

julia> merge({"foo"=>"Hello World!"}, {"bar"=>1337})
       {"foo"=>"Hello World!", "bar"=>1337}
```

Helpme will attempt to find a suggestion based on the exception raised and the code passed. If one is found, it will print a friendly message to INFO.

## Todo

In no particular order:

* Examples, Examples, Examples. To be any helpful, Helpme will need a database of all the mistakes Julia-coders tend to run into. At present, it only has thirty-one examples to take suggestions from.
* (DONE) Better Distance Calculations. To determine the "closest" example error and code to what's passed to it, `@helpme` needs to know what determines "close"--that is, it needs a distance function. At present, because I already had the code laying around, it uses a Levenshtein distance on their string representations. I hope to use something a bit more accurate in the future.