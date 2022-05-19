# Navigating directories

If you made it here, I guess you already know how to use the `cd` (change directory) command. When you

```sh
$ cd dir
```

you go to the directory `dir`, which means you change the shell's current directory. You can test this by running

```sh
$ pwd
```

every time you have changed the directory. It will show you if you got the the expected place.

The command `cd`, and any other command that takes files or directories as arguments, generally take a so-called "path" as argument. This is a sequence of `/` separated directory or file names, that describes which sub-directires you need to go through to find a file.

If you are here, you can try looking into a sequence of sub-directories using

```sh
$ ls foo
$ ls foo/bar
$ ls foo/bar/baz
```

When we look at `foo` we are looking at an immidiate sub-directory, and when we look at `foo/bar` or `foo/bar/baz` we are looking deeper into sub-directories.

If we 

```sh
$ cat foo/bar/baz/qux.txt
```

we will see the content of the file `qux.txt` in the directory `baz` inside the directory `bar` in the directory `foo`.

If the directory name you give `cat`, `ls`, `cd` etc. starts with `/` the path is absolute, which means you are specifying a path from the root of your file system and up. If the name doesn't start with `/` it is relative to the current working directory.

Try jumping to the root of your file system with

```sh
$ cd /
```

(don't worry, I will get you back again safely).

Check that you got there with

```sh
$ pwd
```

and see what is there with

```sh
$ ls
```

What you see there depends on your operating system and its setup, and we are not particularly interested in it here. We are just sight-seeing.

Go back to where you came from by writing

```sh
$ cd -
```

The `-` path is special and means "the last directory I was in before I moved here". It is a convinient shortcut to get back when you were sightseeing or when you accidentally got to a place you didn't want to go to.

Other special paths are `.` and `..`, the current directory and the parent of it, and we have already seen those. If you change your directory to `.` you are going nowhere

```sh
$ pwd
$ cd .
$ pwd
```

If you want to go to the parent directory you use

```sh
$ cd ..
```

and then go back again with

```sh
$ cd -
```

Another special path is `~`. This path refers to your home directory. You can check where that is with

```sh
$ cd ~
$ pwd
$ cd -
```

It works with more than `cd`, so if you want to see which files you have in your home directory you can use

```sh
$ ls ~
```

or you can look at a file there with

```sh
$ head ~/foo
```

assumnig that there is a file called `foo` in your home directory.

When we type in paths to commands, they can sometimes be quite long. You can save some key-strokes by auto-completing them. Practically all modern shells will auto-complete paths if you hit TAB. Try typing

```sh
$ ls /
```

and then hit TAB. Maybe hit TAB twice; some shells won't auto-complete if your path is already describing a location, as `/` does, but with the second TAB at least you should get at list of all the directories on the root of your file system. Pick one at random, e.g. `bin` (if you have that, otherwise pick another), and type one or two letters, and then hit TAB again. Now the shell should complete the name. After that, if you are looking at a directory, another TAB or double-TAB will show you what is inside it.

The exact way that shells auto-complete depends on the shell. Some can handle only paths, other know the commands you have and can auto-complete options and such as well. Often, it is something you can configure. For now, auto-completing paths will suffice, but get used to using auto-completion and not to typing out long paths by yourself.

That was it for this exercise, I hope it wasn't too painful. Go up one directory level (`cd ..`) and then enter the next one.

