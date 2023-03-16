---
title: "Keeping Perfect From Being the Enemy of Good Enough"
date: 2023-03-14T21:46:59-06:00
draft: false
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

## Me, Myself, and My Impostor Syndrome
I was a Gifted Child&trade;. Throughout my childhood I was constantly told how much potential I had, how I could do anything I wanted, how far I would go. It felt like I was constantly riding a wave of praise and my expectations of myself grew to match what I was being told. Then, high school and college hit me like a brick to the side of the head.

<figure>
    <iframe src="https://giphy.com/embed/hkncjh7jx6dcA" width="412" height="350" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/happy-kid-flying-hkncjh7jx6dcA">via GIPHY</a></p>
    <figcaption>My graceful entrance into adulthood. </caption>
</figure>

Suddenly, the conversation changed, and I progressed from not meeting to wasting that same vaunted potential. All of the childhood praise came back as a taunting chorus in my ears, reminding me that I was never as good as I could be. The internal voice repeating these accusations began to have more say in decisions. I began avoiding asking others for help, lest they also think of me as "wasted potential" or something similar. Any project I started followed a similar cycle: bury myself in research so I could figure out the "perfect" path, give myself a mean case of information overload, and quit. This cycle provided more ammunition for the internal voice bringing me down, which then lead to more unfinished projects, which... you get the idea.

Three particular examples from the last two years come to my mind: my experiments with [Nix](https://nixos.org/) and a stateless development environment, a productivity system I built using [Obisidian.md](https://obsidian.md/), and writing this blog.
### My Home in Disarray
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

### The Least Productive Productivity System
Personal productivity systems and the field of *"personal knowledge management"* are other interests of mine. I am constantly on the lookout for new ways to squeeze 30 hours of productivity out of the 24 hours in a day. Finding `Obsidian.md` and the [Building a Second Brain movement](https://www.buildingasecondbrain.com/) in 2021 seemed like a gift from the gods. Immediately, I set to work cataloging all my knowledge in the most perfect system ever imagined by humankind.

<figure>
    <iframe src="https://giphy.com/embed/RRoN1v9ibIKslCcVCg" width="350" height="350" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/HBOMax-fboy-island-fboyisland-RRoN1v9ibIKslCcVCg">via GIPHY</a></p>
    <figcaption>Yes, I'm sure you can see where this is going.</caption>
</figure>

Before I had collected more than 20 notes in my Obsidian vault, I decided to start creating the perfect taxonomy for tagging and organizing notes. This led to me using the [Dataview](https://blacksmithgu.github.io/obsidian-dataview/) plugin to create the perfectly automated task tracking system and note status tracker. I dumped loads of hours into writing Dataview scripts and testing new vault configurations. I was basically the poster child for [premature optimization](https://www.youtube.com/watch?v=tKbV6BpH-C8).

*Ok, this is the part where you act surprised.*

Within a month, the system sat idle and I was no closer to knowledge nirvana. I'm also not sure how many times I needed to teach myself [Gall's Law](http://principles-wiki.net/principles:gall_s_law): *"a complex system evolves from a basic system"*. I wanted to "pass GO" and step right into the complicated. Since I skipped the most simple and fundamental step of working with a basic system, all my beautiful scripts and ideas were discarded in about a tenth of the time it took to set them up. Of course, I learned my lesson this time and - just kidding, I blamed myself for not creating a good enough system, and promptly walked away from note curation for the better part of a few months.

<figure>
    <iframe src="https://giphy.com/embed/2w6I6nCyf5rmy5SHBy" width="400" height="351" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/SNL-snl-saturday-night-live-2w6I6nCyf5rmy5SHBy">via GIPHY</a></p>
    <figcaption>My Obsidian vault proudly displaying my efforts.</caption>
</figure>

As with the first failed Nix experiment, the notes vault stayed on my mind even after I thought I moved on. I read stories of people creating their successful systems, and blamed myself for not being creative enough to come up with something similar. Eventually, I decided I had to try again. This time, I wanted to test out a brand new concept of "learning from my mistakes." First, I archived all of my previous structures out of sight and focused soley on capturing notes as I felt I needed to. I intentionally avoided any thoughts of tags, statuses, queries, plugins, or library science unless they related to a note I was writing.

I read a great article recently titled ["No Architecture is Better Than Bad Architecture"](https://rogovoy.me/blog/no-architecture). There were a lot of great quotes from the article, but one in particular stood out to me:
>  Another risk is that architecting and structuring your code is a great and fun way to procrastinate.

Working on building out my "perfect productivity system" was all just a way to procrastinate working on a project that would actually fill the vault with useful notes. It was so easy to fall into the trap of thinking that I was making sure that your system will stand the test of time, or you are handling all the edge cases, or *insert other excuse here*. If there was such a thing as a perfect design, software bugs wouldn't exist and Y2K would have just been a [Danish multiple-unit train](https://en.wikipedia.org/wiki/IC3). As in a lot of other areas of life, you won't know the final form for your code or project until it's released into the wild and fulfills the function it needs to. As [Mike Tyson famously said](https://www.sportskeeda.com/mma/news-everybody-plan-get-punched-mouth-how-famous-mike-tyson-quote-originate):
> Everyone has a plan until they have to actually use their project.

Or something like that.

<figure>
    <iframe src="https://giphy.com/embed/l3Ucfk8zqn7NAjLLq" width="480" height="294" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/showtime-mike-tyson-shosports-l3Ucfk8zqn7NAjLLq">via GIPHY</a></p>
    <figcaption>What's your plan?</caption>
</figure>

### I Ain't No Vonnegut
Growing up, I loved reading and this eventually translated into a love of writing. Creative writing assignments in school were my favorite, and I used to dream up stories and write them down in my journal. Somewhere along the way, I started to compare my writing against my favorite authors and my writing habit started to peter out. If I couldn't pick up a pen and immediately write what I thought of as the "perfect" novel with the epic adventure of Tolkien, the emotional impact of Hemingway, Vonnegut's humor, and Sanderson's worldbuilding, was it even worth trying?

<figure>
    <iframe src="https://giphy.com/embed/PLJqELt6udsdQhjTgA" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/lifetimetv-lifetime-youonlifetime-PLJqELt6udsdQhjTgA">via GIPHY</a></p>
    <figcaption>My drafting process</caption>
</figure>

I would be delusional if I thought that I was the only person in the world who had ever given up a writing hobby because of impostor syndrome. Even knowing that I am far from the only person struggling with self-esteem induced writer's block, I have struggled get back into writing. Despite reading about how some of [my idols have discussed their first drafts](https://www.writingroutines.com/famous-writers-on-first-drafts/) I remained afraid of any creative writing endeavor. There are countless daydreams, stories, and adventures that have been discarded from my memory because I refused to let myself create anything imperfect.

<figure>
    <iframe src="https://giphy.com/embed/3oriNWeb2gzrZufbZ6" width="480" height="480" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/br-3oriNWeb2gzrZufbZ6">via GIPHY</a></p>
    <figcaption>The later stages of my creative writing idea lifecycle</caption>
</figure>

Reading all of the past tense in that paragraph might lead you to ask: "What did you do to get over it? How did you master that fear?"

The short answer is: "lol, I wish." There is still a voice in my head that demands perfection and bemoans the inadequacy of my output. I'm still not a fan of my writing. Sometimes, I still let that voice make the decisions as to when and how much I work on writing projects.

But as with the other examples from this post, I have worked on narrowing my focus to the smallest of *"good enough"* steps to build up to larger progress closer to my idea of *"perfect"*. This blog is one of those steps. Giving myself a space to be imperfect and find my voice again has given me courage to create more (especially to my current audience of my wife and no one else). I am still working on a method to prevent myself from entering a cycle of endless drafting. Setting a deadline for this post to be published and telling my "audience" about my plans helped get this one out the door instead of staying as an outline in my notes.

Admittedly, the publishing date did slip by a few days because of some external events. But for the first time in close to a decade, I am writing regularly again. Rather than reaching for that all-elusive perfection, I am allowing myself to publish something that is "good enough."

<figure>
    <iframe src="https://giphy.com/embed/bcrOR2stk6tKIxqPOZ" width="480" height="268" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/reaction-bcrOR2stk6tKIxqPOZ">via GIPHY</a></p>
    <figcaption>Me telling me that the post won't be ready by the deadline I set for myself.</caption>
</figure>

## "Hey Google, How Do You Land a Plane?"
Class, what did we learn today?

Besides the fact that I need therapy for my self-esteem, we've learned about the **power of allowing something to be "good enough."** Again, I'm not saying that striving for perfection is something to be shunned. Nor am I saying that I will now settle and not seek growth or improvement. But unless scientists figure out a way to instantly transfer knowledge to individuals, I am going to have to continue learning through failure and repetition. I am going to have to be content with producing things that are "good enough".

<figure>
    <iframe src="https://giphy.com/embed/PmpA5ohOUl1xC" width="480" height="268" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/funny-weird-alien-PmpA5ohOUl1xC">via GIPHY</a></p>
    <figcaption>Side note: if Neuralink can figure out that instant download thing, hit me up Ol' Musky!</caption>
</figure>

Trade-offs are an essential part of life. Acknowledging that something is "good enough" for publish or release is recognizing the diminishing returns of improvement from further iteration without feedback. That assumes that further iteration without feedback results in *any improvement at all*. Realizing something is "good enough" is not always settling for less. It is laying a foundation for raising the standard of "good enough" down the road.

How am I actually keeping the idea of perfection from stopping any forward progress? From my experiences above, I've found a few strategies that have been working well:
1. Make project goals granular and focused on single "units" of work (e.g. migrating one set of dotfiles to Nix versus the entire system)
2. Set due dates for tasks. As mentioned in my [2023 Annual Review](https://alexandersauceda.dev/posts/annual-review/#the-winding-path), I am frequently a victim of over-planning and this helps to force *action over plans*.
3. Find at least one win. It's all too easy to focus on the negatives, but by forcing myself to acknowledge at least one positive from every effort sets the stage for more imperfect progress. Noting one win or good thing also helps to focus on the things that I can build on later.
4. Sharing my work. Hiding my projects and progress is often a symptom of being embarrassed that it isn't *perfect*. Allowing myself to talk about what my projects, even if they are unfinished, has provided incentive to continue working on them.
5. Using something like the ["Rule of Three"](https://en.wikipedia.org/wiki/Rule_of_three_(computer_programming)) when I start to focus a little too much on the "clean up" tasks before a product is ready. It is a handy benchmarking mechanism to make sure I'm procrastinating by "tidying something up" rather than making actual progress.

Watch out as I submit some shitty code to my personal GitHub repos. Keep an eye out for more lackluster blog posts. These efforts may all be far from *perfect*, but maybe they will be *good enough* to continue improving.


<figure>
    <iframe src="https://giphy.com/embed/3b8GDuWKKGZIyACVM0" width="480" height="480" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/cbc-schittscreek-schitts-creek-3b8GDuWKKGZIyACVM0">via GIPHY</a></p>
    <figcaption>Tell em, Moira!</caption>
</figure>
