+++
title = "thinkin' bout helix and neovim"
date = 2023-08-24
draft = false
slug = "thinkin-bout-helix-and-neovim"
+++

*tl;dr: whoever thought a text editor should be configured in lua was mad*

I've been using Helix for the last few months, and for the most part, it's great. Configuring it with Rust, Zig, and Markdown has been a snap. Changing various settings to suit my preferences has been a breeze. Launching it with the beloved "hx" command has been a third metaphor for a thing being easy. And, most importantly, Helix's approach to manipulating text makes far more sense to me than Vim and NeoVim's does.

And yet.

<!-- more -->

In the course of writing that paragraph, I've already used a NeoVim feature that Helix doesn't offer, at least at time of writing: spell checking. (And, by extension, the ability to add specific words to "the list of words NeoVim won't complain about.) Soon I'll set up others--including a file tree, an integrated terminal, and more--that either haven't been added to Helix yet or show no signs of ever being added to the editor.

This feels like my difficulty with choosing between Zig and Rust. One is an up-and-comer that makes a lot of smart choices (or, at least, choices that seem like the ones I would make) and the other is an established utility that can be used to get shit done even if there are pain points aplenty. When it came to Zig and Rust, I chose the latter. Now I'm looking to see if I'll do the same with NeoVim and Helix.

Let's talk about why I'm in this position.

## 'selection-verb' > 'verb-selection'

Helix operations work by selecting a chunk of text--be it a word, line, or some other unit--and then acting upon it. If I want to delete a specific word, for example, I type "w" to select it and "d" to delete it. This is exactly how I've already thought about the problem--I saw a word that I wanted to delete so I deleted it.

NeoVim works in the opposite direction. If I want to delete a specific word, I type "d" to delete something and then "w" to make it clear that I want to delete the word under my cursor. This strikes me as somewhat nonsensical and error-prone (thank goodness "undo" is just a "u" away!) but it's a control NeoVim inherited from its predecessor so whatever.

I'm sure that some people learning a modal editor will naturally prefer the latter approach, and that it's not just nostalgia or muscle memory preventing them from embracing Helix's approach, but I suspect that's a small portion of the population. Helix's approach is more logical, less error-prone, and my favorite by far.

## editing toml > writing lua

Imagine telling someone they would have to familiarize themselves with a whole-ass programming language just to make Microsoft Word suit their preferences. You'd be laughed out of the room, and I'd put the odds of someone actually doing that at zero.

I don't think many of those people would prefer to use a markup language, but at least some would be more willing to do that than learn Lua. (I'm currently learning Rust, and even I prefer configuring Helix with TOML over configuring NeoVim with Lua.)

I've experimented with a few NeoVim configs. NvChad is nice, and I might end up going back to it at some point, but I found that the way the project was set up made it far too frustrating to update to suit my own preferences. LazyVim is easier to configure in some ways, but only because it hides a lot of options away, and that can be frustrating too.

(See [this](https://www.youtube.com/watch?v=aiSI6vdZWgE) video from Luke Pighetti to show just how stark the difference between configuring NeoVim and Helix can be.)

## but where is the goddamn file tree?

Or, "that complexity affords additional features that I'm looking for."

NeoVim doesn't feature a file tree by default; neither does Helix. The difference is that at least two NeoVim plugins offer easy access to a file tree; exactly zero Helix plugins do the same. Because there are no Helix plugins. What you see is what you get.

This "batteries included" approach is great--until you want to do something that requires additional batteries. Then you can either submit a pull request that either A) won't be approved or B) will take quite a while to make its way into a stable release or... you can decide that feature isn't that important to your workflow after all.

> I just now realized this also applies to Zig and Rust. The former has a "batteries included" standard library that I really appreciate, but when it's time to use something that doesn't exist within the standard library, your options are incredibly limited. Rust has an anemic standard library that allows popular crate maintainers to hold many Rust devs hostage to support an RFC that's over a year in the making, but at least those options are available! Between "make your own battery" and "go to the battery store," at least for now, I'm pretty much stuck with the latter.

So NeoVim has a file tree. And I prefer the way it shows errors / warnings from my LSP. And it has spell checking. And I can experiment with other plugins that seem like they could improve my current workflow. All I have to do is go to the battery store.

Do I want to go to the battery store? No. Do I want to have to learn a new language because it's the only way to install those goddamn batteries? Not really! But I want some of these batteries, damn it, so I'm going to have to at least make an effort.

> Which, again, just like Zig and Rust. Ugh.

## so where do i go from here?

I'm going to devote a day--an entire goddamn day, or at least as much of one as I possibly can--to setting up NeoVim and getting my brain used to Vim motions instead of Helix motions. After that, I'll continue to evaluate NeoVim, then I'll make a decision.

I suspect I'll end up where I am with Zig and Rust: Longing for the day the former is ready for me, but accepting the latter in the meantime, and perhaps learning to love it along the way.
