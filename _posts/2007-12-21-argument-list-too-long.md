---
title: "关于“Argument list too long”"
layout: post
tags: ["linux", "tools", "operation"]
---

今天又看见这个信息了，虽然已经知道是glibc的限制，但还是好奇了一把，找了找资料。最后终于发现是ARG_MAX。

<!-- more -->
下面是一篇参考资料

----------------------------------------------------------------------------
Re: argument list too long
From: Bob Proulx Subject: Re: argument list too long Date: Thu, 18 Oct 2001 09:22:21 -0600
Frank

> Date: Thu, 18 Oct 2001 10:30:03 -0700

An interresting time warp. My machine received your mail before you
sent it. :-)

Thu, 18 Oct 2001 08:33:29 -0600

> almost any program like 'rm' 'ls' etc complains to display
> or delete my directory contents (9780 files). doing it
> with a workarround is cumbersome. can't these progrs use dynamic
> memory allocation ?

Those programs do use dynamic memory allocation. I am not sure why
you think they do not. But some specific versions do have known
problems. It would be most helpful if we knew the OS you were running
on, the version of the program and the exact commands you were running
to recreate any problem behavior. Otherwise we can only guess and
ass*u*me.

But you said 9780 files which would lead me to believe you ran out of
environment space. That leads me to the following canned answer about
this. But notice the message came from "bash" and not from "mv" in my
example below. It is an operating system limitation and usually
reported by your command line shell. You did not include your example
and so I can't tell if this mathes your problem exactly. I can only
guess and assume it does. :-)

Bob


Canned reply follows...

I tried to move about 5000 files with mv, but it said:

bash: /bin/mv: Argument list too long

The UNIX operating system tradionally has a fixed limit for the amount
of memory that can be used for a program environment and argument list
combined. You can use getconf to return that limit. On my Linux system
(2.2.12) that amount is 128k. On my HP-UX system (11.0) that amount is
2M. It can vary per operating system. POSIX only requires 20k which
was the traditional value used for probably 20 years. Newer operating
systems releases usually increase that somewhat.

getconf ARG_MAX
131072

Note that your message came from "bash" your shell command line
interpreter. Its job is to expand command line wildcard characters
that match filenames. It expands them before any program can see
them. This is therefore common to all programs on most UNIX-like
operating systems. It cannot exceed the OS limit of ARG_MAX and if it
tries to do so the error "Argument list too long" is returned to the
shell and the shell returns it to your.

This is not a bug in 'mv' or other utilitities nor is it a bug in 'bash'
or any other shell. It is an architecture limitation of UNIX-like
operating systems. However, it is one that is easily worked around
using the supplied utilities. Please review the documentation on 'find'
and 'xargs' for one possible combination of programs that work well.

You might think about increasing the value of ARG_MAX but I advise
against it. Any limit, even if large, is still a limit. As long as it
exists then it should be worked around for robust script operation. On
the command line most of us ignore it unless we exceed it at which time
we fall back to more robust methods.

Here is an example using chmod where exceeding ARG_MAX argument length
is avoided.

find htdocs -name '*.html' -print0 | xargs -0 chmod a+r
