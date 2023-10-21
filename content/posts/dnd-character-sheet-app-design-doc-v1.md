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
I will create a web app with a Go backend and a Svelte frontend. I'm picking Go because I really want to learn it, and Svelte for no particular reason than Fireship.io likes it and it seems to have a pretty solid community I can learn from. The primary focus at first will be the backend, so a more complete design of the frontend is coming later. I would also eventually like to look at using [Wails](https://wails.io/) or [Tauri](https://tauri.app/) (if it opens up to more backend languages) to bundle a desktop app as well with a local-first approach.

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
2. equivalence query for string fields and boolean fields
	1. Names of anything
	2. If a spell is a ritual
	3. If a spell is concentration
	4. Damage types
3. Base stats and stat modifiers
4. Full text search
	1. This would likely be the most useful with descriptions, but could have use finding certain spells or materials if you aren't sure of something

# Design Document Disclaimer
I have intentionally written this design doc as a fairly barebones version, because development in the first stage will primarily take place on my local environment. Eventually, when I plan to open this up to users, I will do a much more thorough design on authentication, data persistence and handling, data security, and scaling. However, I need to have a working frontend and backend as a POC before I get to that point. I love over-engineering as much as the next guy, but if I try to get the perfect design for all of those concerns now, I'll never start work.
