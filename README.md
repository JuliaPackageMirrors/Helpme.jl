# Helpme

[![Build Status](https://travis-ci.org/snotskie/Helpme.jl.svg)](https://travis-ci.org/snotskie/Helpme.jl)

Frustrating error messages getting you down? Wrap the code giving you problems in a call to the `@helpme` macro like so:
```julia
julia> Pkg.add("Helpme")
julia> using Helpme
julia> @helpme begin
       merge(["foo"=>"Hello World!"], ["bar"=>1337])
       end
INFO: Julia attempts to choose the proper type for Dicts when [brackets] are
used, and merge can be fussy when types don't match up. To force Julia to
assign the type Dict{Any,Any}, use {braces} in your variable definitions for
Dicts.
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
Helpme's database needs to be trained (which only takes about a second). I'll make sure each version has an appropriate pre-trained database set-up in `Helpme/src/database.jl` so that training only needs to be done on my machine.

However, if you want to retrain this database for any reason, it's possible by deleting `Helpme/src/database.jl`, opening the Julia REPL, and running `julia> using Helpme`.