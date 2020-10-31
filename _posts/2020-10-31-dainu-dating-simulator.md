---
layout: post
title: Dainu Dating Simulator
date:   2020-10-31 12:00:00
categories: game-jams unity
---

[**Download link**](https://drive.google.com/file/d/10j7YWKGPpwNfXBRJPUA0m_shptktaWvN/view?usp=sharing)

PUND Studios presents - Dainu Dating Simulator!

![title](/static/img/DainuDatingSim/title.png)

<blockquote>
Find the dainu of your dreams with our hottest new dating app! Pay attention, because you'll need to have some ~wily empathy~ in order to court the dainu(s) of your dreams. Can you date all the dainus before the meteors hit?
</blockquote>

This game was the result of a [**game jam**](https://en.wikipedia.org/wiki/Game_jam) that some housemates and I worked on in November 2019. We brainstormed and considered a variety of ideas around a single randomly-chosen constraint - that the game needed to have something to do with dinosaurs. 

As you can see, we chose the best possible outcome. 

![conversation](/static/img/DainuDatingSim/tracyConversation.png)

## The design

There were two main things we wanted our game to have. 
<ul>
<li>A dating-app-style swipe right/left interface where the player can read through dainu biographies and choose whether to try to date them</li>
<li>A visual-novel-style date segment where the player goes out to a <em>swanky</em> restaurant with a dainu they matched with</li>
</ul>

The dating portion of the game would feature a number of conversational decisions the player needed to make based on information they gathered from previous parts of the date and from the dainu's biography. Don't remember how Tim likes his coffee? Sorry bucko, looks like you failed the date. 

![coffee](/static/img/DainuDatingSim/theodoreCoffee.png)

In order to make the game more fast-paced, we introduced a time mechanic - a meteoric one, at that. 

<video class="center-block" width="100%" height="auto" controls="controls">
  <source src="/static/img/DainuDatingSim/timer.mp4" type="video/mp4">
</video>

## The work

Our team had six members on it with a variety of skills, and most member had not participated in a game jam before. One of the core ideas we had in mind when picking the kind of game we wanted to make was to choose something that everyone could have a big role in helping to create. Dainu Dating Simulator fit well with this goal - in addition to development and art, it required a healthy amount of writing for all the characters. We also invested a good amount of time into the audio, recording and editing emotive sounds for the prominent dainus.  

<video class="center-block" width="100%" height="auto" controls="controls">
  <source src="/static/img/DainuDatingSim/reaction.mp4" type="video/mp4">
</video>

The result ended up being a fairly good balance of things to work on. 

We used Unity as our engine, which ended up being a cool opportunity for the other developers to learn how it worked. We were able to split up the work into a few different areas to avoid bumping into each other:
<ul>
<li>Backend logic for organizing and serving profiles during the dating app portion of the game</li>
<li>Creating a system to easily configure dainu profiles/dialog and another system for displaying them in the game</li>
<li>Animation and UI work</li>
<li>Countless miscellaneous tasks, like creating the game timer</li>
</ul>

The tasks I worked on had a good mixture of backend and frontend work. I handled most of the profile configuration system and display logic and that ended up being a nice opportunity to better familiarize myself with Unity's concept of ScriptableObjects. Take a look at this simple ScriptableObject script `DainuData.cs`: 

{% highlight csharp %}
public class DainuData : ScriptableObject {
    public enum DateType {
        Match, 
        NotMatch
    }

    // UID
    public int id;

    // Display name, occupation, and location
    public string dainuName;
    public string dainuJob;
    public string dainuLocation;

    // Which type of dainu this is (whether this can result in a date)
    public DateType dateType;

    // Main image for dainu
    public Sprite profileImage;

    public Sprite fullBodyImage;

    // Complete biography for dainu
    public string bio;

    // This represents a sequence of BOTH the main character's lines AND the dinosaur's lines during dates
    public List<DialogEvent> dateDialogEvents;

    // The "game over" string for when the date sucks
    public string dateBadEnd;
}
{% endhighlight %}

This allows us to serialize instances of `DainuData` instances and configure their fields via the Unity editor. 

![dainuconfig](/static/img/DainuDatingSim/dainuConfig.png)

With this system, non-developers were able to add profile information, images, dialog, and audio into instances of `DainuData` in order to include dainus into the game. Hurray for accessibility! 

I was also able to work on creating animations to add some movement into the game. I had not had all that much experience using Unity's animator, so I was able to practice that quite a bit. I found that keyframe animation in Unity was quite intuitive, but ran into a bit of trouble setting up the animation flow using the Animator tool. Still, since we budgeted a lot of time for me to work on this, I was able to get some simple animations going. 

<video class="center-block" width="100%" height="auto" controls="controls">
  <source src="/static/img/DainuDatingSim/animations.mp4" type="video/mp4">
</video>


This project captured one of the coolest moments you can find in a game jam - as the weekend was winding down and it became clear that we did not have enough time to include everything we wanted, all of us agreed to pick a few more days in the coming weeks to meet back up and finish the project. After much animation polishing and dialog tweaking, we had our finished game, which meant that we could begin bugging all of our friends to play it. 

## The best dainu

It's Kim.

![dainuconfig](/static/img/DainuDatingSim/kimConversation.png)

## Download link

[**Try the game out!**](https://drive.google.com/file/d/10j7YWKGPpwNfXBRJPUA0m_shptktaWvN/view?usp=sharing) 