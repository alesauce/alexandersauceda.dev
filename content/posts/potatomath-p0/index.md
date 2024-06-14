---
title: "Potatomath P0"
date: 2024-06-14T12:53:31-06:00
draft: false
tags:
- engineering
- rust
- nix
- potatomath
---

## Project Output
Before I say anything else, here is the link to the repo on GitHub {{< icon "github" >}} [Potatomath project repo](https://github.com/alesauce/potatomath).

## Background
Some time ago, a friend of mine posited a thought-provoking question: if he only bought potatoes to feed his family of five, what was the minimum dollar amount that he could feed his family for in a given month? He asked this question to a team of engineers, and we promptly spent an entire lunch break discussing the various foods that could be substituted for the potatoes (rice was our top pick) and different ways that you could improve the process to calculate this cost to optimize which food you're buying from month to month. From this simple, dumb question spawned this project idea: create an application that can calculate the approximate cost to feed a family of a given size with a given food item.
{{< alert >}}
**This project is not medical advice and is for entertainment purposes only**. I am not a medical professional nor am I even qualified to figure out my own dietary needs. Please don't use anything in this project to actually figure out how to feed anyone.
{{< /alert >}}

Now, I am a known scope-creeper. I can easily take a request to make a calculator that adds 2+2 and then start designing a Wolfram-Alpha competitor. It's a problem that I am actively working on, and this seems like a great opportunity to take a step in the right direction. I spent a few minutes scribbling down ideas for this project and then separated those ideas into prioritized tiers ranging from P0 (first to implement) to P5 (least important).
<figure>
    <iframe src="https://giphy.com/embed/iF8IaDx2N6vfzS2k52" width="480" height="271" style="" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/parksandrec-parks-and-recreation-rec-peacocktv-iF8IaDx2N6vfzS2k52">via GIPHY</a></p>
    <figcaption>Me designing "fizzbuzz" </figcaption>
</figure>

## Prioritized tiers
### P0
This project is an opportunity to learn Rust from scratch, and the P0 reflects that fact. It's essentially a "Hello, World!" program with extra steps. My goals with P0 were:
- Rust package that builds on my machine.
- Package that can take user input of a number of people and calculate the cost to feed that amount of people.
- Should only use fixed values where a later priority level would require user input or an API call. Planned uses of fixed/harcoded values:
	- Price of a 5 pound bag of potatoes. "P1: Allow the user of the package to pick a type of food from a list and fetch the price from the Internet." Given that requirement, I'll use a fixed value as a placeholder for now and retrieve the price from an API later.
	- Calories per person. "P3: Allow users to modify the required average caloric value for the given family size." I'll use `2000` as a hardcoded value for now and as the default in the future.
	- Calories per 5 pound bag of potatoes.  "P1: Allow the user of the package to pick a type of food from a list and fetch the price from the Internet." I think calories per unit is something I should get from an API or database when I retrieve the price as well. This would make the price calculations actually make sense as well.
### P1 through P5
These points are all subject to change because this is a dumb project that I'm doing for fun when I have time outside of my full time job and various other responsibilities. I tried to write all of my priority levels as one sentence success criteria to keep my overall goals simple. These will likely get fleshed out more as I actually devote more thought to them.
- P1: Allow the user of the package to pick a type of food from a list and fetch the price from the Internet.
- P2: Create a front end for this application and host it somewhere that is publicly accessible.
- P3: Allow users to modify the required average caloric value for the given family size.
- P4: Allow users to specify individuals for the caloric calculations (e.g. 3 kids, 2 adults) and modify caloric calculations for both. Gotta support people going into bulking season.
- P5: Allow users to get prices in non-US currencies. I'm a man of the world, after all.

<iframe src="https://giphy.com/embed/3o8dFtBRuym9wGc024" width="480" height="269" style="" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/baio-man-of-the-world-3o8dFtBRuym9wGc024">via GIPHY</a></p>

## Development process
### Getting started
I decided to use [Crane (a Nix library for building Cargo projects)](https://github.com/ipetkov/crane) for my development environment and build system. The Nix language, build system, and configuration management tooling has a growing influence on my life outside of work. [Crane's quick start flake](https://github.com/ipetkov/crane/blob/master/examples/quick-start/flake.nix) came in handy as I got started. After reading a few online tutorials and some of the wonderful [Rust book](https://doc.rust-lang.org/book/), I came up with this basic plan of attack:
- Take in a string input from `std::io` from the user at the command line
- Convert that to an `i32`
- Multiply that number of people by a hardcoded value of potatoes price and how much is needed to feed people
- Output the correct number
### Baby's first error
My first error came quickly. I started off trying to use `unwrap()` on its own to convert the `String` input from the user to an `i32`, which gave me a wonderful stack trace that told me in no uncertain terms that I wasn't in Python-land anymore. Not to worry though, because I've been programming just long enough to know how to use search engines ([Perplexity](https://www.perplexity.ai/) in this case).

[The summary and sources from this search](https://www.perplexity.ai/search/rust-convert-string-qUbunjfOQyaZvEQwPuJa8Q) recommended using `unwrap` with `match` logic to handle errors rather than just letting the `unwrap()` function go straight to panicking. After using match to output the malformed input that was causing the panic, I realized that I needed to use the `trim()` function to strip whitespace. After this fix, I was greeted with the following output and felt like the most brilliant human on Earth:
```
Please input the number of people you are feeding.
5
You input 5
 people.
Your total cost per month is: $75. Don't forget the multivitamin!
```
### Finishing up
[This first commit](https://github.com/alesauce/potatomath/pull/1/commits/bb4e61b2a19c716da9f4d398b6b0cdd0187329cf) was basically just one long `main()` function in my new Rust project. I wanted to feel like a real engineer and add some unit tests, which meant that I needed to break out some of the logic into separate, smaller functions. I did this over three different commits ([first](https://github.com/alesauce/potatomath/pull/1/commits/bb4e61b2a19c716da9f4d398b6b0cdd0187329cf), [second](https://github.com/alesauce/potatomath/pull/1/commits/87aaa1247e9bfc9bcd781abd13082e3df37ce193), and [third](https://github.com/alesauce/potatomath/pull/1/commits/82d146407ef6a571fe4814f342a1addaf01f8094)) as I worked on the module more and found better ways to split up functionality to add useful testing. Adding unit tests inside the same module as the function that I was writing was a new concept to me, but I do enjoy having all the functionality for code wrapped up in one file instead of having to toggle between different directories to review the function that I'm trying to test.

The weird formatting of the initial output also bugged me to no end. After another search and trying out a few things, it turned out it was because I was just using the raw String value from the user for this instead of formatting (e.g. using `trim()`).
## Next steps
I am going to start working on the P1 as I have time. Looking at my calendar for the summer, I'm a little worried at how infrequently "as I have time" is going to come, but I will just have to try to keep my goals realistic and try to chip away at this a bit at a time. So far, some of the goals I have for P1 after merging P0 outside of the one sentence statement above:
- Write a README for the GitHub repo
- Create a CI job using GitHub Actions and the Nix flake
- Refactor potatomath flake to use flake-parts or Nix glue code instead of flake-utils
- Potentially change P1 of potatomath to use a database that I create and populate to store and retrieve the food prices from instead of API calls

Thanks for reading!
