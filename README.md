NAME
====

cync - Simple Synchronize tool across ssh

SYNOPSIS
========

    $ cync [-n] <src> <dest> [find-args]

    $ cync foo bar -name .git -prune -o
    $ cync foo jacob@monstro:/home/jacob/foo

    # more in depth example:
    $ mkdir -p foo/d
    $ touch foo/{a,b,d/c}
    $ ln -s a foo/l

    $ ./cync foo bar
    find foo -printf "%y %Ts %m \0%p\0 \0%l\0\n"
    find bar -printf "%y %Ts %m \0%p\0 \0%l\0\n"
    find: `bar': No such file or directory
    mkdir bar
    chmod 775 bar
    touch -d @1358736682 bar
    cp foo/a bar/a
    chmod 664 bar/a
    touch -d @1358736677 bar/a
    cp foo/b bar/b
    chmod 664 bar/b
    touch -d @1358736677 bar/b
    mkdir bar/d
    chmod 775 bar/d
    touch -d @1358736677 bar/d
    cp foo/d/c bar/d/c
    chmod 664 bar/d/c
    touch -d @1358736677 bar/d/c
    ln -s a bar/l
    done!

    $ ./cync foo work:bar
    find foo -printf "%y %Ts %m \0%p\0 \0%l\0\n"
    ssh work find bar -printf "%y %Ts %m \0%p\0 \0%l\0\n"
    find: `bar': No such file or directory
    ssh work mkdir bar
    ssh work chmod 775 bar
    ssh work touch -d @1358736682 bar
    scp foo/a work:bar/a
    a                                                       100%    0     0.0KB/s   00:00
    ssh work chmod 664 bar/a
    ssh work touch -d @1358736677 bar/a
    scp foo/b work:bar/b
    b                                                       100%    0     0.0KB/s   00:00
    ssh work chmod 664 bar/b
    ssh work touch -d @1358736677 bar/b
    ssh work mkdir bar/d
    ssh work chmod 775 bar/d
    ssh work touch -d @1358736677 bar/d
    scp foo/d/c work:bar/d/c
    c                                                       100%    0     0.0KB/s   00:00
    ssh work chmod 664 bar/d/c
    ssh work touch -d @1358736677 bar/d/c
    ssh work ln -s a bar/l
    done!

    $ echo yup > foo/y

    $ ./cync foo bar
    find foo -printf "%y %Ts %m \0%p\0 \0%l\0\n"
    find bar -printf "%y %Ts %m \0%p\0 \0%l\0\n"
    cp foo/y bar/y
    chmod 664 bar/y
    touch -d @1358736930 bar/y
    done!

    $ ./cync foo work:bar
    find foo -printf "%y %Ts %m \0%p\0 \0%l\0\n"
    ssh work find bar -printf "%y %Ts %m \0%p\0 \0%l\0\n"
    scp foo/y work:bar/y
    y                                                       100%    4     0.0KB/s   00:00
    ssh work chmod 664 bar/y
    ssh work touch -d @1358736930 bar/y
    done!

DESCRIPTION
===========

This program will run a find(1) command on both source and destination
and synchronize the results.

The source or destination can be a ssh host:file. It doesn't matter if
they are regular files or directories.

-n will just print what it would do, but won't actually do it.

Nice because find(1) lets you include or exclude whatever files you
need with a fairly complete syntax. Dont include the action (print of
the results), that will be generated for the program by the program.

It's meant to be simple, not particularly efficient: If you rename a
directory, this will delete the old one and transfer the new one. if
you make a small change to a large file, the whole file will be
transferred. It works in only one direction, the source directory is
not touched.

The end result of the script is a series of rm, cp, mkdir, ln, chmod,
and touch commands.

It uses the modification time to determine if the file is out of date.

