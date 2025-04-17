+++ 
date = "2021-02-19"
title = "JuliaDB"
slug = "juliadb"
tags = []
categories = []
+++

{{< rawhtml >}}
<img src="/images/julia_db_logo.png" style="max-height: 400px;">
{{< /rawhtml >}}

## Key Features

- Has two main data structures: IndexedTable and NDSparse.
- Can work with data that is larger than the machine RAM.
- Allows parallel processing with the `addprocs` function.
- Works with Dagger.jl's `save` and `load` functions to make a sort of index file adjacent to your table which allows for fast loading and access.

### Table

- "Table" refers to both an IndexedTable and NDSparse
- Tables store data in columns
- Tables are typed, meaning changing a table requires returning a new table
- JuliaDB has few mutating operations because a new table is necessary in most cases

{{< rawhtml >}}
<br>
{{< /rawhtml >}}

#### Indexed Table

- Basically a named tuple of vectors which **behaves** like a vector of named tuples
- IndexedTable can be sorted by any number of primary keys (defined using parameter `pkey`). The table will be sorted by the first priamry key, then the second, and so on.
- Iterates over NamedTuples of **rows**
- Use `table` function to create or `loadtable` to load existing data
- `loadtable` should only be used once, then the `save` function should be used to save the table in bianry format to then be quickly loaded using `load` in the next session. The `save` and `load` functions are Dagger.jl fucntions.

```julia
x = 1:6
y = vcat(fill('a', 3), fill('b', 3))
z = randn(6);

t = table(x=x, y=y, z=z); pkey = [:x, :y])

Table with 6 rows, 3 columns:
x  y    z
─────────────────
1  'a'  1.06936
2  'a'  -1.29235
3  'a'  -0.725817
4  'b'  -0.422334
5  'b'  0.0246361
6  'b'  0.669536
```

```julia
t[1]

(x = 1, y = 'a', z = 1.069356265804105)
```

```julia
t[1].y

'a'
```

```julia
for item in t
    println(item)
end

(x = 1, y = 'a', z = -0.5856775236297086)
(x = 2, y = 'a', z = -1.8225049388821863)
(x = 3, y = 'a', z = 0.2538693543229162)
(x = 4, y = 'b', z = 3.0769831992975276)
(x = 5, y = 'b', z = 1.0156881097767552)
(x = 6, y = 'b', z = 0.7371713978290413)
```

### NDSparse

- Behaves like a sparse array with arbitrary indices
- The keys of an NDSparse array are sorted
- Use `loadndsparse` to load existing data
- Its made for working with data that is sparse over the domain of the index (stock data)
- Iterates over NamedTuples of **values**

```julia
nd = ndsparse((x=x, y=y), (z=z,))

2-d NDSparse with 6 values (1 field named tuples):
x  y   │ z
───────┼──────────
1  'a' │ 1.0548
2  'a' │ 1.64493
3  'a' │ -0.738508
4  'b' │ 0.325126
5  'b' │ 0.299526
6  'b' │ 0.787615
```

```julia
nd[1, 'a']

(z = 1.0548018299672375,)
```

```julia
nd[:, 'a']

1-d NDSparse with 3 values (1 field named tuples):
x │ z
──┼──────────
1 │ 1.0548
2 │ 1.64493
3 │ -0.738508
```

```julia
for item in nd
    println(item)
end

(z = 1.0548018299672375,)
(z = 1.6449349801308328,)
(z = -0.7385076399317578,)
(z = 0.3251263120611191,)
(z = 0.2995262607206383,)
(z = 0.7876150538404173,)
```

### When To Use a Table or NDSparse?

- NDSparse cases
  - Stocks. First two columns are stock name and date. You will often want to know the closing price of a particular stock on a particular day. In a table, you would need to query where the apple stock has that particular date. In an NDSparse, it is just getting the index with apple and that date.

## API

#### select

Selects particular data and optionally applies function on it.

```julia
select(t, (:x, :z))

Table with 6 rows, 2 columns:
x  z
─────────────
1  1.37942
2  0.525809
3  0.754992
4  -0.0998402
5  -1.43908
6  1.96262
```

```julia
select(t, (:x, :z) => row -> row.x > row.z)

6-element Array{Bool,1}:
 0
 1
 1
 1
 1
 1
```

#### reduce

Applies a function pairwise. Its good for functions with the associative property, meaning order doesnt matter. For a table that is four rows long, reduce(+, t) is the same as:

out = +(row1, row2)

out = +(out, row3)

out = +(out, row4)

Which will get the sum of the four rows.

[More on JuliaDB API.](https://juliadb.juliadata.org/latest/api/)

#### map

Map function. `map` and `select` have overlapping functionality.

```julia
map(row -> row.x > row.z, t)

6-element Array{Bool,1}:
 0
 1
 1
 1
 1
 1
```

#### columns

Gives you a NamedTuple of Vectors where each name is the column name and each vector is the column data.

```julia
columns(t)

(x = [1, 2, 3, 4, 5, 6], y = ['a', 'a', 'a', 'b', 'b', 'b'], z = [1.5812131489064223, -0.5545552318128536, -0.5490833373597503, 0.291760065744581, -1.411530524868723, -1.449557749618286])
```

#### rows

Gvies you a StructArray which is basically a Vector of NamedTuples. Each row is one NamedTuple.

```julia
rows(t)[1]

(x = 1, y = 'a', z = 1.5812131489064223)
```

#### summarize

Applies a function (or functions) column-wise.

`summarize(function, table, by: select)`

#### addprocs

Adds as many workers as there are CPU cores available. `addprocs(2)` would add two working processes. When there are multiple processes, tables will be loaded as distributed tables across all the workers. So oyu cant index a row based on row number or iterate over all rows.

### Statistics

JuliaDB integrates with OnlineStats (a julia stats package) using the functions `reduce` and `groupreduce`.

```julia
julia > using JuliaDB, OnlineStats
julia > t = table(1:100, rand(Bool, 100), randn(100));
julia > reduce(Mean(), t; select = 3)

Mean: n=100 | value=0.159962
```

### General Wisdom

- Be as specific as possible when selecting to minimize the amount of data you are passing around.
- After loading a table for the first time, use Dagger.jl's `save` and `load` functions to work with the table.
