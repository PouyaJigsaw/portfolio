---
title: Dynamic Difficulty in Video Games
author: Pouya Pournasir
pubDate: 2021-12-12 12:00
tags:
  - User Experience
  - Unity
  - User Testing
  - Interview
description: Researched about psychological aspects of video games for optimized game design. Programmed an innovative dynamic difficulty system to maximize user’s enjoyment.
imgUrl: '../../assets/difficulty.png'
layout: "../../layouts/BlogPost.astro"
---



The goal of this study is to find out the impact of the two approaches that is used in implementing dynamic difficulty on player’s overall experience: One where the dynamic difficulty is implemented, and it does not tell the player about the changes - Automatic dynamic difficuly system. The second approach is that the dynamic difficulty is implemented but while it is trying to change the difficulty it asks the player whether they would like to increase or decrease difficulty, giving players choice - choice based dynamic difficulty system. In this paper we call the first approach automatic dynamic difficulty system and the second one, choice-based dynamic difficulty system.

Players engage with two challenge-based platformer games, referred to as "Microgames." Each round of these short games lasts approximately 20 seconds to 1 minute. Players are instructed to play each microgame for 5 minutes, repeating levels multiple times. In the automatic DDS condition, difficulty adjusts subtly after each round based on the player's performance—increasing after a loss and decreasing after a win. This gradual tuning occurs without explicit communication to the player. In the choice-based DDS condition, the difficulty changes after three consecutive wins or losses. A screen appears, allowing players to choose whether they want to increase or decrease the difficulty. If a player experiences three consecutive deaths, the "Easier" screen appears, offering the option to decrease difficulty by 3 points for a noticeable change. Similarly, winning three times in a row prompts the "Harder" screen, allowing players to increase difficulty by 3 points.

The microgames has been created based on the Unity LEGO asset, which was provided by unity as game templates. In the first microgame, which is called “Evil Machine”, players have to pick up the five collectibles that are on the platform. The problem is that there are two deadly arm machines; rotating in the opposite direction of each other, and players have to figure a way to avoid them and collect the pickups. After collecting those pick-ups, the players win the round. In the “Pandamonium” microgame, there is a big panda waiting for the player’s arrival at the main stage and shooting red laser beams at them while they are at the stage. The goal is to collect a certain number of fruits, each fruit stuck in the trees scattered in the field. Players have to lure the panda into shooting laser beams at the trees and collect the fruits after the trees are destroyed. After collecting them, the player wins a round.

The impact of difficulty and its different implementations have been discussed in the literature numerous times. We believe this work can shed some light on how people react to dynamic difficulty systems and its different variations.
