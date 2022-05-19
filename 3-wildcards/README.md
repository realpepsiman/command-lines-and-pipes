# Wildcards

If you want to do something to multiple files--say move all of them--it is tedious if you have to spell all the file names out explicitly. Instead, you want to specify a pattern that selects the files you want to work with. There are multiple ways of doing this, but the UNIX way, sometimes called ["glob"](https://en.wikipedia.org/wiki/Glob_(programming)), looks something like the below. It is up to the shell how to do this, so it can (and will) vary with the shell you are using, but there will always be something similar to this.

## Matching "everything": *

A `*` on the command-line will match any file name. (If there are no files, shells behave differently, but all shells I know about will match all files if there are any).

In this directory, try the command

```sh
$ ls *
```

The `*` should match everything, so the result is the same as just running `ls` without arguments, but only because `ls` will list everything by default. If you don't belive me, try running `wc` without arguments and with the `*` argument.

```sh
$ wc
```

(will read from `stdin` so use Ctrl-D to stop it).

```sh
$ wc *
(... output omitted ...)
```

(will count lines, words, and characters in all the files. All files, except `README.md`, because we don't care about their content here, but `wc` will still count).

You will use a `*` on its own when you want to do something with all files in the current directry, for example move them. More often, though, you combine `*` with a partial file name, and if you do that, you will match all files that match the partial name. For example, if you use `ls *.sam` you will see all the SAM files in this directory, or if you use `wc *.fa` you will word count all the FASTA files. (Yes they are all empty, but the point is that you will select those files).

Use this pattern to select all files that are `fish` related. Then use it to select all that are `sharks` related.

You can use multiple `*`s in a pattern and you can use multiple partial file names. The pattern `fish-*.sam`, for example, will pick all fish SAM files.

You already know how to make directories, so make the following structure:

```
fish/
     sam/
     fa/
sharks/
     sam/
     fa/
```

then use `mv` and pattern matching to move all the `fish` SAM files to `fish/sam`, all the `fish` FASTA files to `fish/fa` and the same for the `sharks` files.

## Matching a single character, `?`

The `?` symbol will match a single character. This isn't that powerful, but it does occationally has its usage. For example, go into the `fish/sam` directory and write 

```sh
$ ls fish-?.sam
```

It will pick the `fish` `.sam` files numbered from zero to nine, but not the rest, because the `?` matches a single and not two characters. Likewise, `fish-??.sam` will pick the files numbered 10 to 19.


There are usually a few more glob-patterns in a shell, but the shells are far from standardised, so you will have to check the documentation for the specific shell you are using to learn more about those.
