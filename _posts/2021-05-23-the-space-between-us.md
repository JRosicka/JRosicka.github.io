---
layout: post
title: The Space Between Us
date:   2021-05-23 16:00:00
categories: game-jams unity
---

![Title screen](/static/img/TSBU/titleScreen.PNG)

[**Download link**](https://jam-a-llamas.itch.io/the-space-between-us)

*The Space Between Us* is a game I helped make over the course of 72 hours for the [**Ludum Dare 48**](https://ldjam.com/events/ludum-dare/48) game jam. There were 14 people on our team, making this the largest game jam that I’ve worked on.

## The Premise

After hashing out a number of ideas, we settled on our game. Thematically, the player plays as the captain of a ~~spaceship~~ ~~drill~~ space-drill, running around his ship and frantically repairing devices as they constantly break. 
As the player does this, conversations between the captain and the ship’s cadet play out. The tone is overall comedic, both in the conversations and the set-design. 

<video class="center-block" width="100%" height="auto" controls="controls">
  <source src="/static/img/TSBU/bringingUpMinigame.mp4" type="video/mp4">
</video>
<br/><br/>
As for gameplay, the overall loop focuses on a sort of triage - the player needs to run from device to device, completing various minigames to fix the devices as they break. Think *Warioware* meets *Overcooked* with some obligatory *Among Us* elements. 

## The Team and Division of Labor

As mentioned, our team consisted of 14 (!!!!) people with backgrounds ranging across software development, 2D art, 3D art, music production, and project management. Additionally, there were some extra roles to be 
filled such as dialog writing and voice acting. I was one of the three software developers on the team. 

With such a large team, it was important that we stayed organized. Having a designated project manager role certainly helped, as did our pre-planning session we had the week before the jam. 
We had strong planning going in, and this helped immensely in pacing out our work over the course of the weekend. 

As for me and the other two developers, we did what we could to avoid the inevitable merge conflicts that seem to harrow every game jam. The main way we were able to steer clear of this was to delegate 
entire systems out to each developer so that there was as little crossover as possible. We had one developer work on the main game logic and all of the accompanying animations and particle effects, 
one work primarily on the dialog system, and me mostly work on the minigames. 

<video class="center-block" width="100%" height="auto" controls="controls">
  <source src="/static/img/TSBU/polish.mp4" type="video/mp4">
</video>
<br/><br/>
## Minigames

We made a total of 7 minigames to represent repairing broken devices. As the main developer working on them, it was my job to create a good variety of games that each felt sufficiently difficult, intuitive, and polished. 
And of course, I needed to work quickly so that I could give a good amount of time to each game. 

How did I do? It was somewhat of a mixed bag, but I think they turned out pretty good overall. 

![Engine repair minigame](/static/img/TSBU/engineRepair.PNG)

One of the main challenges as I was working on these was a design one - how to make the games interesting while still keeping them intuitive. The player needs to be able to recognize what they’re supposed 
to do immediately after seeing a minigame for the first time. I ended up providing instructions for each minigame to make the objective as clear as possible. 

We provided some visual indicators where helpful - making the fail-states and success-states as prominent as possible seemed important. Take the drill-repair minigame - a big shaky red animation plays when the player 
mistimes their click, and a happy green “you did it” animation plays when they pass it. Audio indicators were helpful as well, and our audio designer did a great job making a lot of little audio clips to put throughout the game. 

<video class="center-block" width="100%" height="auto" controls="controls">
  <source src="/static/img/TSBU/drill.mp4" type="video/mp4">
</video>
<br/><br/>
Though this visual and audio feedback is helpful, there is still a lot of room for improvement. Adding more indicators and clarification would go a long way - the oxygen minigame especially could use some work. 
In that minigame, the player needs to both balance out the oxygen levels between the tanks and use up all of the oxygen from the reserve meter. Though the instructions explain this, most new players 
do not immediately understand what’s going on. I don’t blame them though, since the reserve meter is unmarked and there is no indication about how close the levels need to be to each other. 
Little visual improvements like that would go a long way. 

![Oxygen repair minigame](/static/img/TSBU/oxygenRepair.PNG)

From the technical side of things, I also ran into issues with UI scaling. The minigames were entirely built in the UI by using the canvas, but I did not properly anchor some of the GameObjects for some of the minigames to 
scale properly on different aspect ratios. Attempts to improve this were pretty messy. Taking some time before the jam to learn more about how to properly manage canvases in Unity would have saved a lot of time in the long run. 

One last thing to note about minigames - there must be some sort of cosmic force out there that compels me to make bullet hell shmups. 

<video class="center-block" width="100%" height="auto" controls="controls">
  <source src="/static/img/TSBU/shmup.mp4" type="video/mp4">
</video>
<br/><br/>
Not that I’m complaining, since those are a ton of fun to make. 

## Feedback and Improvements

In the weeks following the conclusion of the project, we were able to get a ton of feedback from various playtesters. Players seemed to really enjoy the overall feel of the game and the level of polish, 
a sign that it was a great idea to invest as much as we did into the secondary elements of the game. We also got a lot of great feedback about things to improve on: 

* Pacing and dialog improvements: Dialog lines would interrupt each other if one started playing before the previous one finished. In response, we patched the game to prioritize each dialog line so that lines of higher priority did not become interrupted by lines of lower priority. 
* Minigame improvements: Some minigames were not intuitive enough to new players, as discussed earlier. 
* First-time experience: At the very beginning of the game, it wasn’t especially clear to players about where they should go or what they should be doing. We improved this first-time experience by starting the game with a single close-by broken device for them to fix and with a pathway of unlocked doors leading to the device. We also added a minimap and an indicator around the player pointing to broken devices. Still, the actual core gameplay loop is often unclear - players need to be taught that the game is about running around the ship and fixing devices. The best thing we could do here is to add a more clear-cut tutorial, perhaps even creating a small tutorial level where we explain to the player that devices will be breaking all the time and that it is their job to fix them. 

![Minimap](/static/img/TSBU/minimap.PNG)

To summarize, the most important elements that were missing from the initial release of our game centered around tutorialization. It seemed like players generally had a great time at their second or 
third playthroughs of the game, but it would have been great to have that happen at players’ *first* playthroughs. The target audience of this game jam was important to keep in mind for this too - most of our players 
are those browsing through a ton of games on the LDJam site, so we shouldn’t expect that second or third playthrough to necessarily even happen for any given player. 

## Final Score

*The Space Between Us* scored #3 overall out of 2721 entries in the Jam category, and it scored #1 in art and #1 in sound! This was a very pleasant surprise to see - we felt like the final game was great, 
but weren’t expecting anything like this. Our team was very talented and put in some great work, so it’s awesome to see so many people enjoy it. 

![Score](/static/img/TSBU/score.PNG)

You can download *The Space Between Us* [**here**](https://jam-a-llamas.itch.io/the-space-between-us)!
