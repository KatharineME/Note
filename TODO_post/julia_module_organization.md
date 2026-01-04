+++
date = "2021-07-26"
title = "Julia Module Organization"
slug = "julia-module-organization"
tags = []
categories = []
+++

## What is a Module?

A module is a self contained unit of code with a separate global scope namespace. It can only see what is inside itself. A module is defined with `module NameOfModule ... end`. A module can contain files and other modules.

## include vs. using

`include` and `using` are two ways of accessing code in Julia. `using` tells Julia to reference something that has already been defined, while `include` tells Julia to copy and define code that was not previously defined. `include` should only be run once. Thereafter `using` should be used to access it.

## Example Module

This is the Kate.jl file that defines the Kate module. Everytime there is an include statement, you can imagine the code in that file being copied and pasted into this Kate.jl file.

Modules A and B have no knowledge of eachother, nor do they know they belong to module Kate. Whereas module Kate knows modules A and B and each of their files.

```julia

module Kate

module A

include(f1.jl)
include(f2.jl)
include(f3.jl)

end

module B

include(f4.jl)
include(f5.jl)

end

include(f6.jl)

end

```

The f1.jl file for example looks like this. In order for Kate module to see the `f1` function, it must be exported at the end of the file. If there are multiple methods for one functions, all will be exported with a single export statement.

```julia

function f1(x)

    return(x -5)

end

export f1
```

Now in a Jupyter notebook I'll be able to run the following:

```jupyter

using Kate

Kate.A.f1()

Kate.B.f5()

Kate.f6()

```

## Dependencies Within a Module

### A function in one module calls another function in the same module

f2 calls f3. f3 can be called directly in f2 becuase they are within the same module.

```julia

function f2(x)

    return f3(x + 2)

end

```

### A function in one module calls a function in another module

f4 calls f1. `using ..A: f1` tells julia to reference the previously defined f1 function from the A module. The two dots `..` are used to go one level up from the current module.

```julia

using ..A: f1

function f4(x)

    return f1(x)

end

```

## Order Matters When Defining a Module

In order for f4 to use f1, module A must be defined before module B in Kate.jl. If the their order was reversed, julia would throw an error when it tried to call `using ..A: f1` because module A would not have been defined yet. Here is another illustration of why order matters in a module file like Kate.jl. This module file will throw an error because when `a = b` is evaluated, `b` has not been defined.

```julia

module Kate

a = b

b = 5

end
```

However, this module file works. It doesnt matter that function `a` calls `b`and `a` comes before `b` becuase they are not being executed, they are merely being defined.

```julia

module Kate

function a()

    return b(5)

end

function b(x)

    return(x +5)

end

end
```
