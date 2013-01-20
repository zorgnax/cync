NAME
====

cync - Simple Synchronize tool across ssh

SYNOPSIS
========

    $ cync [-n] <src> <dest> [find-args]

    $ cync foo bar -name .git -prune -o
    $ cync foo jacob@monstro:/home/jacob/foo

DESCRIPTION
===========

This program will run a find(1) command on both source and destination
and synchronize the results.

The source or destination can be a ssh host:file.

-n will just print what it would do, but won't actually do it.

Nice because find(1) lets you include or exclude whatever files you
need with a fairly complete syntax. Dont include the action (print of
the results), that will be generated for the program.

It's meant to be simple, not particularly efficient: If you rename a
directory, this will delete the old one and transfer the new one. if
you make a small change to a large file, the whole file will be
transferred. It works in only one direction, the source directory is
not touched.

The end result of the script is a series of rm, cp, mkdir, ln, chmod,
and touch commands.

It uses the modification time to determine if the file is out of date.

