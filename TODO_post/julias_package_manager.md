+++ 
date = "2021-11-11"
title = "Julia's Package Manager Pkg"
slug = "julias-package-manager-pkg" 
tags = []
categories = []
+++

{{< rawhtml >}}
<img src="/images/pkg.png" style="max-height: 200px;">

<p> 
{{< /rawhtml >}}

[Julia's](https://docs.julialang.org/en/v1/) package manager is called Pkg. Pkg creates and manages environments for julia projects, much like npm maintains an environment for each of its packages. Understanding how Pkg works is essential for being effective in julia. So let's dive in.

## The Big Picture

**Pkg prioritizes local project environments over global environments.**

But how does Pkg identify a project environment? What constitutes a project? There are two files that Pkg checks for: **Project.toml and Manifest.toml**.

{{< rawhtml >}}

<p style="font-size:110%; color:#4063D8; font-weight:600; padding:10px 0px;">A julia project is defined as a Project.toml and Manifest.toml pair.</p>
{{< /rawhtml >}}

Given a Project.toml and a Manifest.toml pair, you can `instantiate` the exact same environment using Pkg.

Pkg uses the julia environment variable `LOAD_PATH` to search for project environments (Project.toml and Manifest.toml pairs) to work in. In most situations, `LOAD_PATH` has three entries like so:

{{< rawhtml >}}
<img src="/images/load_path.png" style="max-height: 200px;">

<p> 
{{< /rawhtml >}}

The first is the current project's environment (which needs to be activated for Pkg to see it). The next is the global julia environment thats installed on the machine, `1.6` for example. In fact, if you look in` ~/.julia/environments/v1.6` you will find a Manifest.toml and Project.toml. And last are the standard libraries which come with julia when its installed.

{{< rawhtml >}}

<p style="font-size:110%; color:#4063D8; font-weight:600; padding:10px 0px;">Pkg starts with the first entry of LOAD_PATH, and will check subsequent entries in order until it finds what its looking for.</p>
{{< /rawhtml >}}

## Project.toml and Manifest.toml

So how does Pkg interact with the .toml files individually?

You can think of Project.toml as the **input** for the Pkg interpreter, and Manifest.toml as Pkg's **output**.

This is a typical julia project structure for your reference:

```sh
LICENSE
Manifest.toml
Project.toml
README.md
src/
    Project.jl
test/
    runtests.jl
```

### Project.toml

Project.toml describes the project on a high level.

```toml
name = "test"
uuid = "3b32e9fd-c20c-4cfa-a9b2-bfc1ff37ba3c"
authors = ["KatharineME <katharine.me@icloud.com>"]
version = "0.1.0"

[deps]
Revise = "295af30f-e4ad-537b-8983-00126c2a3abe"
Example = "7876af07-990d-54b4-ab0e-23690620f79a"

[compat]
BenchmarkTools = "1.2.0"
Dates = "3.1.20"
julia = "1.6"
```

The `name` field is the name of the project, the `uuid` field sets a universally unique identifier ([UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)) for the project, the `author` field is simply the name and email of the author, and the `version` of the project which follows [semantic versioning](https://semver.org).

The `deps` section lists the dependencies of the project. Notice the dependencies under `deps` dont include details like version number or the packages they depend on.

The `compat` section lists compatibility requirements: basically what has to be true for this package to run. For example `julia = "1.6"` is for the example project above to run. In the compat section you can also specifiy whether you will allow minor version updates or patch updates to the specificed packages. More documentation on the compat section [is here](https://pkgdocs.julialang.org/v1/compatibility/#Compatibility).

Side note: A julia _package_ is definded as a julia project with a `uuid` and `name` set in the Project.toml.

### Manifest.toml

Manifest.toml is an “absolute record of the state of the packages in the environment”. It includes exact information about direct and recursive dependencies.

```toml
# This file is machine-generated - editing it directly is not advised

[[ArgTools]]
uuid = "0dad84c5-d112-42e6-8d28-ef12dabb789f"

[[Artifacts]]
uuid = "56f22d72-fd6d-98f1-02f0-08ddc0907c33"

[[Base64]]
uuid = "2a0f44e3-6c83-55bd-87e4-b1978d98bd5f"

[[BenchmarkTools]]
deps = ["JSON", "Logging", "Printf", "Profile", "Statistics", "UUIDs"]
git-tree-sha1 = "61adeb0823084487000600ef8b1c00cc2474cd47"
uuid = "6e4b80f9-dd63-53aa-95a3-0cdb28fa8baf"
version = "1.2.0"

[[Dates]]
deps = ["Printf"]
uuid = "ade2ca70-3891-5945-98fb-dc099432e06a"
```

As you can see in the comment at the top, Manifest.toml is not intended to be manually edited, it is strictly Pkg's output. It is continuously being regenerated and updated as the envrionment of the project changes.

## Calling Pkg

Pkg has its own REPL that you can access from command line by starting julia and then hitting the `]` key, which will move you from the julia REPL into the pkg REPL.

{{< rawhtml >}}
<img src="/images/pkg_repl.png" style="max-height: 250px;">

<p> 
{{< /rawhtml >}}

In the pkg REPL you can enter commands that manage package environments like `add` to add a dependency. More on that in the API section below.

You can also call Pkg from the julia REPL like so:

```julia
using Pkg;

Pkg.add("Example")
```

## Pkg API

#### `generate`

Creates a boilerplate julia project in the current directory.

#### `build`

Runs the `deps/build.jl` script in the current package, then runs the build scripts for all the dependencies in depth-first recursive order.

#### `develop`

Make a package available for development.

{{< rawhtml >}}
<img src="/images/pkg_develop.png" style="max-height: 250px;">

<p> 
{{< /rawhtml >}}

#### `activate`

Activates a project's environment. For example, `activate .` activates the current directory's environment. This can also be done when starting julia in the project of interest like so: `julia --project`. This command needs to be run in a directory with Project.toml and Manifest.toml at the top level.

#### `instantiate`

If Manifest.toml exists in the active project, download all packages as described. If Manifest.toml doesn’t exist, get a set of feasible packages from Project.toml and install them, then create a manifest.toml.

#### `st` or `status`

Print out the status of the Project.toml or Manifest.toml. By default, the status of Project.toml will be printed. If `--manifest` is passed, the status of the Manifest.toml will be printed.

#### `add`

Add a package (a package that the current project will use and depend on). This command installs the package, updates the Project.toml `deps` section, and updates the Manifest.toml.

{{< rawhtml >}}
<img src="/images/pkg_add.png" style="max-height: 250px;">

<p> 
{{< /rawhtml >}}

#### `rm` or `remove`

Remove a package. This command uninstalls the package from the project environment and removes it from the Project.toml. If no other packages depend on it, it will be removed from the Manifest.toml. If other packages this project depends on use it, it will remain in the Manifest.toml. You can run `rm -m pkg_name` to remove the package and all packages that depend on it from the Manifest.toml.

#### `up` or `update`

Update packages according to the `deps` and `compat` sections of Project.toml. Passing `-m` like in `update -m` will update packages according to the Manifest.toml.

#### `resolve`

Updates the Manifest.toml to the current needs of the project according to Project.toml.

#### `test`

Runs the `test/runtest.jl` in the package directory.

#### `gc`

Garbage collect packages not used for significant time to free disk space.
