# Creating files and directories and moving them around

Now that you know how to move around in the directory hierarchy, you might be wondering how to create your own directories. That's easy, you use the "make directory" command, `mkdir`.

Try this:

```sh
$ mkdir foo
$ ls
README.md foo
```

If you want to remove your shiny new directory again, you use the "remove directory", `rmdir`, command:

```sh
$ rmdir foo
```

There are thousands of ways of creating new files. Many commands will create files for you, and if nothing else, you can redirect the output of a command into a file to create the file. For simplicity, let's create a file that contains the string "Hello, World!\n".

```sh
$ echo "Hello, World!" > hello.txt
$ cat hello.txt
Hello, World!
```

If you are wondering what the `\n` was doing above, since it doesn't appear on the command line or the output of the `cat` command, it is a newline. The file `hello.txt` contains the string "Hello, World!" and then a newline. When we print it with `cat`, we just see a line change after "Hello, World!", but in the file there is a byte representing this, and in UNIX that symbol is written as `\n`.

If you want to delete a file, you use the "remove" command, `rm`.

```sh
$ rm hello.txt
```

The `rmdir` and `rm` commands do almost the same thing, they delete something in your file system, but one is for directories and the other is for files. There isn't any technical reason for keeping those two operations as separate, you could have just `rm` and have it work on directories, but we have two commands to avoid accidentally treating a directory as a file or vice versa. You can override some of that behaviour, though, and there are cases where you might want to. If, for example, you have a directory that contains other directories and files

```sh
$ mkdir foo
$ mkdir foo/bar
$ echo qux > foo/qux
$ echo qax > foo/qax
```

the `rmdir` command will not let you delete a non-empty directory

```sh
$ rmdir foo
rmdir: foo: Directory not empty
```

because `rmdir` will try to protect you from accidentally do that. You can, however, delete the directory with `rm` if you use the option `-r` (recursive)

```sh
$ rm -r foo
```

**A warning about removing files and directories.** UNIX generally assumes that you know what you are doing, and with few exceptions, such as keeping `rm` and `rmdir`, it does little to protect you from yourself. If you tell it to delete a file, that file is gone! If you wanted it in a trash can, you would have moved it there instead. It certainly won't, because you didn't ask for it. There is no "undo" on the command line, and deletion is forever. Be careful. (And use version control systems like `git` to get something even better than "undo").


If you want to rename a directory (or file) you don't have a "rename" command, but you have a "move" command that can do the same. If you move a directory to a new name, you have renamed it.

```sh
$ mkdir foo
$ mv foo bar
$ ls
README.md bar
```

The `mv` command is just a little more versatile than a rename one would be. If you want to move a file or directory into another directory, the `mv` command will happily do that as well.

```sh
$ mkdir baz
$ mv bar baz
$ ls
README.md baz
$ ls baz
bar
```

When we do `mv bar baz`, `mv` can see that `baz` already exists and is a directory, so it doesn't overwrite it, but it moves `bar` there instead. If `baz` didn't exist, we would rename `bar` as we did for `foo` -> `bar` above. If it existed, but was a file, it would be overwritten.

If we can move files, can we also copy them? Of course we can, and the command is called `cp`.

```sh
$ ls
README.md baz
$ ls baz
bar
$ echo foo > foo
$ ls
README.md baz foo
$ ls baz
bar
$ cp foo qux
$ cp foo baz/qax
$ ls
README.md baz foo qux
$ ls baz
bar qax
```

You cannot copy a directory with `cp`, for the same reasons that you cannot `rm` a directory, but you can copy it with all its stuff if you give `cp` the flag `-r`:

```sh
$ cp -r baz foobar
$ ls foobar
bar qax
```

That was what I wanted to show you here. Try playing around with these commands for a bit. When you tire of it, go up one directory and pick the next exercise.
