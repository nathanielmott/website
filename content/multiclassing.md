+++
title = "what does 'multiclass into wizard' mean, anyway?"
date = 2023-07-16
draft = true
slug = "what-does-multiclass-into-wizard-mean-anyway"
+++

*tl;dr: *

So I've been learning how to program. Again.

I've tried to learn how to write software a few times over the years. In 2019 I downloaded Apple's Swift Playgrounds app for the iPad, in 2021 I made my way through Paul Hudson's excellent "100 Days of SwiftUI" course, and in 2022 I bought Dr. Angela Yu's incredibly popular introductory Python course on Udemy. On at least two occasions I also tried to read through "The Rust Programming Language."

Each of those attempts resulted in some incremental progress. Swift Playgrounds introduced me to basic programming concepts like variables, functions, and control flow; "100 Days of SwiftUI" showed me how those building blocks could be assembled into working software; and Yu's course helped me realize I didn't want to learn Python. That, combined with seemingly everyone's insistence that Rust is the future, led me to what Rust developers call "The Book" over and over and over again.

I finally read the entirety of "The Book" for the first time at the end of June. Then I read through it again, but this time I read Brown University's version, which adds quizzes to each section. I also made my way through "Rust in Action" by Tim McNamara to help put these concepts in context and give me a basic understanding of what the fuck people are talking about when they talk about "systems programming." (And, of course, watched entirely too many videos about Rust on YouTube.)

That's a lot of reading! I probably should have started a project alongside my second pass through "The Book," but I didn't want to repeat my previous efforts by completing a few tutorials and then giving up when it came time to write something of my own because I didn't fundamentally understand what I was doing. So I internalized as much about Rust as I could and then wrote a "toy" combining metadata about a given file as well as the number of paragraphs and words within.

And that's when I realized I'm not crabby enough for the crab club... yet.

---

Let's get a few things out of the way, internet:

1. No, I didn't ragequit after running into problems with the borrow checker or lifetimes, and my experience with the Rust compiler was generally positive
2. Cargo is, at least in my limited experience, amazing
3. I understand the importance of memory safety and think it makes sense for larger projects to take advantage of Rust's capabilities in that regard
4. I'm open to using Rust in the future and am not saying you shouldn't use it now

Carrying on.

Rust has everything a new programmer might want: mature tooling, an extensive standard library, and a massive ecosystem that builds upon those capabilities. Apple had "there's an app for that"; Rust has "there's a crate for that." These are all good things, and it made writing the toy I mentioned earlier fairly easy.

