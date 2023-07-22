+++
title = "what i use: july 2023"
date = 2023-07-21
draft = true
slug = "what-i-use-july-2023"
+++

*tl;dr: look at the most popular tools on /r/unixporn lol*

A few weeks ago I wrote about switching to Linux for the vast majority of my computer  usage. (Compusage?) That post was light on details about my setup--I got in the obligatory Arch mention, said that "pretty much everything on this system is here because I want it to be," and then neglected to mention what I'm actually using.

I like reading these kinds of posts, so fuck it, let's post one.

## core

I'm not using a full-on desktop environment like Gnome or KDE Plasma. Instead, I'm using a tiling window manager for Wayland called Hyprland. It's proven easy to configure--even though it doesn't officially support Nvidia hardware--and I'm pleased with the results. It's both aesthetically pleasing and productive.

My terminal emulator of choice is Alacritty because Kitty doesn't officially support terminal multiplexers like tmux. Alacritty is similarly easy to customize, and even though I'm still learning how to use tmux, I have it running almost constantly. The only other user-facing software guaranteed to be running is waybar. 

I write these blog posts, program, and manage configuration files in Helix, the "post-modern text editor" that offers modal editing similar to Vim but flips the primary methods of manipulating text on its head. It also supports LSPs and tree-sitter grammars for most popular languages out of the box, so minimal setup is required.

Firefox is my primary browser. (I have to use Chromium to manage my keyboard's firmware, unfortunately, but such is the modern web.) I recently discovered the vimium plugin that has allowed me to keep my hands on my keyboard even more often than before, which is both incredibly convenient and slightly cursed.

## command line tools

I keep up with the news for work using Newsboat. I like it, but I don't love it, and I've considered exploring alternative solutions or developing my own RSS reader.

Gitui has definitely helped me wrap my head around Git, which is in turn revelatory and maddening, especially when I'm trying to use a submodule because Zig's package manager is inscrutable. (More on that at a later date.) I find its interface much easier to use than either Git's CLI or GitHub's desktop app for macOS.

And of course there are all the requisite utilities for modern Linux usage:

* bat
* exa
* hyperfine
* fd
* ripgrep
 
## graphical tools

I use Thunar when I don't feel like crawling through directories on the command line, read my ebooks with Calibre, and socialize with Discord, which remains a pile of hot garbage despite a recent update to the Linux version of the app. (I'd replace it with an IRC or Matrix client but the network effect is real.)

I wasn't sure if I should include Rofi in this category or the "core" category. If I knew how to use it better, I suspect it would in the latter. For now I mostly use it to launch the previously mentioned graphical tools, and since I don't use many of those, it's definitely a nice-to-have rather than a must-have. I'll learn more about it later.

## misc

* Swaylock
* SDDM

