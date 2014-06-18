# Helpme

[![Build Status](https://travis-ci.org/snotskie/Helpme.jl.svg)](https://travis-ci.org/snotskie/Helpme.jl)

Frustrating error messages getting you down? Wrap the code giving you problems in a call to the `@helpme` macro like so:
```julia
julia> @helpme begin
       merge(["foo"=>"Hello World!"], ["bar"=>1337])
       end
INFO: Julia attempts to choose the proper type for Dicts when [brackets] are used, and merge can be fussy when types don't match up. To force Julia to assign the type Dict{Any,Any}, use {braces} in your variable definitions for Dicts.
ERROR: no method convert(Type{ASCIIString}, Int64)
 in setindex! at dict.jl:521
 in merge! at dict.jl:78
 in merge at dict.jl:83

julia> merge({"foo"=>"Hello World!"}, {"bar"=>1337})
       {"foo"=>"Hello World!", "bar"=>1337}
```

Helpme will attempt to find a suggestion based on the exception raised and the code passed. If one is found, it will print a friendly message using to INFO.