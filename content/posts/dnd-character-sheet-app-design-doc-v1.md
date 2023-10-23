---
title: "Creating a New DnD Character Sheet Application"
date: 2023-10-20T18:51:29-06:00
draft: false
tags:
- dungeons-and-dragons
- projects
- open-source
- development
- engineering
- golang
---

# Problem Statement
It's often very clunky and manually taxing to manage a Roll20 character sheet. Especially for first timers, it can be downright challenging. It also adds a lot of mental overhead from switching back and forth and reviewing the rule sheet. If there was a way to automatically manage some of these rules in the background (perhaps with RAW defaults at first, then find ways to make those configurable), it would significantly alleviate the overhead in creating and managing a new character for all players, especially new players.

# Solution
## High-Level Overview
I will create a web app with a Python backend and a Svelte frontend utilizing the Model-View-Controller paradigm. I am choosing Python to try using more idiomatic tooling within the Python ecosystem, versus the Amazon-internal tooling I'm used to. The primary focus at first will be the backend, so a more complete design of the frontend is coming later. I would also eventually like to look at using [Tauri](https://tauri.app/) to bundle a desktop app as well with a local-first approach. The data storage layer will utilize [Surreal DB](https://surrealdb.com/), because it is feature-rich, is being actively developed, and is multi-paradigm for ultimate flexibility.

## Requirements

### Table Stakes from Roll20
- [ ] Attributes
- [ ] Modifiers
- [ ] Proficiency bonus
- [ ] Saving throws
- [ ] Skills
- [ ] Passive wisdom
- [ ] Proficiencies and languages
- [ ] Coinage
- [ ] Items, equipped or not, and weight
	- [ ] If items have attacks: item attacks, modifiers, damage type
- [ ] Feats
- [ ] Resources
- [ ] Short/long rest replenishment
- [ ] AC
- [ ] initiative
- [ ] speed
- [ ] freeform bio
- [ ] personality traits
- [ ] ideals
- [ ] bonds
- [ ] flaws
- [ ] spell levels
- [ ] spell slots
	- [ ] free-form spell additions
	- [ ] prepared spells
	- [ ] spell slots of levels

### Ideas for Improvement
1. Automatically pull info from standard spells and items to fill out free form areas
	- Use the 5E API as the baseline for classes, attributes, etc.
	- Import custom rules, classes, weapons, etc. as JSON files with validations
3. Automatically track temporary stats - e.g. hp, speed, etc. in wild shape
4. Automatic attacks with items - e.g. hit die, then roll damage automatically with modifiers
5. Calculate rolls with a proficiency or skill - e.g. "perform an investigation check"
6. Able to export all application data (e.g. filled out character sheet) to a standard data format, most likely JSON

### Data Access Patterns
Before I decide on the datastore I want to use, I think it'll be helpful to think about some of the potential data access patterns I want to implement. This list will obviously not be exhaustive and is subject to change as the app develops. Creating a baseline of potential access patterns will provide some datapoints on what kind of datastore I need and what potential features I need. This is probably also worth asking a few people for input, so it's not just my thoughts going into it.

Anyway, without further ado, the data access patterns identified during brainstorming:
1. Range and equivalence queries over numerical values 
	1. Spell levels
	2. Damage output for weapons
	3. Weight for equipment
	4. Monster CR for druids
	5. Casting time
	6. Range
	7. Duration
	8. Area of effect
2. Equivalence query for string fields and boolean fields
	1. Names of anything
	2. If a spell is a ritual
	3. If a spell is concentration
	4. Damage types
3. Base stats and stat modifiers
4. Full text search
	1. This would likely be the most useful with descriptions, but could have use finding certain spells or materials if you aren't sure of something

# Design Document Disclaimer
I have intentionally written this design doc as a fairly barebones version, because development in the first stage will primarily take place on my local environment. Eventually, when I plan to open this up to users, I will do a much more thorough design on authentication, data persistence and handling, data security, and scaling. However, I need to have a working frontend and backend as a POC before I get to that point.

# Next Steps
Rather than design in depth until I have what I feel is the "best solution", I am going to start with the basics I have and start coding the bits and pieces I have already identifed as requirements. If I start small, I can always find new ways to fit the pieces together.

Again, as I mentioned above, I am doing my best to get out of my own way on this. I recently read an article on the concept of ["Question-Driven Development"](https://nickjanetakis.com/blog/learning-a-new-web-framework-with-question-driven-development). Namely, it involves starting with one small problem and going until you hit a block. Ask how to solve that block, then go until you hit the next block, and  so on. It has really stuck with me, if only because I feel like I have been doing this subconsciously throughout my technical career - do what I can. Get an error. Figure out how to solve that error. Learn a ton of other things on the way.

I'm clearly far from unique with that approach. But reading the article made me realize how much I rely on that. As I have mentioned in [other articles](link to perfect as enemy), I have a longstanding problem of unreasonable expectations for myself. Figure out one problem, not the entire project.
