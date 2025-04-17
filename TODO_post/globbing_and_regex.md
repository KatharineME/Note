+++ 
date = "2021-01-09"
title = "Globbing and Regex"
slug = "globbing-and-regex"
tags = []
categories = []
+++

This post is all about time saving tools. Globbing is for filename completion in shell and regex is for searching text.

## Globbing

The asterisk means "zero or more characters" in the file glob. So this command returns all files ending in `.md`.

```sh
ls *.md
```

This command returns all files starting with `A` and ending with `.md`.

```sh
ls A*.md
```

The question mark means one unknown character in the file glob. So this command will return `All.md` but not `About.md` or `A.md`.

```sh
ls A??.md
```

## Regex

{{< rawhtml >}}
<img src="/images/regex_trex.png" style="max-height: 300px;">
{{< /rawhtml >}}

`^` means "at the beginning of the line" and `$` means "at the end of the line". So this commmand returns lines beginning with "moose".

```sh
grep "^moose" file.txt
```

And this command returns lines ending with "mojo".

```sh
grep "mojo$" file.txt
```

This command returns lines beginning with "moose" and ending with "mojo". The `.*` means zero or more (`*`) of any character (`.`).

```sh
grep "^moose.*mojo$" file.txt
```

More:

| Regex Symbol  | Utility                                                                 |
| ------------- | ----------------------------------------------------------------------- |
| \             | negate special-ness of character after                                  |
| \s            | whitespace (a space, tab, or newline)                                   |
| ?             | one or more of the character that precedes it                           |
| .\*           | zero or more of any characters                                          |
| vertical line | or                                                                      |
| []            | within a range, for example ^[A-Z] to search line starting with capital |
| /i            | makes regex before it case insensitive                                  |

## grep and fgrep Come to Play

`grep` is a command line tool for searching text. `grep` never uses globbing. `grep` uses regex. However, with a command like this one:

```sh
grep file* README.md
```

the shell will do globbing before passing the command to grep. The shell will find `filename.txt` in the current directory (assuming it exists) and then grep will look for "filename.txt" in the README.md. However, adding quotes like this:

```sh
grep "file*" README.md
```

will prevent the shell from globbing and then `file*` will be interpretted as regex and grep will look for "fil" ending with zero or more "e" in README.md.

`fgrep` or `grep -F` on the other hand allows you to search for exactly what you type. For example, this command:

```sh
grep -F "$" file.txt
```

will return all the lines with the "$" character in file.txt.
