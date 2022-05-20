# Searching and sorting

In this directory I've put some files (you can use `ls` to see which) and in *some* of those files you will the world `ctib`. As you know, the command `grep` can find which ones.

Try

```sh
$ grep ctib *.txt
```

This should give you the lines in the `*.txt` files that contain `ctib`. Two things, though, I only want the file names and not the lines, but reading `man grep` tells me that there is an option for getting this.

```sh
$ grep -l ctib *.txt
qax.txt
```

That is a poor result when I was expecting more than one file containing `ctib`, but then, I am only searching for lower-case `ctib`. Maybe I should ignore case? Use `man grep` to find out which flag you need to set for this. When `man` opens, you can type `/` to start a search, and then type `case` to get to the first occurrence of `case`. If you are lucky, that will tell you what you need to do. If not, you can use `n` to go to the next occurrence.

Once you are done, use `grep -lX ctib *.txt` where `X` is the option you found. I get `bar.txt`, `foo.txt`, and `qax.txt`.

Imagine there were lots and lots of these. We might not want all of them printed to the terminal, but only the largest files (the bigger the better when it comes to our `CTiB`).

The `ls` command can sort files according to size.

```sh
$ ls -sS *.txt
72 qux.txt
48 bar.txt
16 foo.txt
 8 baz.txt
 8 qax.txt
```

The `-s` option prints the size (in a unit that isn't necessarily meaningful to us but captures the file size) and `-S` sorts the output in decreasing size.

We want to combine the `grep` command where we find the files of interest with the `ls` command to get the files sorted with respect to size. Time to write our first pipeline!

This seems like the obvious way to do it:

```sh
$ grep -il ctib *.txt | ls -sS
```

We get the files and we pipe them into `ls`. Unfortuantely `ls` is one of the few commands that do not read information from its standard input. We need to give it the result of `grep` as arguments.

The command `xargs` can translate its standard input to arguments, and if we do

```sh
$ grep -il ctib *.txt | xargs ls -sS
48 bar.txt	16 foo.txt	 8 qax.txt
```

we get what we want. The ouput of `grep` is the three files, `xargs` takes this output and put add them to the `ls -sS` command so we get `ls -sS foo.txt bar.txt qax.txt` and then we get the desired result.

However, I am not interested in sorting the files by "block size", I want them sorted by number of lines. The `wc -l` command can give me the number of lines in a file:

```sh
$ wc -l *.txt
     785 bar.txt
     127 baz.txt
     127 foo.txt
      18 qax.txt
     813 qux.txt
    1870 total
```

Use `xargs` to only get the counts for the files that contain `ctib`.

The result should be something like this:

```
     785 bar.txt
     127 foo.txt
      18 qax.txt
     930 total
```

The last line is not a file, and we want to get rid of it. Here, unfortunatly, the most intuitive way to get that is not portable to all platforms. You might be on a platform where `head -n -1` includes everything except the last line, but that requires the GNU `head` implementation. A portable solution that is not at all intuitive is `sed \$d` for removing the last line. The `sed` command (stream editor) does a lot of stuff, but it is too complicated to cover here. Just use

```sh
$ ≪ your | pipeline ≫ | sed \$d
     785 bar.txt
     127 foo.txt
      18 qax.txt
```

Here, we are lucky that the files are already sorted with size, but that need not be the case. Add a `sort` to the pipeline to sort them. Remember that `sort` sorts lexicographically while `sort -n` will sort numerically.

Sorting according to file size might not be that interesting, though, if we are mostly interested in how often we see `ctib`. Then we should sort with respect to the number of occurrences of `ctib` in the files.

If we remove the `-l` option to `grep` we can get all the occurrences of `ctib`.

```sh
$ grep -i ctib *.txt
bar.txt:He prayeth best, who loveth best, and do his CTiB homework.
foo.txt:BY EDGAR ALLAN CTIB POE
qax.txt:How do I love ctib? Let me count the ways.
qax.txt:I love ctib to the depth and breadth and height
qax.txt:I love ctib to the level of every day’s
qax.txt:I love ctib freely, as men strive for right;
qax.txt:I love ctib purely, as they turn from praise.
qax.txt:I love ctib with the passion put to use
qax.txt:I love ctib with a love I seemed to lose
qax.txt:With my lost saints. I love ctib with the breath,
qax.txt:I shall but love ctib better after death.
```

These are mostly unique lines, so if we counted them we would get multiple counts per file rather than the total number in a count. Our friend `sed`--well, he's not a friend yet but some day he will be--can help us again. The command will look like line-noise but here it is:

```sh
$ grep -i ctib *.txt | sed 's/:.*$//'
bar.txt
foo.txt
qax.txt
qax.txt
qax.txt
qax.txt
qax.txt
qax.txt
qax.txt
qax.txt
qax.txt
```

The `s/:.*$//` removes everything between the `:` and the end of a line. The command works like this: `s/.../.../` means search for something that looks like what is at the first `...`'s place and *s*ubstitute it with what is at the second `...`'s place. We ask for `:.*$` to be replaced by an empty string. In `:.*$` the `:` just means `:` but `.` means "any character at all", like the `?` in glob, and `*` means "zero or more of", so `.*` means zero or more characters. The `$` means "end of line" so `:.*$` means anything from `:` to the end of the line. This kind of stuff, called regular expressions, are standardised but tricky to learn, so don't worry about it here. You just need to know that `grep -i ctib *.txt | sed 's/:.*$//'` will give you one occurrence of a file name for each time `ctib` occurs in that file.

You've already seen that `uniq -c` can count the number of occurrences for you. The times we have used it, we had to sort the input before we could use it, and you can do that with `sort`, but do you need it here?

```sh
$ grep -i ctib *.txt | sed 's/:.*$//' | uniq -c
   1 bar.txt
   1 foo.txt
   9 qax.txt
```

Almost what we want. But we want the file with the most occurrences att the top. Can you get it there?

Another way to do this is to use `grep`'s `-c` (count) option. It will print each file name with a count of occurrences after it:

```sh
$ grep -ic ctib *.txt
bar.txt:1
baz.txt:0
foo.txt:1
qax.txt:9
qux.txt:0
```

Now `grep` has already done most of the work for us, we just need to sort the results.

Just running it through `sort` is not going to cut it. That will sort the result by file name and not by count, since it will simply sort the lines lexicographically. However, `sort` can do more than that. The `-t` option tells it that the data consists of a number of columns, separated by some field character. We have two columns, separated by `:`, so `-t ':'` will tell `sort` that we have two columns separated by `:`. Then, we want to sort by the second column, or "key", and the option `-k` does that: `-k2` tells `sort` to sort by the second key. Add `-n` to sort nummerically and `-r` to get the largest number at the top and we are done.

```sh
$ grep -ic ctib *.txt | sort -t ':' -k2 -rn
qax.txt:9
foo.txt:1
bar.txt:1
qux.txt:0
baz.txt:0
```

If you want to remove the files that have zero occurrences, you can use `grep` once again. The option `-v` tells `grep` to print the lines that do *not* match the search string and the string `:0$` specifies a colon followed by a zero and then a newline, and those are the ones we want to remove.

```sh
$ grep -ic ctib *.txt | sort -t ':' -k2 -rn | grep -v ':0$'
qax.txt:9
foo.txt:1
bar.txt:1
```

 Now, what if we needed the actual file names and not file names together with counts? We might want to use the file names further down a pipeline. Can we remove the counts? Of course, in thousands of different ways. An easy one is using the command `cut`. It handles files that it can interpret as columns--similar to what `sort` just did for us--and we can use it to pick out columns that we want. The `-d` option specifies the column separator (it does roughly the same as `-t` for `sort`) and the `-f` flag specifies which field (column) we want, so `cut -d: -f1` will split lines into columns on `:` and pick the first column.

```sh
$ grep -ic ctib *.txt | sort -t ':' -k2 -rn | grep -v ':0$' | cut -d: -f1
qax.txt
foo.txt
bar.txt
```

Our friend `sed` can also do it, but the command is far less readable:

```sh
$ grep -ic ctib *.txt | sort -t ':' -k2 -rn | grep -v ':0$' | sed 's/^\(.*\):.*$/\1/'
qax.txt
foo.txt
bar.txt
```

The reason `sed` commands are so unreadable is that the tool can do far more than simple tools like `cut` and at the same time it was written by people who thought key strokes were a scarce resource. The `s/.../.../` part means substitute the first part by the second part as before. The `^\(.*\):.*$` means any string that starts at the beginning of a line, `^`, matches anything `.*` up to a `:` and then anything again `.*` until the end of the line `$` and the `\(...\)` bit tells `sed` to remember that bit of string so we can get to it later. It is the first such grouping and we can get at it with `\1`. So with `s/.../\1/` we replace the first bit with the first `\(...\)` grouping in it. With time you will learn to write commands like this, but they will always be hard to read... Anyway, `sed` is a powertool and when `cut` will do, you will want to use the simpler tool.



