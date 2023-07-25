+++
title = "what does 'multiclass into wizard' mean, anyway?"
date = 2023-07-24
draft = true
slug = "what-does-multiclass-into-wizard-mean-anyway"
+++

*tl;dr: learning to program but with a d&d mindset*

I said in [another post](https://www.nathanielmott.com/rust-or-zig-july-2023-idk/) that "lately I've been trying to learn programming" following "several kinda-sorta-failed attempts in the past that left me with a passing knowledge of assignments, control flow, etc" but left me feeling "more like I'm using a magic item, in D&D parlance, than casting a spell myself." The [about](/content/pages/about.md) page on this site says "Nathaniel Mott is a bard looking to multiclass into wizard."

Let me expand on that.

<!-- more -->
## struggling to learn cantrips

I've tried to learn how to write software a few times over the years. In 2019 I downloaded Apple's Swift Playgrounds app for the iPad, in 2021 I made my way through Paul Hudson's excellent "100 Days of SwiftUI" course, and in 2022 I bought Dr. Angela Yu's incredibly popular introductory Python course on Udemy. On at least two occasions I also tried to read through "The Rust Programming Language."

Each of those attempts resulted in some incremental progress. Swift Playgrounds introduced me to basic programming concepts like variables, functions, and control flow; "100 Days of SwiftUI" showed me how those building blocks could be assembled into working software; and Yu's course helped me realize I didn't want to learn Python. That, combined with seemingly everyone's insistence that Rust is the future, led me to what Rust developers call "The Book" over and over and over again.

I finally read the entirety of "The Book" for the first time at the end of June. Then I read through it again, but this time I read Brown University's version, which adds quizzes to each section. I also made my way through "Rust in Action" by Tim McNamara to help put these concepts in context and give me a basic understanding of what the fuck people are talking about when they talk about "systems programming." (And, of course, watched entirely too many videos about Rust on YouTube.)

That's a lot of reading! I probably should have started a project alongside my second pass through "The Book," but I didn't want to repeat my previous efforts by completing a few tutorials and then giving up when it came time to write something of my own because I didn't fundamentally understand what I was doing. So I internalized as much about Rust as I could and then wrote a "toy" combining metadata about a given file as well as the number of paragraphs and words within.

I ultimately ended up building two versions of this toy: one in Rust and one in Zig.

## trying a first-level spell

As I put it in the previous post:

> Rust's solution feels more like using a magic item. Zig's solution--even though I'm still largely depending on the standard library and zig-datetime module--feels a lot more like I'm learning how to cast a spell. \[...\] I definitely learned a lot more writing the Zig version of this utility than the Rust version. That was important for this step of my journey to learn programming, and a large part of me would like to stick with Zig for the foreseeable future, especially since I'm not worried about finding a job or anything like that.

Let me be clear: I would love to continue with Zig. But after spending a few more days grappling with the package manager--the alternative to which is messing around with Git submodules, which has led to mixed results, in my experience--I'm firmly leaning towards using Rust until Zig is closer to that hallowed 1.0 milestone.

This wasn't just because of the build system, though. It's also because I realized that part of my problem had little to do with Rust itself and everything to do with the fact that Rust makes me feel stupid in a way that Zig doesn't. (Even though I can't figure out something as simple as managing dependencies.)

That isn't meant as an indictment of Rust--yet. My current stance is that Rust is *complicated*, and rather than recognizing that, my ego got in the way. So I've decided to put that aside for now--or at least attempt to--and engage with the staggering amount of learning materials available for programming in Rust. 

(I do think I would ultimately *prefer* Zig to Rust. The lack of hidden control flow, ability to achieve most common tasks with just the standard library, and syntax / grammar / whatever you want to call it all seem to make it better suited to my brain. But I'm open to feeling differently about this as I continue to learn Rust.)

## multiclassing, not dual-classing

Here's a throwback for people who played Advanced Dungeons & Dragons 2nd Edition: I'm specifically looking to multiclass into wizard, not [dual-class](https://adnd2e.fandom.com/wiki/Multi-Class_and_Dual-Class_Characters_(PHB)) into it. 

I like writing. Enough people have paid me to do it that I think I'm decent at it. A combination of factors--the fact that I get lost in codebases spread across more than a handful of files among them--leads me to believe that I wouldn't like nor be decent at programming in a way conducive to professional success.

Maybe that, too, will change. But for now I'm content to research new spells on the side. Bards are notorious for being jacks of all trades and masters of none. Why should this particular bard be any different?