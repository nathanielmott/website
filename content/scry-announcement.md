+++
title = "introducing scry: a magical stat(1) clone"
date = 2023-09-16
draft = false
slug = "introducing-scry-a-magical-stat-clone"
+++

*tl;dr: i made a simple command-line tool!*

`scry` is, as the title suggests, a magical `stat(1)` clone. This post outlines the beginning of the project, how `scry` differs from `stat`, and where I plan to go from here.

 <!-- more -->

## how it started

`scry` began as a combiniation of `stat` and `wc` intended to help me familiarize myself with Rust. It didn't take long to have an initial prototype (Rust's standard library provides most of the necessary functions) but I quickly identified a problem: getting the word, line, and paragraph counts from a file counted as "accessing" it. There was no way for me to "stat" the file without messing with the results.

I originally solved this problem by having the word counting occur *after* the stat call. This kinda worked, but only if I was fine with the data being messed up afterwards. So then I broke the `stat` and `wc` clones up into arguments; that way I would only mess with the data if I was truly committed to getting those counts.

This was...fine. `scry` was still faster than both `stat` and `wc` in my benchmarks, and because I decided what would be printed, it offered a cleaner view into the data I cared about. Having to supply the argument every time was a bummer, but meh.

Then I realized that `scry` didn't need to be a combination of `stat` and `wc`; it could just be a better `stat`. (And the better `wc` could be its own project, of course, which I plan to take on after I've finished this iteration of `scry`.)

## the problems with `stat`

I have three problems with `stat`:

1. It provides data I don't care about.
2. It doesn't accept piped input.
3. It can't recursively check files within a directory.

I'd already solved the first problem with `scry`. That left two more: accepting piped input and recursively checking files within a directory.

### where tf is my pipe support?

`stat` can be told to analyze multiple files at once. I assumed this meant it would be possible for me to `ls | stat` and get the results for every file in a directory. It's not. You get this result:

```
ls | stat
stat: missing operand
Try 'stat --help' for more information.
```

You can solve this problem with `xargs`:

```
ls | xargs stat
// results
```

But only if you're in the same directory. Let's say I want to check the files within a subdirectory. For this example, I'm in my "notes" directory and want to check the "rust" subdirectory that houses my notes from various learning materials:

```
ls rust | xargs stat
stat: cannot statx 'programming-rust-book-notes.md': No such file or directory
stat: cannot statx 'rust-atomics-and-locks-notes.md': No such file or directory
stat: cannot statx 'rust-in-action-notes.md': No such file or directory
stat: cannot statx 'rust-ownership.md': No such file or directory
stat: cannot statx 'rustlings-notes.md': No such file or directory
stat: cannot statx 'the-rust-book-notes.md': No such file or directory
```

This is only partly `stat`'s fault. `ls` doesn't return a full path for each file, so as soon as you use it to list the contents of another directory, the path for each result can no longer be inferred. Even if you use `ls` in the current working directory, however, `stat` won't accept its input without invoking `xargs`.

`scry` will. (And, in fact, currently doesn't support `xargs`. Whoops.) That means you can easily collect metadata about all of the files within a directory using `ls | scry`. I find that `scry`'s line-based output also results in better... erm, results when using tools like `ripgrep`, but that's a minor advantage to most workflows.

But what if you want to gather info about the files in another directory?

### lemme peek at that dir

If you `stat` a dir, you'll get results like this. (And this example stats the "src" directory of the `scry` repository because I'm using a dev version of the latter.)

```
stat src
  File: src
  Size: 4096            Blocks: 8          IO Block: 4096   directory
Device: 259,2   Inode: 44171508    Links: 2
Access: (0755/drwxr-xr-x)  Uid: ( 1000/    mott)   Gid: ( 1000/    mott)
Access: 2023-09-11 10:30:20.952159248 -0400
Modify: 2023-09-11 10:30:09.045534814 -0400
Change: 2023-09-11 10:30:09.045534814 -0400
 Birth: 2023-07-22 10:06:38.255573416 -0400
```

That's cool! And if you `scry` that dir:

```
scry src
File:     src
User:     mott
Path:     /home/mott/projects/scry/src
Bytes:    4096
Created:  Jul 22, 2023 10:06 -04:00
Modified: Sep 11, 2023 10:30 -04:00
Accessed: Sep 11, 2023 10:30 -04:00
```

Also cool! But remember when I wanted to `ls` another directory and get information about the results? That's because `stat` can't recursively enter a directory and gather metadata about each file inside. (Or at least that's my understanding based on the `stat` man page; let me know if you've found a way to do it!)

`scry`, on the other hand:

```
scry -r src // recursive would also work
Name:     metadata.rs
Path:     /home/mott/projects/scry/src/metadata.rs
Type:     file
User:     mott
Bytes:    3240
Created:  Sep 11, 2023 13:02 -04:00
Modified: Sep 11, 2023 13:02 -04:00
Accessed: Sep 11, 2023 13:02 -04:00

Name:     main.rs
Path:     /home/mott/projects/scry/src/main.rs
Type:     file
User:     mott
Bytes:    1263
Created:  Sep 11, 2023 11:22 -04:00
Modified: Sep 12, 2023 11:13 -04:00
Accessed: Sep 12, 2023 11:13 -04:00
```

`scry` currently only supports one level of recursion, so if the target directory contains a subdirectory, it will return the same value as if you'd just used `scry <SUBDIRECTORY>`. I might add the ability to specify multiple levels of recursion, although I suspect that would be a lot of info to print all at once.

(As an aside: `scry`'s results are much clearer than the equivalent `cd DIR && ls | xargs stat` because of the newline; stat doesn't separate results when used on multiple files. I find that leaves an unreadable mess in my terminal window.)

## where stat wins

There are two areas where `stat` beats `scry`:

1. It's preinstalled on most (all?) distros.
2. It can (at least attempt to) query the filesystem.

There's nothing I can do about that first point. I'll look more into the second point in the future, but for now, if you need to `stat -f` something, `scry` doesn't offer an equivalent functionality. (Thank goodness `stat` is already on your system!)

## a potential roadmap

`scry` operates under the assumption that provided paths are valid UTF-8. This should be a fair assumption in the year two thousand and twenty three, but I'll look into this, even though it makes me angry and shouldn't be necessary.

I plan to introduce two additional output methods before `scry` reaches 1.0: JSON and a "legacy" mode that emulates `stat`. The ability to support configurable output like `stat` does is also a nice-to-have but isn't currently planned.

Windows support is unplanned until I can test it locally--or someone else who *can* test locally submits the necessary changes to enable support for the platform. `scry` should work on all Unix platforms, though, so please tell me if it doesn't.

There are some performance optimizations I could implement--namely using a buffered writer when printing information about multiple files / directories--but as it stands `hyperfine` says that running `scry -r notes` from my home directory is approximately seven times faster than running `cd notes && ls | xargs stat`. That ain't bad!

(And for a completely unrealistic benchmark: `ls | scry` in a directory containing 10,244 files is approximately 48 times faster than `ls | xargs stat`. Any performance optimizations beyond that, frankly, would be more indulgent than practical.)

## closing thoughts

I fully expected `scry` to be a quick project that I could archive right after it helped me figure out some of the conventions around writing a CLI program in Rust. My only other programming experience outside of Advent of Code is following along with the "100 Days of SwiftUI" course, so I had a lot to learn with this first project.

I didn't expect to end up creating a tool with--if I can be allowed to brag for a moment--a more Unix-like approach (allowing piped input without `xargs`) and more features (recursive directory scanning) than a decades-old tool found in most distros. Even though `scry` has a ways to go, I'm damn pleased with this 0.5 release.

The source code for `scry` can be found on my Sourcehut. (The version on my GitHub is out of date and should not be used.) I plan to make installing the tool easier in the future, but for now, it's primarily intended for anyone comfortable building it from source. 
