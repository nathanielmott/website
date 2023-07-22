+++
title = "rust or zig? as of july 2023, i don't know"
date = 2023-07-21
draft = false
slug = "rust-or-zig-july-2023-idk"
+++

*tl;dr: started at the top, went to the bottom, now we're here. wherever that is.*

I've spent the last few weeks writing a basic port of the `stat` command in Rust and Zig. Throughout several iterations of this utility, which has reached the "I'm willing to discuss this in public" stage but not the "I'm willing to post the source code" stage, I've had to confront a difficult question: Rust or Zig?

## variable scope

This utility initially combined a word counter (similar to `wc`) and `stat` in a single tool. It seemed like the kind of thing I'd find useful, and it also required me to learn about parsing arguments in command line utilities, accessing file metadata, and formatting that data into something a human might want to read.

The fundamental problem with this approach is that any method I've found of counting words, lines, and paragraphs changes the time a file was last accessed. This means half the utility (`stat`) was undermined by the other half (`wc`) and I've yet to find a solution to this problem that isn't "accept inaccuracies with that field."

> Okay. I'm realizing as I type this that I could gather the "accessed" field before the file's contents are read, save that information somewhere, then display the saved information rather than the new information until a new timestamp is recorded.But that would be significantly more complicated than what I currently have.

So eventually I shifted to "`stat` but with only the fields I care about." (Those being the times when a file was created, modified, or accessed.) Huzzah!

## where zig and rust differ

Spoilers: Gathering and presenting this information in the user's local timezone is significantly easier in Rust and requires approximately half as many lines of code.But I learned a lot more developing the Zig version of the tool.

Both versions of this utility rely on their respective standard libraries and a crate/module for managing dates. That's because Rust and Zig both rely on the `stat` syscall--not to be confused with the command line utility of the same name!--to access file metadata. (At least on Linux; let's not discuss Windows just yet.)

Unfortunately for me, the `stat` syscall doesn't return the times a file was created, modified, or accessed in a human-readable format. Instead, you get a 128-bit integer representing the number of milliseconds since the Unix epoch time. That means I have to turn the result into something a person can understand.

This is what that code looks like in Rust using the chrono crate:

```
    let created: DateTime<Local> =
        DateTime::from(metadata.created().expect("couldn't get created"));
    let formatted_created_time = format!("{}", created.format("%m/%d/%Y %H:%M"));

```

(Let's ignore the fact that I'm not properly handling optionals or errors there. The important thing is that I was able to specify I wanted the local time.)

This is what the code looks like in Zig:
```
    let formatted_created_time = format!("{}", created.format("%m/%d/%Y %H:%M"));
    const created_local = Datetime.shiftTimezone(created_datetime, &system_tz);
    const created_formatted = try Datetime.formatHttp(created_local, allocator);
```
Except that doesn't show all the work I had to do to get that "system_tz." That requires a modification to the zig-datetime module I made after reading through the Zig standard library, RFC-8536, and compilation errors for over a day. This is the combination of functions that allows me to get the system timezone on Linux:

```
    const system_tz: Timezone = try chrono.get_system_timezone(allocator);

    pub fn get_system_timezone(allocator: std.mem.Allocator) !Timezone {
    const system_time = std.time.timestamp();
    const tzfile = try std.fs.cwd().openFile("/etc/localtime", .{});
    const tzinfo = try std.Tz.parse(allocator, tzfile.reader());
    var current_timetype: std.tz.Timetype = undefined;

    for (tzinfo.transitions, 0..) |transition, idx| {
        if (transition.ts > 0 and system_time < transition.ts) {
            current_timetype = tzinfo.transitions[idx - 1].timetype.*;
            break;
        }
    }

    const name = current_timetype.name();
    const offset: i16 = @intCast(@divFloor(current_timetype.offset, 60));

    const System = Timezone.create(name, offset);
    return System;
}
```
This: 

1. Gets a timestamp of the current system time
2. Finds and reads the TZif file symlinked to from /etc/localtime
3. Determines which "transition" is active
4. Returns the name of the timezone and the offset from UTC

(Oh, and the name doesn't actually work, presumably because I haven't figured out how to explicitly declare those three bytes as the format Zig uses for strings.)

All of which is to say that it took me several days and several times as many lines of code to accomplish the same result in Zig that I did in Rust.

And I fucking loved it.

## casting fireball versus using a wand of fireballs

I'm working on a separate blog post about this, but lately I've been trying to learn programming. I've had several kinda-sorta-failed attempts in the past that left me with a passing knowledge of assignments, control flow, etc. But each time I've felt more like I'm using a magic item, in D&D parlance, than casting a spell myself.

Rust's solution feels more like using a magic item. Zig's solution--even though I'm still largely depending on the standard library and zig-datetime module--feels a lot more like I'm learning how to cast a spell. Consider that I had to:

1. Learn how Linux programs check the currently active timezone
2. Learn how to extract that information from a TZif file
3. Combine that information with a fairly opinionated existing module
4. Do all this while also handling allocators, a lack of macros, etc.

I definitely learned a lot more writing the Zig version of this utility than the Rust version. That was important for this step of my journey to learn programming, and a large part of me would like to stick with Zig for the foreseeable future, especially since I'm not worried about finding a job or anything like that.

But.

## ziggy without the stardust

There's no denying that Rust has an advantage when it comes to the developer ecosystem and tooling. Zig is making progress on this front--an official package manager will debut with the formal release of version 0.11 in August--but wrangling with dependencies and the build system is significantly more complex than in Rust.

Zig also has a lot fewer learning resources (though I've found Ziglings particularly useful) and the documentation isn't quite to Rust's standard. That's to be expected! Zig is significantly younger than Rust, hasn't reached version 1.0, and sees regular updates between major point upgrades. This iguana's always evolving.

But there's a difference between learning how a particular function, syscall, or paradigm works and trying to figure out how the fuck I'm supposed to use the Zig package manager. (Or, failing that, use a Git submodule in a way that makes both my primary Git repository and the Zig build system happy with me.)

Contrast that with the wonders of `cargo add` and `cargo build` and it's hard not to think the grass is greener on Rust's side of the fence--even though I generally prefer Zig's syntax, the option to learn more about memory management, and the fact that the resulting binary is a quarter the size even though it's not dynamically linked to a number of libraries like the Rust version of this utility is.

## so where do i go from here?

I told my wife earlier that I almost wish I'd waited another couple of years to get into programming for the Nth time. By then I expect that Zig would've ironed out the problems with its package manager, introduced even more features, and finalized the much-needed switch to using snake-case for standard library functions. (Heh.)

Right now I'm not sure what to do. On the one hand, Zig offers a more bottom-up approach to learning that resonates with me, and I could probably devote some of the time I'd spend exploring other subjects to understanding its build system. On the other hand, Rust would allow me to focus on those other subjects instead.

Part of me wants to continue developing projects in both languages. I'm not on any particular deadlines; I'm prioritizing learning over being productive. But I also know my brain, and it doesn't like undue hassle, especially when it knows there are alternative solutions just a ".rs" instead of a ".zig" away.

At the moment I'm leaning towards developing in Rust while keeping pace with Zig updates and potentially supporting the project in other ways. (Money. I mean with money.) This isn't my preferred solution due to the binary sizes, dynamic linking,and fugly syntax, but at least I won't have to struggle with the build system.

Then again, maybe I'll give learning the Zig build system and package management another week or so, especially since the official 0.11 release isn't that far away.

Gah!