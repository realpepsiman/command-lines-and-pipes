# A (very) quick and (very) dirty introduction to the UNIX command line

This class is not about the UNIX environment and the UNIX command line[^1]—we have another class for that—but knowing how a command line works and what the components of it are is an essential skill for any serious use of computers in a scientific setting. Textual interfaces where you can combine different tools in a consistent way has time and time again proven to be a more flexible and versatile than the graphical user interfaces you might be familiar with from desktop computing, and the UNIX command line provides a powerful framework for this, based on a few simple ideas.

The ideas are simple, at least. There is a bit of a learning curve if  your computational background is desktop computers. However, once you grasp the basic ideas the rest will follow over time.

When you write your own programs, they will usually have to fit into this command line environment. The purpose of this exercise is to understand the UNIX command line well enough that we can write programs that play well with other command line tools. Luckily, this doesn't require that we first master the command line, understanding the much simpler underlying ideas suffices.

In the following we shall explore the basic anatomy of a command line tool and see how we can combine tools if they are written with the command line in mind. We will do this with various examples of command line tools and so-called "pipelines". We will only see a small subset of the tools you will have available in an UNIX-like environment, and only see a tiny fraction of what these tools can do. The point is not to become experts in the different tools—that can come later—but how the “language” of the command line can combine tools. The examples are simply teasers for what you might be able to do with the tools, if you explore them further.


## The terminal and the shell

First, let’s get you a UNIX shell and a terminal. If you are running Linux or macOS, you already have a UNIX system—both operating systems are UNIX variants—but if you are running Windows you will not have a UNIX environment by default. Not to worry, you can install one. [WSL](https://docs.microsoft.com/en-us/windows/wsl/install) or [MobaXterm](https://mobaxterm.mobatek.net) will give you a virtual machine running Linux, for example. But for these exercises, we do not need a full UNIX environment, and the [PowerShell](https://docs.microsoft.com/en-us/powershell/), that I am told is getting popular, will do just fine.

I don’t have any experience with running UNIX on Windows, as I haven’t used Windows since the last century, so I will trust that you can work out yourself how to get a working version up and running if you want to go for that solution. If you decide to use PowerShell, I have checked that all the examples should run. I will have to defer to [elsewhere for how to start it, though](https://www.howtogeek.com/662611/9-ways-to-open-powershell-in-windows-10/).

Ok, from now on, I will assume that you can start a terminal. What you will see depends on your system and countless setup parameters, but when I open a shell, I get a window that looks like this:

![iTerm with fish](figs/iterm-with-fish.png)

What you will see can look quite different, because much of what we are seeing can be configured according to personal taste.

What we have here is actually two separate programs, a *terminal* and a *shell*. The two tend to merge when we talk about them, but they are separate things. One, the *terminal*, is the window you see on your computer’s desktop. It is responsible for displaying text and for handling input from you when you type on your keyboard. The name “terminal” refers back to the Elder Days where you only communicated with your computer through a keyboard and a green on black screen. The hardware was there terminal. Things have changed, so now you have a window on your screen instead that does the same as the hardware used to do, and the program that handles that inherited the name *terminal*.

The difference between terminals is mostly cosmetic; they provide different types of eye candy, but rarely different functionality. Sometimes a little, and the terminal I use, [iTerm](https://iterm2.com) has some features that I quite like, but it is very rare that the choice of terminal matters the slightest. Use whatever default terminal you have, and it will be a long while before you need to consider alternatives.

The other program, the *shell*, is the one that is doing the real work. The terminal gives the shell all the text you write, then the shell does what it does with it and provides a result, that the terminal then displays.

When you type in commands, the shell will parse the commands you give it and execute programs according to the instructions you gave it. Since it is up to the shell to interpret what you write, the choice of shell matters more than the choice of terminal. Different shells have different syntax—they give you small languages to provide the computer with instructions in—but the different shell (languages) have roughly the same features and for a novice, it doesn’t matter which shell you use. When you get to where you write more complicated commands, or where you start programming in the shell language, it will matter, but for now it doesn’t.

The shell I’ve been using the last couple of years is called [fish](https://fishshell.com). I find it useful for interactive work with the computer. For programming, I tend to use [bash](https://www.gnu.org/software/bash/) instead, though. If you are on macOS or Linux, the default shell is probably bash. It will also be the default for some of the Windows solutions, but if you installed PowerShell, the shell is (you guessed it) PowerShell.

Below you can see the same commands in three different shells, fish, [zsh](https://www.zsh.org), bash and PowerShell, respectively. As you can see they all look a little different, but you can configure all of them to look completely different so you shouldn't put too much into that. The four images show the same commands in the different shells, first I change directory `cd` to `Classroom/birc-ctib` and then I list `ls` the files there.

![fish](figs/fish.png)

![zsh](figs/zsh.png)

![bash](figs/bash.png)

![pwsh](figs/pwsh.png)

The basic interaction with all shells involves typing something into a *prompt*. The prompt on all the figures is to the left of the cursor, the big rectangular block you can see on the last line (it isn't there on the lines where the shell has already evaluated a command). Prompts can be configured, so below I will use `$` as the example prompt, but keep in mind that it will look different on your computer.

When you interact with your shell, you write a command at the prompt and then hit ENTER. (This should be familiar to most of you). Then the shell will interpret what you wrote, execute the command you gave it, and print the result.

How this work, in some more detail, is what we will learn in this exercise, but first, we will look at a few example commands.

## A few useful commands

This will not be a tutorial of most useful commands you will want to know, you will pick those up with time if explore the UNIX environment as you work on exercises and projects, but I will show you a few to get you started.

### man

The most useful command to know is `man`, short for *manual*. This is where you get information about the tools you have. If you write

```sh
$ man man
```

you get the manual for the `man` command.

Back in the good old days, all tools would have a manual page that described them, and all the old tools still do. Unfortunately, this isn’t true for many modern bioinformatics tools, where you have to run the tool with a flag such as `-h` or `--help` to get the same information, but that is a worry for another day. For the commands I will show you in this exercise, `man` will give you a description. Use this command to learn more about the various tools and commands.

### ls

The `ls` command you saw in the examples above lists the files in a directory. Try the command

```sh
$ man ls
```

You will find that the command takes an unreasonable number of options. The exact options depend a bit on which platform you are on, but there will be more than you care to study right now on all platforms. On my machine, the options are

```
ls [-ABCFGHLOPRSTUW@abcdefghiklmnopqrstuwx1%] [file ...]
```

where the long line `[-ABCFGHLOPRSTUW@abcdefghiklmnopqrstuwx1%]` are all the options. Ignore them. The important part is `ls [file ...]`. This bit tells you that `ls` takes zero or more files as arguments. Zero or more because `file ...` is in `[…]` which means that they can be left out and “more” because of the ellipsis after `file`.

We used it without arguments above

```sh
$ ls
```

and if we do this, it will list the current directory. (More on that shortly).

If you give it options, which will typically be directories, it will list those. So

```sh
$ ls foo
```

will list the file or directory `foo`. If `foo` is a file, it will just list `foo`, and if `foo` is a directory it will list all the files in it.

### mkdir, touch and rm

Try running these commands:

```sh
$ mkdir foo
$ touch foo/bar
$ touch foo/baz
$ ls foo
```

What do you see?

Use `man mkdir` to find out what `mkdir` does.

(It creates a new directory).

Use `man touch` to see what `touch` does.

(It “touches” a file, which isn’t something we need here, but touching a file that doesn’t exist has the side-effect of creating it).

Why do you get the result you get when you run these commands?

If you want to remove a file, the command `rm` is what you need. However,

```sh
$ rm foo
```

won’t work for us here. Since `foo` is a directory, we cannot simply remove it the same way as we can a file. However,

```sh
$ rm -r foo
```

will do the trick. The option `-r` tells `rm` to remove files recursively, and then it will happily take a directory and remove it together with all files and directories inside it.

A command such as `rm -r foo` gives us a common pattern for instructions to a shell. The `rm` bit is a program that we want to run. The `foo` argument is an option to it, and the `-r` bit is a flag that modifies how the program is run. There are some conventions for how programs use flags and options and such, but unfortunately there is more than one convention, so you never really know how any given program handles arguments. That is up to the programmer who wrote the tool, and you have to check the documentation.

We will see how to write tools that take arguments, and how to parse the arguments as flags and options, in some of the later exercises and projects.

### echo

The command `echo` prints the arguments you give it:

```sh
$ echo foo bar baz
foo
bar
baz
```

Think of it as a simple “print” statement in a shell. When you are working interactively, it doesn’t seem like a terribly useful command, but it does have its usage.

If you write

```sh
$ echo foo bar baz > qux
```

you will have created a new file, `qux`, that contains the lines:

```
foo
bar
baz
```

The magic happens with the symbol `>`, and it isn’t actually `echo` doing the magic but the shell. When we write this, we redirect the output of the command to the left of `>` into the file on the right. We return to this in a little bit. 

### cat, more and less

The `cat` command con*cat*tenates files.

If you call `cat` with a number of files, it will print their content one after another. A number could be one, so you can write

```sh
$ cat qux
foo
bar
baz
```

to get the content of the file `qux` we created above. The same file can appear more than once, so you could also get:

```sh
$ cat qux qux
foo
bar
baz
foo
bar
baz
```

It is a useful command if you want to see the content of a small file, but if a file is large, you probably don’t want to get all of it printed. Instead you can get a view of it with the commands `more` or `less`. (The `less` command is an improved `more` and “`more` is `less`” is programmer humour).

Try running `more qux` and `less qux`.

If you read `man cat` you will see `[file ...]` in the command description, telling you that you can also call `cat` with zero files. If you do this, it will look like nothing is happening.

```sh
$ cat

```

but try writing something and hitting ENTER. It will echo what you just wrote. Write some more and hit ENTER again, and the same thing happen. When you grow tired of this you can hit `CTRL-d` to finish.

What happens is that `cat` reads from “standard input”. It takes over from the shell so all you write goes to `cat`, and everything you write will be echoed because that is what `cat` does—it prints each input line.

Just as you can send the output of a command to a file with `>` you can also tell a program that its “standard input” should be taken from a file. The symbol is then `<`. So

```sh
$ cat < qux
foo
bar
baz
```

in a UNIX shell will do the same as

```sh
$ cat qux
```

This, however, doesn’t work with PowerShell. We can get a similar effect in a different way, but we get to that later.

For `cat`, this isn’t useful in itself; we get exactly the same effect by calling `cat qux` as `cat < qux`, but it has its uses as we shall also see shortly.

### cmp and diff

If you want to see if two files are identical—you can probably imagine that this can be useful—two tools are particularly useful. The `cmp` command will tell you if two files are the same. It will print that they are different if they are

```sh
$ echo foo baz bar > qax
$ cmp qux qax
qux qax differ: char 7, line 2
```

or print nothing if they are the same:

```sh
$ cmp qux qux
```

Just because it doesn’t print anything doesn’t mean that the tool doesn’t return a value, it just isn’t printed. In PowerShell or bash you can get the status of a command using `echo $?`

```sh
$ cmp qux qux
$ echo $?
True
$ cmp qux qax
qux qax differ: char 7, line 2
$ echo $?
False
```

(PowerShell will write `True` or `False` while bash will write 0 for true and non-zero for false).

This is mostly useful for shell programming, which is beyond the scope of this class.

If you want to see the differences between two files, `diff` is your tool.

```sh
$ diff qux qux
$ diff qux qax
2d1
< bar
3a3
> bar
```

It will also print nothing if the files are identical—then there are no differences—and otherwise it will print the differences it see. You need to remove `bar` in the second line of `qux` and then add a `bar` at the third line to get `qax`. The way `diff` displays the differences can be changed with a plethora of options that you can learn about using `man diff`.

The `-y` option is particularly useful. It displays the two files next to each other with `<` when a line is present in the first file and not the second, and with `>` when something is present in the second file but not the first.

```sh
$ diff -y qux qax
foo     foo
bar   <
baz     baz
      > bar
```


## Navigating the file system

I won’t say much about the file system. You already know how the file system consist of a hierarchy of directories and files. The only thing to add to this, when it comes to shells, is that we have something called the *current (working) directory*, and all commands are relative to that place.

The command `pwd` will show you where the shell’s current working directory is. You get a string of `/`-separated names. The slash is used to separate directories on UNIX (you might be used to backslash on Windows). Whenever you provide a file-name to a command, it will be interpreted in one of two ways. 

An *absolute* path to the file—any file-name that starts with a `/` is absolute, and the path—the string of `/`-separated names is interpreted as starting in the root of your file system. What that is, depends on your platform.

A *relative* path is one that doesn’t start with a `/`. Those will be interpreted as a path that starts in the current working directory and relative to that.

So, when we wrote `cat qux` we used a relative path. We gave `cat` the `qux` file in the current directory. Earlier, when we did `touch foo/bar` we also used a relative path. We specified the directory `foo` in the current directory, and then the file `bar` inside that `foo` directory.

In addition to this, there are two special names, `.` and `..`. A single dot, `.`, always refers to the current directory. So if we wrote `cat ./qux` we would explicitly say that we wanted the `qux` in the current directory. There are some situations where it is necessary to specify `./qux` instead of `qux`, but we won’t go into that here.

The two dots, `..`, refers to the parent of a directory. If we wrote `cat ../foo` we would be looking for the `foo` file not in this directory but in the directory one up.

Try running these commands and see what you get:

```sh
$ ls .
$ ls ..
```

The two dots can be used inside a path, so if we wrote

```sh
$ mkdir foo
$ ls foo/..
```

we would ask `ls` to list the parent directory of `foo` (which would be the same as the current directory.

To change directories you use the command `cd`. It takes a path as an argument, so you can change the current directory to the sub-directory `foo` using

```sh
$ cd foo
```

the go back to the original directory with

```sh
$ cd ..
```

If you use `cd` with arguments, you will be send to your home directory, whatever that is. It depends on your platform, and it isn’t an important concept on a personal computer, but it is if you get an account on a shared system like our GenomeDK cluster. The home directory is the root of all your own files, kept separated from other users’ files.

### grep

The `grep` command lets you search in files. The name has a weird mnemonic, `grep` stands for “global, regular expression, print”, a name that made perfect sense in the Elder Days where it was a command you could give the `ed` editor, but today it is pure nonsense. Instead, `grep` has become a verb in its own right, and you will hear programmers talk about “grepping” for stuff.

The simplest usage is `grep word file` that will search for `word` in the file `file` and print all the lines where `word` is found.

A boring example is this:

```sh
$ grep foo qux
foo
```

It prints a single `foo` as there is one line in `qux` that contains `foo`.

If you search in more than one file, `grep` will also tell you which file it found `word` in.

```sh
$ grep foo qux qax
qux:foo
qax:foo
```

The countless options you can give `grep` can change how it outputs it findings, whether it prints the lines where it finds `word` or just which files it found `word` in, ,whether it should output the files it *didn’t* find `word` in instead, and so on. It is one of the most useful search commands you have at your disposal on the command line, once you learn how to use it.

## Arguments, standard input, output, and error, and redirecting

I know that it all looks confusing and overwhelming at this point. You’ve seen a tiny fraction of the commands you have available in a shell, and all of them take an obscene number of options, and there is no way that you can remember the commands or the options. Luckily, with `man` you don’t have to remember the options, but if you don’t know the name of a command, `man` is of no use.

It *is* overwhelming; it *is* confusing. It is not just you. There *are* too many commands to remember, and every time someone writes a new tool, yourself included, there is one more command to add to the toolbox.

Have no fear, though. It is just like learning a new language. A new language contains tens or hundreds of thousands of words and you need to learn a lot of them before you can speak the language, and at least you have fewer words on the command line than in English, Swahili or Dutch.

The more you use the language, the more words you will learn, and soon you will speak a passable command line, if not yet completely fluent. The only thing you need to do to keep learning is to, whenever you want to do something, X, ask the question: “how do I do X in my shell?”. You can ask me, you can ask a friend, and Google is your friend if you have no others. Slowly but surely you will increase your vocabulary.

Luckily, the grammar is much simpler for a shell than a natural language. It is more complicated than I will show you here—and each shell is a separate language with its own grammar when it comes to the more complicated stuff—but it is simpler than the natural languages you already speak.

I will show you the basic grammar, though, and illustrate how a simple grammar gives you a powerful language.

We start with the grammar for a single command. You’ve seen several examples above. A command looks like this:

```sh
$ some-command arg1 arg2 arg3 …
```

There is a command at the beginning, `some-command`, and then zero or more arguments, `arg1 arg2 arg3 …`

When you write a command like that, the shell will find a program called `some-command` and execute it, giving it the arguments. Some strings you can’t use as arguments, we will see most of them below, because the shell will interpret them as commands to itself rather than the program, but generally it just gives the program the arguments.

What the program does with the arguments is entirely up to the program. The shell doesn’t know, nor does it care. That is the program’s responsibility. The way to find out what they mean is using `man some-command`.

When you write your own programs, you have access to the arguments that a user provides on the command line. Where you have them depends on the programming language you use, and I will show you how to get at them in Python, the language we use for this class, next week.




## The UNIX command line paradigm—the pipeline


[^1]: When I refer to the UNIX command line here, I don't mean to imply that this particular way of interacting with a computer is exclusively a UNIX feature. The command line was used before UNIX and variants such as DOS developed later. The particular way of interpreting a command line, and in particular the way to connect commands in pipelines, however, originated in UNIX.