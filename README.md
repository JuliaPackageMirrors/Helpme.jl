# Helpme

[![Build Status](https://travis-ci.org/snotskie/Helpme.jl.svg)](https://travis-ci.org/snotskie/Helpme.jl)

Frustrating error messages getting you down? Wrap the code giving you problems in a call to the `@helpme` macro like so:
```julia
julia> Pkg.add("Helpme")
julia> using Helpme
julia> @helpme begin
       merge(["foo"=>"Hello World!"], ["bar"=>1337])
       end
INFO: Julia attempts to choose the proper type for Dicts when [...] is used, and functions like merge(collection, others...) can be fussy when types don't match up. To force Julia to assign the type Dict{Any,Any}, use {...}. More info at <http://bit.ly/1nUDhyc>.
```

Helpme will attempt to find a suggestion based on the exception raised and the code passed. If one is found, it will print a friendly message to INFO.

# Usage
When `@helpme` is passed an expression, it executes it, catching any errors raised. It then breaks these errors and the expression down into a list of features. Finally, it searches through an in-memory database with similar sets of features to determine which suggestions to provide.

It's important to know a few tips to get the most out of this package.

## Expression Size
Expressions that are very large can confuse the feature-search. The best way to use `@helpme` is to pass it a smallish expression that raises the error you are getting.

## Syntax Errors
If you are trying to figure out a syntax error, `@helpme` can't help normally because Julia will raise the error before the macro has the chance to examine it. To work around this, wrap your troublesome code up like so:
```julia
julia> @helpme eval(parse("a .=== b"))
```

Note that there are serious issues with using `eval`: [it always uses global scope](https://github.com/JuliaLang/julia/issues/2386). Take care with this workaround.

## Retraining
Helpme's database needs to be trained when it first loads (which only takes a few seconds). Furthermore, because each verson of Julia may have different syntax trees and/or error messages, each requires its own database file.

I'll attempt to include an appropriate pre-trained database for each Julia version in `Helpme/src/database.VERSION.jl` so that training only needs to be done on my machine.

However, if you want to retrain your database manually for any reason, delete `Helpme/src/database.VERSION.jl`, open the Julia REPL, and run `julia> using Helpme`.