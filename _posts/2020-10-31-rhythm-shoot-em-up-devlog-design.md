---
layout: post
title: Rhythm Shmup Devlog #1 - Design
date:   2020-10-31 12:00:00
categories: unity
---

This will be the first in a series of posts diving into the development process of 
a game I've been working on prototyping - a genre mashup of 
[**bullet hell shoot-em-ups (bullet hells)**](https://tvtropes.org/pmwiki/pmwiki.php/Main/BulletHell) 
and [**rhythm games**](https://en.wikipedia.org/wiki/Rhythm_game). 

This first post will focus on the design for this project. 

## Background

When it comes to making groundbreaking new ideas for games, there is a lot of potential in 
genre mashups. I believe that game developers can get a lot of mileage out of combining 
together game genres that already exist rather than necessarily trying to create a brand 
new genre from scratch. So, when I try to think of innovative new game ideas, I try to focus 
less on the question of "what's a type of game that no one has created before?" and more on the 
question "what's a *combination* of games that no one has *combined* before?"

[**Slay the Spire**](https://en.wikipedia.org/wiki/Slay_the_Spire) is a perfect example of this, 
a roguelike-deckbuilder hybrid that paces itself to match the progression of any 
typical roguelike (advancing to more difficult areas and simultaneously collecting more powerful items) 
and any typical deckbuilder (building a more powerful, synergistic deck over the course of the game) 
in a complementary way. Neither of these individual elements are radically re-imagined in Slay the Spire, but 
the *combination* of the two are what makes the game stand out. 

Now, slapping any two game genres together can pretty reliably make a game appear fresh and new, but 
this doesn't automatically make the game's design a good one. I imagine that something like a racing-horror 
game or a trivia-platformer game would be... difficult to pull off. Though who knows, maybe these examples 
have some hidden untapped potential. 

## Bullet hell - rhythm game hybrid

This particular genre mashup is one I've had in my head for a while, and I think the biggest reason 
it would work so well is because both types of games capture a similar *fantasy* - a specific feeling or 
gameplay experience that typical games in these genres attempt to evoke. 

There are a lot of elements of these two genres that share the same *fantasy*: 
* Both tend to have very fast-paced, high-intensity gameplay focused on there being *a lot* of things on the screen to dodge (in the case of bullet hells) or hit (in the case of rhythm games). 
* The game looks visually impressive with the screen often being full of projectiles/notes. This both gives the player a huge feeling of satisfaction when they're able to clear a difficult section of the game, and also looks visually impressive to spectators - "Woah, how did they do that? There's so much going on at once!"
* Dodging/hitting bullets/notes requires good reaction time and a lot of practice for particularly difficult sections. The key to success often lies in being able to sort through the visual noise on the screen and determine what you *actually* need to focus on at any given time. 
* Playing the game at one's highest level requires them to enter into a sort of [**flow state**](https://www.headspace.com/articles/flow-state) in which they are able to react to things happening on the screen seemingly without having to think. For me, this is the most satisfying part of both kinds of games. 

At their core, these two genres seem like a perfect fit. 

There are a handful of examples of these two genres being combined, the most prominent of which is likely 
[**Just Shapes & Beats**](https://www.youtube.com/watch?v=aEGVEr4_3kI). This game seems to appeal to
the above elements nicely, providing a fast-paced dodging experience in which every aspect of the game's
bullet patterns and animations in on-beat with the soundtrack. This shoud serve as a good model for further exploring
a mashup between bullet hells and rhythm games. 

There are a number of design considerations to make which I'll go into more detail about in future posts,
but my high level design goal is to have my prototype appeal to this fantasy I've outlined. 

## Prototype work

I chose to use the Unity engine to create a prototype, and will be going forward with using it if I end
up creating a larger project based on this idea. The scope of the prototype is a ~30 second experience focused
on dodging basic patterns. 

The real meat of this prototype lies in the backend systems, and this is what I've been focused on for
most of its development. There are two particular elements of this backend framework which I've
been working on:
* A pattern configuration system to set up how and when enemies spawn, move, and fire patterns. The "when" of this is important since I need to be able to easily synchronize the game logic to the timing of the soundtrack. 
* A system for creating individual bullet patterns. I've chosen to use [**Unity's particle system**](https://docs.unity3d.com/ScriptReference/ParticleSystem.html) to create bullets since it has a strong system for configuring and visualizing particles. However, a good amount of work has needed to go into making this usable for configuring patterns in a fast and reliable manner.

These elements contain the majority of this prototype's complexity and probably both warrant their own post at some point. 

Currently, the prototype is mostly playable, though I'll want to finish making improvements to the  
framework before configuring more of the front-end. Here's a snapshot of what I have so far: 

<video class="center-block" width="100%" height="auto" controls="controls">
  <source src="/static/img/FitD/basicGameplay.mp4" type="video/mp4">
</video>

I'll be excited to see what I can do with this prototype once the framework is in a more complete
spot. Stay tuned for updates!
