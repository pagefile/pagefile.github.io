---
layout: post
title: "Making Levels"
date: 2018-4-5 0:17:00 -0500
---

Using strings to load scenes is icky. Sure, I can expose the string to the inspector and type it in, but what if I have multiple assets loading the same scene? Then I have to be sure to change them all. It's also easy to type the name wrong and break something. To make things easier and cleaner, I made my own level loading class that uses ScriptableObjects to load scenes. It lets me reference a scene by name in one spot no matter how many assets need it, and lets me just drag it into the inspector like any other asset. It's very handy of handling levels in Unity.

![Level asset in project view](/files/img/SO_Level.jpg)

Another handy use is that I can customize level loading. Right now I'm using the first scene I made as a partial level since it's going to be a structure that most levels will have anyway. I could make it a prefab, but since it's part of the level design and not an object that is used as part of gameplay (like an enemy ship or projectil) it makes more sense to have it as a scene. With this set up I can avoid duplicating data across scenes that might have the same or similar designs, like skybox geometry.

![Level asset assigned in Inspector](/files/img/SO_Level_Inspector.jpg)

For this game, partial levels are probably mostly going to be used to seperate the mechanical and visual aspects of a level. I'm doing it this way because I want more than just a vertical scrolling background like older shooter's like this would have. There's various ways to solve the problem, but many of them have a pretty big flaw in one way or another. In the end, I decided I'd have two cameras. One camera is over the player and enemies. This is where the action happens. Another camera is over the level geometry. A landscape, a city, a space ship, or maybe a moon. It travels along a path to simulate the player flying through a level. The player and enemies actually only exist in a small area in the game space. The level is sort of being rendered behind them as a sort of moving skybox. This means I don't have to worry about parenting ships, projectils, and particle effects to the camera, or worry about lining enemies up where the player can hit them when the camera zig zags around a level. Here's how it looks in game right now:

{% include video.html file="/files/videos/SimpleGameplay.mp4" %}

And here is how it looks while playing in scene view

{% include video.html file="/files/videos/shooterSceneView.mp4" %}

With Unity's layers, I can keep both scenes seperate even when they're really right on top of each other. I don't really have a good way of automating the layers right now, so everything has to be manually applied to the prefabs and GameObjects, but I might be able to find a solution later.