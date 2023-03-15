---
title: "Keeping Perfect From Being the Enemy of Good Enough"
date: 2023-03-14T21:46:59-06:00
draft: true
tags:
- engineering
- nix
- writing
- impostor syndrome
- venting
---

## Are We All a Little Masochistic?
"Perfect" is a loaded word. It conjures up images of the pinnacle of something, the most desired form - the perfect dish, the perfect photo, the perfect solution, etc., etc., etc. Our society venerates stories and examples of what we collectively perceive as perfection, and it becomes the standard we measure ourselves against.

On the other hand, "good enough" often gets a level of disrespect it doesn't deserve. "Good enough" sounds like "complacency", and "complacency" is almost a dirty word. We can't let "good enough" happen. Whatever we are working toward must meet that standard of perfection, or it wasn't worth doing in the first place. In the meantime, we cause ourselves untold amount of pain and anguish by measuring ourselves to that standard of "perfection". It's like picking up a basketball with the sole goal of beating Kobe in a 1 on 1.

<figure>
    <iframe src="https://giphy.com/embed/7rqiYoGahYG3K" width="480" height="265" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/basketball-kobe-bryant-7rqiYoGahYG3K">via GIPHY</a></p>
    <figcaption> You really wanna try your luck in that 1 on 1?</caption>
</figure>

To be clear, I'm not saying that the pursuit of growth and improvement is not worthwhile. I'm not saying that striving for perfection will invariably lead you to wrack and ruin. But I am saying that sometimes, "good enough" is okay. Sometimes, "good enough" might just be perfect for the situation.

### Me, Myself, and My Impostor Syndrome
I was a Gifted Child&trade;. Throughout my childhood I was constantly told how much potential I had, how I could do anything I wanted, how far I would go. It felt like I was constantly riding a wave of praise and my expectations of myself grew to match what I was being told. Then, high school and college hit me like a brick to the side of the head.

<figure>
    <iframe src="https://giphy.com/embed/hkncjh7jx6dcA" width="412" height="350" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/happy-kid-flying-hkncjh7jx6dcA">via GIPHY</a></p>
    <figcaption>My graceful entrance into adulthood. </caption>
</figure>

Suddenly, the conversation changed, and I progressed from not meeting to wasting that same vaunted potential. All of the childhood praise came back as a taunting chorus in my ears, reminding me that I was never as good as I could be. The internal voice repeating these accusations began to have more say in decisions. I began avoiding asking others for help, lest they also think of me as "wasted potential" or something similar. Any project I started followed a similar cycle: bury myself in research so I could figure out the "perfect" path, give myself a mean case of information overload, and quit. This cycle provided more ammunition for the internal voice bringing me down, which then lead to more unfinished projects, which... you get the idea.

Three particular examples from the last two years come to my mind: my experiments with [Nix](https://nixos.org/) and a stateless development environment, a productivity system I built using [Obisidian.md](https://obsidian.md/), and writing this blog.
## My Home in Disarray
About a year ago, I discovered the magic of [dotfiles](https://www.youtube.com/watch?v=r_MpUP6aKiQ). As an experienced procrastinator and tinkerer at heart, I immediately set about tweaking my system's configuration. When researching dotfile repositories from other people, I became obsessed with the concept of the perfect single-command ("one-liner") automated system install and configuration. After finding more and more repos with one-liner installations, the only goal that seemed worthy to me at the time was a one-liner installation of my own.

I spent SO MUCH TIME working on that perfect one line installation - compiled notes, starred GitHub repos, bookmarked blog posts and documentation websites. My research lead me to the Nix ecosystem and I began to get visions of configuration glory. After a few weeks, I took stock of my progress and proudly reviewed the reference materials I had collected. "This is it," I thought, "time to build the most beautiful automated stateless infrastructure system known to humankind." I opened up a text editor on my computer to start working and promptly drew a blank. :grimacing:

<figure>
    <iframe src="https://giphy.com/embed/U5IRLxDIlJ7zhV9t78" width="480" height="267" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/AmericanDadTBS-americandad-americandadtbs-U5IRLxDIlJ7zhV9t78">via GIPHY</a></p>
    <figcaption>Don't tell my wife how I got the bruise on my forehead.</caption>
</figure>

I had SO MANY REFERENCES, but rather than providing the perfect plan to move forward I had instead overloaded myself with information. *(Getting flashbacks to the intro? Same.)* When I took a step back and reviewed my system, I realized that I didn't have a single dotfile in version control. My environment home configuration was hastily patched together and differed from machine to machine. By this point, I had become so overloaded with potential pitfalls and things to do with Nix that I became paralyzed. In true Alexander fashion, I decided that if I couldn't do it perfectly, I wouldn't do it at all, and instead began using a program called [chezmoi](https://www.chezmoi.io/) to manage my dotfiles. Initially, I told myself that it made more sense for what I wanted, but I knew it was because I had scared myself off from trying Nix.

<figure>
    <iframe src="https://giphy.com/embed/l2QEgWxqxI2WJCXpC" width="329" height="300" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/fire-vacuum-this-is-fine-l2QEgWxqxI2WJCXpC">via GIPHY</a></p>
    <figcaption>Cleaning up the mess of notes and bookmarks on my desktop.</caption>
</figure>

I persisted with chezmoi until March 2023. After installing [Pop! OS](https://pop.system76.com/) on my home desktop and coming face to face with another slog through finding and installing missing packages and tools, I decided to give Nix another try. Thanks to the beautiful [Zero-to-Nix website](https://zero-to-nix.com/) built by [Determinate Systems](https://determinate.systems/), I was able to get started with Nix flakes and home manager in a [new repo](https://github.com/alesauce/nix-dotfiles). This time instead of setting my only goal as the "one liner", I started smaller. My first task was to migrate my Neovim configuration, and only my Neovim configuration, to be managed by Nix. There were a few issues I had to work through to get [Nix home-manager](https://nix-community.github.io/home-manager/) running, but I got the new flake set up and working on multiple machines within two days. Attempt #1 to install Nix resulted in a load of unused references that were meant to build up to the perfect solution. Attempt #2 resulted in success and new knowledge precisely because I allowed myself to set a target that was "just good enough".

## The Least Productive Productivity System
Personal productivity systems and the field of *"personal knowledge management"* are other interests of mine. I am constantly on the lookout for new ways to squeeze 30 hours of productivity out of the 24 hours in a day. Finding `Obsidian.md` and the [Building a Second Brain movement](https://www.buildingasecondbrain.com/) in 2021 seemed like a gift from the gods. Immediately, I set to work cataloging all my knowledge in the most perfect system ever imagined by humankind.

<figure>
    <iframe src="https://giphy.com/embed/RRoN1v9ibIKslCcVCg" width="350" height="350" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/HBOMax-fboy-island-fboyisland-RRoN1v9ibIKslCcVCg">via GIPHY</a></p>
    <figcaption>Yes, I'm sure you can see where this is going.</caption>
</figure>

Before I had collected more than 20 notes in my Obsidian vault, I decided to start creating the perfect taxonomy for tagging and organizing notes. This led to me using the [Dataview](https://blacksmithgu.github.io/obsidian-dataview/) plugin to create the perfectly automated task tracking system and note status tracker. I dumped loads of hours into writing Dataview scripts and testing new vault configurations. I was basically the poster child for [premature optimization](https://www.youtube.com/watch?v=tKbV6BpH-C8).

*Ok, this is the part where you act surprised.*

Within a month, the system sat idle and I was no closer to knowledge nirvana. I'm also not sure how many times I needed to teach myself [Gall's Law](http://principles-wiki.net/principles:gall_s_law): *"a complex system evolves from a basic system"*. I wanted to "pass GO" and step right into the complicated. Since I skipped the most simple and fundamental step of working with a basic system, all my beautiful scripts and ideas were discarded in about a tenth of the time it took to set them up. Of course, I learned my lesson this time and - just kidding, I blamed myself for not creating a good enough system, and promptly walked away from note curation for the better part of a few months.


<figure>
    <iframe src="https://giphy.com/embed/2w6I6nCyf5rmy5SHBy" width="480" height="351" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/SNL-snl-saturday-night-live-2w6I6nCyf5rmy5SHBy">via GIPHY</a></p>
    <figcaption>My Obsidian vault proudly displaying my efforts.</caption>
</figure>

As with the first failed Nix experiment, the notes vault stayed on my mind even after I thought I moved on. I read stories of people creating their successful systems, and blamed myself for not being creative enough to come up with something similar. Eventually, I decided I had to try again. This time, I wanted to test out a brand new concept of "learning from my mistakes." First, I archived all of my previous structures out of sight and focused soley on capturing notes as I felt I needed to. I intentionally avoided any thoughts of tags, statuses, queries, plugins, or library science unless they related to a note I was writing.
