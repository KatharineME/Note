+++ 
date = "2021-01-28"
title = "Julia"
slug = "julia"
tags = []
categories = []
+++

![julia](/images/julia_banner.png)

{{< rawhtml >}}

<p style="font-size:110%; color: #6f439c; margin: 0; font-style: italic; padding-top:2%;">
The words that we have available change what we will do.
</p>
{{< /rawhtml >}}

## Why Learn Julia? What Are the Value Propositions?

- **Combines Python's usability with C++ speed**
  - Writing a prototype of code in Python, only to later implement it in C++ for speed is a waste.
  - Now these strengths are in one language.
  - This is accomplished using just in time compilation. The way this works is: you write dynamic Julia code, then Julia compiles it into static Julia, then everything runs super fast.
- **Dynamic**
  - Meaning you don't have to define data types, Julia will do that for you.
  - However you have the option of defining them if you want.
- **Readable**
  - Designed to be easy to understand.
- **Flexible**
  - There are optional arugments in functions.
  - Functions can be combined.
  - There is a rich language of types for contructing and describing objects.
- **Multiple Dispatch**
  - Where a function or method can be dynamically dispatched at runtime based on the type of the object the function is being called on, or attributes of one or more of its arguments. An example would be a function whose arguments can be a either integers or strings, and where multiple arguments are string values, the function is dispatched in a certain way.
  - Single dispatch, on the other hand, is when the way that a function is dispatched at runtime is determined by a single data type, or a single function argument. This _special_ argument is highlighted syntactically in some languages.
- **Asynchronous I/O**
  - This simply means processes are allowed to continue running before data transmission is finished.
  - This is contrasted with Synchronous I/O or Blocking I/O where processes need to wait for data transfers to finish before they can continue.
- **Developed for parallel processing**
- **No dependencies on other languages**
  - However, Julia can call other languages easily.
- **Includes math and data science symbols**
  - So an equation can become a line of code.
- **Focus on scientific computing**

## Weaknesses

- **Just in time compilation**
  - It is the reason Julia is easy to use and also lightening fast. So its also a plus.
  - When you run something for the first time it will take surprisingly long to run. This is because all the Julia code you're calling (including julia packages that you called / included) get compiled into a static Julia (like C++ which is static from the start).
  - However, after the compilation is done, it is super fast.
  - The work-around for this is to keep your Julia session running for as long as possible.
- **Community and packages are still maturing**
  - Being a young language, we expect this.
- **Often not backwards compatible**
  - Julia is developing rapidly, and as a consequence, a solution that was posted on Stack Overflow six months ago may already by deprecated, forcing you to actually read the documentation 😝.
- **Code errors can be difficult to interpret**

## Julia in Jupyter

Jupyter Lab is where I develop Julia packages. With the right settings, this works great. Some people complain about the slowness of Julia's JIT compiling. But once you understand how it works, you can organize your notebook to maximize your session length and enjoy the speedy compiled Julia code.

First, you'll need [IJulia](https://github.com/JuliaLang/IJulia.jl), which is the Julia kernal for Jupyter. Once its installed, you'll be able to create a Julia notebook, just like you would create a Python notebook. From the Julia REPL, enter `]` to access pkg mode, then:

```julia
add IJulia
```

Once its running, I would add these lines at the top of every Julia notebook you work in:

```julia
using JuliDB

using IndexedTables

# For viewing DataFrames

ENV["COLUMNS"]=100

ENV["LINES"]=200

# For viewing JuliaDB tables and arrays

IndexedTables.set_show_compact!(false)
```

By default, only summaries of tables and dataframes are shown, running these commands will allow full data to be printed.

To run a shell command in a Julia Jupyter notebook (or in the Julia REPL), use the `;` key binding.

```julia
;pwd

/Users/kate/github/Project/notebook
```

## Basics For Getting Started

#### Using Modules

In most cases, you'll need to use a few Julia packages, also known as modules. JuliaDB.jl is one such module. To get started using the module, you first need to add it to your environment by entering the Julia REPL and pressing `]` to enter pkg mode, then:

```julia
add JuliaDB
```

Once you're back in the Julia REPL, or in a notebook, and you want to use the module you can either enter `import JuliaDB` or `using JuliaDB`. If you `import` the package, you're only importing the module's name, to call its function you would need to enter `JuliaDB.loadtable()` for instance. But if you use `using` than the modules name and all of its function names are imported so you could call `loadtable()` directly.

#### Changing strings to ints

```julia
parse.(Int, ["1", "2"])

2-element Array{Int64,1}:
 1
 2
```

## Symbols

**@ Macro**

- A macro takes in code (a julia expression) as input and spits out code (a different julia expression). So, a macro is a code generator.
- My favorite macro is `@time` which wil print how long a process took, how much memory was used, and percent gc if applicable.

```julia
@time A = Array{Float64,2}(undef, 2, 3)

  0.000001 seconds (1 allocation: 128 bytes)

2×3 Array{Float64,2}:
 2.23877e-314  0.0    0.0
 2.23832e-314  0.0  NaN
```

**!**

- It means "not" is Julia. For example `!=` codes for "not equal to."
- A function name ending with a `!` indicates that it will mutate or destroy the value of one or more of its arguments (compare, for example, `sort` and `sort!`)

**;**

- The semicolon has multiple uses in Julia.
- First, it activates shell mode in the Julia REPL. `;pwd` for example will print the working directory.
- Next, when the semicolon is placed at the end of a piece of code, it stops Julia from printing variables defined in that code. However if a function is called in that piece of code, the output of the function will still be printed. The semilon at the end of this code prevents the filtered indexedtable from being printed, however the time function still prints its output.

```julia
@time filter(i -> (i.CHROM == benchmark_variant[1]), vcf_table);

0.102539 seconds (81.61 k allocations: 4.294 MiB)
```

## Data Types

Julia has lots of datatypes, and yet if you dont specify a data type, Julia will do that for you. Having a solid understanding of the basic data types will save you lots of time and headaches.

#### Tuple

Built in data structure with **fixed-length** that can hold **any values** but cannot be changed (**immutable**)

```julia
julia > x = (2, "soy sauce", "mirin")

(2, "soy sauce", "mirin")
```

```julia
julia > x[2]

"soy sauce"
```

#### Named Tuple

Elements in tuple can be given names. If so, then its a named tuple. Allowd you to access via names.

```julia
julia > x = (protein="beef", carb="rice")

(protein = "beef", carb = "rice")
```

```julia
julia > x[:protein]

"beef"
```

### Core Essentials

**Core.Array**: N-dimensional dense array with elements of type T

```julia
julia > Array{T, N}
```

### Base Essentials

**Base.vcat**: concatenate along 1 dimension

```julia
julia > a = [1 2 3]
julia > b = [4 5 6; 7 8 9]
julia > vcat(a,b)

3×3 Array{Int64,2}:
 1  2  3
 4  5  6
 7  8  9
```
